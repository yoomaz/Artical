#### 命令行：

1. 首先进入 openssl 
2. 生成私钥
3. 把私钥转化成 PKCS8 格式
4. 根据私钥生成公钥
5. 退出 openssl

注意：

1. java开发需要把撕咬转换成 PKCS8 格式
2. 查看私钥公钥内容可以使用 cat 命令

```shell
$ openssl # 进入 OpenSSL 程序
OpenSSL> genrsa -out rsa_private_key.pem 1024 # 生成私钥
OpenSSL> pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt -out rsa_private_key_pkcs8.pem # Java开发者需要将私钥转换成 PKCS8 格式
OpenSSL> rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem # 生成公钥
OpenSSL> exit  # 退出 OpenSSL 程序
```

