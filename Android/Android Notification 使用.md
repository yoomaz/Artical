![notifications_anatomy_02_content.png](http://upload-images.jianshu.io/upload_images/646076-1b334f92e80bf909.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通知必须的元素：
```  java
//  系统状态栏显示的小图标
builder.setSmallIcon(R.mipmap.ic_launcher); // 必须  
//  通知标题栏
builder.setContentTitle("title"); // 必须
//  通知内容栏        
builder.setContentText("content"); // 必须     
```
其他元素：
```  java
NotificationCompat.Builder builder = new NotificationCompat.Builder(this);        //系统状态栏显示的小图标，必须设置，否则报错        builder.setSmallIcon(R.mipmap.ic_launcher); // 必须        builder.setContentTitle("title"); // 必须        
builder.setContentText("content"); // 必须        
builder.setSubText("subtext");       
 // 点击通知，通知是否消失
builder.setAutoCancel(false);        
// 滑动通知，通知不会消失 true:不消失，false：消失        
builder.setOngoing(false);        
// 左上角是否显示时间       
builder.setShowWhen(false);        
builder.setNumber(20);        
// 下拉的时候显示的大图标
builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher));       
 //通知默认的声音 震动 呼吸灯
builder.setDefaults(NotificationCompat.DEFAULT_ALL);       
 // 设置声音
//builder.setSound()
//  设置通知等级，一共五级        
builder.setPriority(2);
// 设置点击通知时候的动作
Intent intent = new Intent(this, SecondActivity.class);        
// 1. 通知和 TaskStackBuilder 一起使用，暂时不是特别了解
//        TaskStackBuilder taskStackBuilder = TaskStackBuilder.create(this);
//        taskStackBuilder.addParentStack(MainActivity.class);
//        taskStackBuilder.addNextIntent(intent);
//        PendingIntent pIntent = taskStackBuilder.getPendingIntent(0, PendingIntent.FLAG_NO_CREATE); 
// 2.设置详细扩展显示时候的样式
//        NotificationCompat.InboxStyle inboxStyle = new NotificationCompat.InboxStyle();
//       
 // 设置头部
//        inboxStyle.setBigContentTitle("Event tracker details:");
//        
// 设置详细
//        for (int i=0; i < 6; i++) {
//            inboxStyle.addLine(i+"");
////        }
//       
 // Moves the expanded layout object into the notification object.//        builder.setStyle(inboxStyle); 
       // PendingIntent.FLAG_ONE_SHOT: 只能点击一次，再点击不创建 activity
       // 0：可以不断点击，点击一次创建一个新的activity，可返回
PendingIntent pIntent = PendingIntent.getActivity(this, 0, intent, 0);        builder.setContentIntent(pIntent); 
Notification notification = builder.build();       
manager.notify(TYPE_Normal, notification);
```

Android 官方文档链接：
https://developer.android.com/guide/topics/ui/notifiers/notifications.html
夸一下谷歌的国际化还是做得不错的，已经有中文版本了