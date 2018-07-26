1. android 安装包必须要签名才可以安装运行，测试的时候如果没有指定签名，会使用默认签名。mac 的默认签名在  `~/.android/debug.keystore` , win 的系统也在相应的 android 目录下

2. 签名内包含很多信息，查看签名的信息可以使用下面这个命令，需要输入签名密码，默认的 debug 签名密码是：`android` , 有的开放平台需要签名的 sha-1 值，例如高德地图的定位服务

   ```shell
   $ keytool -list -v -keystore [具体的签名文件]

   密钥库类型: JKS
   密钥库提供方: SUN

   您的密钥库包含 1 个条目

   别名: androiddebugkey
   创建日期: 2015-7-7
   条目类型: PrivateKeyEntry
   证书链长度: 1
   证书[1]:
   所有者: CN=Android Debug, O=Android, C=US
   发布者: CN=Android Debug, O=Android, C=US
   序列号: 559bd50f
   有效期开始日期: Tue Jul 07 21:33:03 CST 2015, 截止日期: Thu Jun 29 21:33:03 CST 2045
   证书指纹:
   	 MD5: F9:BF:FA:E2:31:BB:D4:E1:B2:5F:B5:23:00:F8:BE:CD
   	 SHA1: DF:CC:AC:58:62:EB:A3:D5:21:C8:AA:4F:3F:7B:0C:E7:8A:0C:9D:87
   	 SHA256: C3:10:FE:25:7F:A8:AE:48:BD:0D:48:DF:B8:88:48:07:C2:57:18:0C:EF:F9:55:BE:4C:3A:F9:1B:80:CD:B6:2C
   	 签名算法名称: SHA1withRSA
   	 版本: 3


   *******************************************
   *******************************************
   ```

3. 一个签名主要包含三个信息：`keystore 密码` , `alias`  , `alias密码`

4. 签名 keystore 文件很容易丢失，最好放在项目里一起 git 上传了，同时建一个 md 文件写上相关的密码