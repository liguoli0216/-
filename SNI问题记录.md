#背景
开发反馈某服务从阿里云slb迁移到内部的nginx代理后，部分java程序无法通过https访问此服务

#新旧环境对比
旧：阿里云slb采用https监听配置https证书，将请求直接转发到realserver
新：阿里云slb采用tcp监听，将请求转发到内部代理集群，内部代理集群配置https证书

#猜测1
内部代理集群的https证书不完整，没有将中级证书合并

#猜测2
无法通过https访问的程序httpclient jar包有问题

#验证1
经过md5对比，发现证书文件内容一致，排除证书文件问题

#验证2
经过和开发沟通，发现不能通过https调用服务的httpclient版本为4.3.4

#结论
经过上述对比，可以不负责任的告诉开发，”你们使用的jar包有问题，自己想办法解决！！！“

#由于之前并没有接触过此类问题，所以只能先进行访问测试
#1 curl访问此服务正常
#2 python访问此服务https正常
#3 java访问此服务报错证书不可信，并且提示的证书并不是此服务配置的证书

#为何java访问的时候提示的不可信证书不是我们配置的？万事不定先抓包(此时的我还不知道SNI)

![Image](https://github.com/liguoli0216/Nginx/blob/master/SNI/httpclient-4.3.4.png)
