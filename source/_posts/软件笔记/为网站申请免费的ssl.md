---
title: 为网站申请免费的ssl证书
date: 2024-12-27 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
  - ssl
---



1. 打开fressssl.cn

   https://freessl.cn/

2. 进入控制台，点击证书自动化

   https://freessl.cn/chart

   ![image-20241227164049693](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227164049693.png)

3. 点击域名授权，添加域名，如果有多个可以添加多个，重复此步骤即可

   ![image-20241227170200615](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227170200615.png)

4. 添加域名后，还需要到dns域名解析配置响应的DCV, 如阿里云的dns域名配置，填入步骤3所示的主机记录和记录值

   ![image-20241227164438454](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227164438454.png)

5. 点击ACME客户端，申请证书

   ![image-20241227164621669](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227164621669.png)

6. 复制ACME.sh命令，到服务端执行

   ![image-20241227164802715](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227164802715.png)

   > 前提要先按照ACME.sh命令，参考文档：https://docs.certcloud.cn/docs/edupki/acme/

   执行成功后，默认会在~/.acme.sh/目录下生成对应域名的目录下

   ![image-20241227165057177](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20241227165057177.png)

7. 部署并开启自动续费和证书更新，acme.sh内部会开启对应crontab自动check，证书进入到30天有效期，acme.sh 会自动完成续期。

   **Apache example**

   ```bash
   acme.sh --install-cert -d /root/.acme.sh/pen.okeeper.com/ \
   			--cert-file      /path/to/certfile/in/apache/cert.pem  \
   			--key-file       /path/to/keyfile/in/apache/key.pem  \
   			--fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \	
   			--reloadcmd     "service apache2 force-reload" 
   ```

   **Nginx example**

   ```bash
   acme.sh --install-cert -d example.com \
   	--key-file       /path/to/keyfile/in/nginx/key.pem  \
   	--fullchain-file /path/to/fullchain/nginx/cert.pem \
   	--reloadcmd     "service nginx force-reload"
   ```

   上述命令中：

   ​	•	--key-file 指定私钥的保存路径。

   ​	•	--fullchain-file 指定完整证书链的保存路径（包括服务器证书和中间证书）。

   ​	•	--reloadcmd 用于指定证书更新后需要执行的命令，例如重新加载 Nginx 配置。

8. 到此整个过程就完成了，最后介绍一种为了取巧将步骤6和7合并的一种命令，如下
   ```
   acme.sh --issue -d pen.okeeper.com -d a2v.okeeper.com -d model-bridge.okeeper.com --dns dns_dp --server https://acme.freessl.cn/v2/DV90/directory/xxxxxxxxxxxxx \
   	--key-file       /data/nginx/cert/key.pem  \
   	--fullchain-file /data/nginx/cert/cert.pem \
   	--reloadcmd     "docker restart my-nginx"
   ```

   此命令可以直接在服务器端生成对应的证书，并安装到指定的文件目录中，便于后续的nginx.conf的配置

   ```sh
   http {
   
       # 配置 SSL
       ssl_certificate /etc/nginx/cert/cert.pem;
       ssl_certificate_key /etc/nginx/cert/key.pem;
   #    ssl_protocols TLSv1.2 TLSv1.3;
   #    ssl_ciphers HIGH:!aNULL:!MD5;
   #    ssl_prefer_server_ciphers on;
   }
   ```

   

