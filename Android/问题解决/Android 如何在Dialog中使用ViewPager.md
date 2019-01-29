# Android 如何在Dialog中使用ViewPager

### 遇到的问题

最近有这样一个普通的需求，viewpager(Fragment) + 一个tab，但是与往常不一样的是，以前是在 Activity 中创建很正常，这次是在一个 Dialog，写完一运行，出现了 :

`java.lang.IllegalArgumentException：No view found for id 0x7f10013 for fragment `

提示中不到 ViewPager 的 id，而且位置是在 Fragment 中，很奇怪

### 原因

我们创建 PagerAdapter 的时候，传入了一个 FragmentManager，我们一般是传入 `getSupportFragmentManager()` ,是 Activity 的 FragmentManger。

如果传入的是 Activity 的 FragmentManger，默认在 Activity 的布局 xml 中寻找ViewPager，但是实际上它是在弹出的View里定义的，并不是在 activity 的布局里，所以出现找不到资源id的情况

同理，如果我们是在 Fragment 里使用 viewpager 嵌套 Fragment，创建 PagerAdapter 时需要使用 `getChildFragmentManager()`

### 解决办法

使用 DialogFragment 来代替 Dialog，同时创建 PagerAdapter 时需要使用 `getChildFragmentManager()，完美解决, 这里贴一个 demo 代码

```java
public class BuyerLiveGoodsDialog extends DialogFragment {

    private XTabLayout tabLayout;
    private ViewPager viewPager;
    private ImageView ivClose;
    private int height;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
        height = (int) (Tools.getScreenHeight(getContext()) * 0.8);

        getDialog().requestWindowFeature(Window.FEATURE_NO_TITLE);
        View view = inflater.inflate(R.layout.dialog_bottom_buyer_live_goods, container, false);
        tabLayout = view.findViewById(R.id.tab_layout);
        viewPager = view.findViewById(R.id.view_pager);
        ivClose = view.findViewById(R.id.iv_cancel);
        ivClose.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                dismiss();
            }
        });

        BuyerLiveGoodsPageAdapter pageAdapter = new BuyerLiveGoodsPageAdapter(getChildFragmentManager());
        viewPager.setAdapter(pageAdapter);
        tabLayout.setupWithViewPager(viewPager);

        return view;
    }

    @Override
    public void onStart() {
        super.onStart();

        Window window = getDialog().getWindow();
        if (window != null) {
            // 一定要设置Background，如果不设置，window属性设置无效
            window.setBackgroundDrawable(new ColorDrawable(getResources().getColor(R.color.text_color)));
            DisplayMetrics dm = new DisplayMetrics();
            if (getActivity() != null) {
                WindowManager windowManager = getActivity().getWindowManager();
                if (windowManager != null) {
                    windowManager.getDefaultDisplay().getMetrics(dm);
                    WindowManager.LayoutParams params = window.getAttributes();
                    params.gravity = Gravity.BOTTOM;
                    // 使用ViewGroup.LayoutParams，以便Dialog 宽度充满整个屏幕
                    params.width = ViewGroup.LayoutParams.MATCH_PARENT;
                    params.height = height;
                    window.setAttributes(params);
                }
            }
        }
    }

    static class BuyerLiveGoodsPageAdapter extends FragmentPagerAdapter {

        List<Fragment> fragments = new ArrayList<>();
        List<String> titles = new ArrayList<>();

        public BuyerLiveGoodsPageAdapter(FragmentManager fm) {
            super(fm);

            // 一口价
            BuyerLiveOnePriceFragment onePriceFragment = new BuyerLiveOnePriceFragment();
            fragments.add(onePriceFragment);
            titles.add("一口价");
            // 拍卖
            BuyerLiveAuctionFragment auctionFragment = new BuyerLiveAuctionFragment();
            fragments.add(auctionFragment);
            titles.add("拍卖");

            notifyDataSetChanged();
        }

        @Override
        public Fragment getItem(int position) {
            return fragments.get(position);
        }

        @Nullable
        @Override
        public CharSequence getPageTitle(int position) {
            return titles.get(position);
        }

        @Override
        public int getCount() {
            return fragments.size();
        }
    }

    public static void showDialog(FragmentManager fragmentManager) {
        BuyerLiveGoodsDialog dialog = new BuyerLiveGoodsDialog();
        dialog.show(fragmentManager, "tag");
    }


}
```

### 源码阅读

首先看 FragmentPagerAdapter 的 instantiateItem 方法，把 ViewGroup 的 id 传入：

```
@SuppressWarnings("ReferenceEquality")
    @Override
    public Object instantiateItem(ViewGroup container, int position) {
    	// 省略部分代码
        if (fragment != null) {
            if (DEBUG) Log.v(TAG, "Attaching item #" + itemId + ": f=" + fragment);
            mCurTransaction.attach(fragment);
        } else {
            fragment = getItem(position);
            if (DEBUG) Log.v(TAG, "Adding item #" + itemId + ": f=" + fragment);
            // 这里会往 FragmentManager 里 add 一个 fragment，传入了 container 的id，也就是 ViewGroup 的id
            mCurTransaction.add(container.getId(), fragment,
                    makeFragmentName(container.getId(), itemId));
        }

        return fragment;
    }
```

Fragment 是有状态的，定义在 Fragment 里：

```java
    static final int INITIALIZING = 0;     // Not yet created.
    static final int CREATED = 1;          // Created.
    static final int ACTIVITY_CREATED = 2; // The activity has finished its creation.
    static final int STOPPED = 3;          // Fully created, not started.
    static final int STARTED = 4;          // Created and started, not resumed.
    static final int RESUMED = 5;          // Created started and resumed.
    // 保存当前的状态
    int mState = INITIALIZING;
```

这些状态是由 FragmentManager 来管理的，通过方法 moveToState：

```java
void moveToState(Fragment f, int newState, int transit, int transitionStyle,
            boolean keepActive) {
        if (f.mState <= newState) {
            switch (f.mState) {
            	// 如果是已经创建的状态
                case Fragment.CREATED:
                     // 重点！！，这里通过 mContainer 的 onFindViewById 去找 ViewGroup，这个 mContainer 是 FragmentManager 所有者的布局！！
                                container = (ViewGroup) mContainer.onFindViewById(f.mContainerId);
                                if (container == null && !f.mRestored) {
                                    String resName;
                                    try {
                                        resName = f.getResources().getResourceName(f.mContainerId);
                                    } catch (NotFoundException e) {
                                        resName = "unknown";
                                    }
                                    throwException(new IllegalArgumentException(
                                            "No view found for id 0x"
                                            + Integer.toHexString(f.mContainerId) + " ("
                                            + resName
                                            + ") for fragment " + f));
                                }
                            }
            }
        }
    }
```

我们可以看到，` container = (ViewGroup) mContainer.onFindViewById(f.mContainerId);` 这里执行了一次`onFindViewById`的操作，也就是在这里报的 `IllegalArgumentException`

s