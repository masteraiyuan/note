## docker gitlab CI/CD

### 问题汇总

1. jenkins 需要设置关闭 防止跨站点请求伪造（CSRF），否则gitlab事件请求会被重置，报403；
2. 报错 Requests to the local network are not allowed

	> gitlab 10.6 版本以后为了安全，不允许向本地网络发送webhook请求，如果想向本地网络发送webhook请求，在Admin area中，在settings标签下面，找到OutBound Request，勾选上Allow requests to the local network from hooks and services ，保存更改即可解决问题
	
3. gilab 找不到 webHook

	> gitlab相对于 2018-10-11 后更新了位置，在  setting>>Integrations>>填写url
	
4. anonymous is missing the Job/Build permission （和第三点冲突，都试试）

	> 系统管理 -> 系统设置 -> 去掉 Enable authentication for ‘/project’ end-point 
	
	> 友情提示：不要去掉防止跨站点请求伪造

### gitlab

1. 利用docker安装gitlab

	```
	docker run --detach --hostname 10.10.0.23 --publish 445:443 --publish 8090:80 --publish 24:22 --name gitlab --restart always --volume /Users/aiyuan/Documents/study/gitlab/config:/etc/gitlab --volume /Users/aiyuan/Documents/study/gitlab/logs:/var/log/gitlab --volume /Users/aiyuan/Documents/study/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest
	```

### gitlab runner

1. 获取最新镜像

	```
	 docker pull gitlab/gitlab-runner:latest
	```
	
2. 创建容器

	```
	 docker run -d --name gitlab-runner --restart always \
	   -v /srv/gitlab-runner/config:/etc/gitlab-runner \
	   -v /var/run/docker.sock:/var/run/docker.sock \
	   gitlab/gitlab-runner:latest
	```
	
3. 启动runner

	```
	docker run -d --name gitlab-runner --restart always -v /Users/aiyuan/Documents/study/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock --add-host gitlab.aiyuan.com:10.10.0.23  gitlab/gitlab-runner:latest
	```
	
3. 注册runner

	```
	sudo docker exec -it gitlab-runner gitlab-ci-multi-runner register
	```
	
4. 将新启动的容器中的gitlab-runner用户加入root组以可以调用docker

	```
	docker exec -it gitlab-runner usermod -aG root gitlab-runner
	```
	
5. 注意：

	* centos下需要安装docker-engine而不是docker:

	```
	yum -y install docker-engine
	
	否则会报错：
	/usr/bin/docker: 2: .: Can't open /etc/sysconfig/docker
	```

	* 每次重启docker service的时候都需要重置docker.sock的所属组。。

	```
	service docker restart
	chown root:root /var/run/docker.sock
	
	使用如下方法可以解决这个问题：
	
	vi /usr/lib/systemd/system/docker.service
	更改以下这行，添加-G root，使其以root用户启动：
	
	ExecStart=/usr/bin/dockerd --registry-mirror=http://3cda3ca9.m.daocloud.io -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375 -G root
	此时/var/run/docker.sock即是root:root用户组了。
	```