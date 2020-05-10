# DockerDemo
1.Linux+aspnetcore+ngnix
2.Linux+Docker+aspnetcore+ngnix

花了一个周末时间,终于调通了,走了不少弯路

花时间最长的就是linux和docker+linux怎么让本机可以访问虚拟机的netcore的端口和docker  netcore的端口

早看到下边nginx这个就不至于浪费这么多时间


old nginx config

 ```js
  server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
   }
 ```

 new nginx config 
 
 ```js
server {
	listen 80;

	#root /usr/share/nginx/html;
	#index index.html index.htm;

	# Make site accessible from http://localhost/
	server_name localhost;

	location / {
		proxy_pass http://localhost:5000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection keep-alive;
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	}
}
```

~~~
配置 nginx 代理。

安装完 nginx 之后，默认的配置文件路径在 /etc/nginx/sites-available/default 文件中。切换工作目录到/etc/nginx/sites-available/，使用sudo gedit default命令打开 default 文件。 在 Server 节点中，找到监听 80端口的location 节点，修改内容为如下：

server {
	listen 80;

	#root /usr/share/nginx/html;
	#index index.html index.htm;

	# Make site accessible from http://localhost/
	server_name localhost;

	location / {
		proxy_pass http://localhost:5000;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection keep-alive;
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	}
}
保存并退出。 然后使用sudo nginx -s reload命令来重新加载配置。
然后我们打开浏览器 输入http://localhost，发现此时已经通过 nginx 来访问我们的站点了

~~~

参考资料:
[Asp.Net Core 发布和部署（ MacOS + Linux + Nginx ）](https://www.cnblogs.com/savorboard/p/dotnet-core-publish-nginx.html)
[ASP.NET Core Docker部署](https://www.cnblogs.com/savorboard/p/dotnetcore-docker.html)
[centos 7 + Net Core 3.0 + Docker 配置说明（不含https）](https://www.cnblogs.com/nickchou/p/11810938.html)