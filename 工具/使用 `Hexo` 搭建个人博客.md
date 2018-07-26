## 前言
在日常学习开发中通过博客记录总结是一个自我提升的好习惯，其他人也可以通过阅读你的博客学习到一些东西。相比老牌`WordPress`, 使用静态博客进行搭建的人越来越多,其中典型的就是 `Hexo`

## 安装 Node.js
进入 [Node 主页](https://nodejs.org/en/), 下载下来稳定版本安装包，直接安装。
使用 `node -v` 来查看版本号

## 安装 Hexo
直接命令 `npm install -g hexo-cli` ，等待一段时间即可，速度很慢，如果很久都没有安装完，可以先停止，开ss后再重新尝试。
使用 `hexo -v` 查看当前的版本号，这个时候可以
## 安装主题
Hexo 安装主题的方式非常简单，只需要将主题文件拷贝至站点目录的 themes 目录下， 然后修改下配置文件即可，我安装的是[Next主题](https://github.com/iissnan/hexo-theme-next)，看起来简洁大方。

在 Hexo 中有两份主要的配置文件，其名称都是 `_config.yml`。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 站点配置文件， 后者称为 主题配置文件。

1. 使用克隆的方式把主题拷贝下来：

    ```
    $ cd your-hexo-site
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next
    ```

2. 打开 站点配置文件， 找到 `theme ` 字段，并将其值更改为`next`

3. 其他详细配置，例如设置 **tags** 等可以查看[官方链接](http://theme-next.iissnan.com/getting-started.html)

4. 使用`hexo clean，hexo generate， hexo server ` ，然后打开 `http://localhost:4000/` 即可查看到更换主题后的站点

## 创建 Github Page
1. 在 **github** 上创建仓库，仓库名必须为 **[your_user_name.github.io]**，固定写法

2. 打开站点配置文件 `_config.yml` 找到 **deploy** 字段，配置如下：

    ```
       deploy:
           type: git
           repository: https://github.com/Graypn/graypn.github.io.git
           branch: master
    ```

3. 执行命令 `npm install hexo-deployer-git --save` 安装部署服务
4. 执行命令 `hexo deploy` ，就会自动部署到 github 上的仓库，然后就可以通过 `graypn.github.io` 进行访问，你也可以自己买域名，解析到这个网址，需要在 source 目录下 创建名字是 CNAME 文件，里面写入你的域名信息