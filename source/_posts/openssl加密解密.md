title: openssl加密解密
date: 2016-06-21 13:59:04
categories:
- 工具
- 开发工具
- openssl
tags:
- openssl
- 开发工具
- 加密解密
---
### 生成密钥和公钥
```
# 使用 RSA 生成1024位的私钥
#   -out: 指定生成的私钥的文件名
#   1024: 为密钥的长度
$ openssl genrsa -out rsa.private 1024
 
# 使用上面的私钥生成公钥
#   -in:  指定输入公钥的文件名
#   -out: 指定生成公钥的文件名
#   -outform: 指定生成PEM格式的公钥
$ openssl rsa -in rsa.private -out rsa.public -pubout -outform PEM
```

### 使用公钥加密，私钥解密
```
# 生成测试文件 test.txt
$ echo 'hello, world!' > test.txt
 
# 使用公钥加密文件: test.enc
#   -in:    指定被加密的文件
#   -inkey: 指定公钥文件
#   -pubin: 指定用纯公钥文件加密
#   -out:   输出加密后的文件
$ openssl rsautl -encrypt -in test.txt -inkey rsa.public -pubin -out test.enc
 
# 使用私钥解密: test.dec
#   -in:    指定被加密的文件
#   -inkey: 指定私钥文件
#   -out:   输出解密后的文件
$ openssl rsautl -decrypt -in test.enc -inkey rsa.private -out test.dec
```
### 使用私钥加密，公钥解密
```
# 生成测试文件 test.plain
$ echo 'hello, world!' > test.plain
 
openssl rsautl -inkey rsa.private -in test.plain -sign -out test.signed
openssl rsautl -inkey rsa.pub -pubin -in test.signed
```
