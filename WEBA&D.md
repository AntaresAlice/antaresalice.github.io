##运维加固手册
——Antares

###一、加固流程
---
####0.创造加固时间
没有保护时间的话，拼着服务宕机也要把防护先做好。
	白名单
####1.本地提权
如果未获得root权限
利用POC获取root

	工具：
		- CVE-2016-5195_dirtyc0w 
		- CVE-2016-8655_chocobo_root  
		- CVE-2017-6074_poc  
		- sudo-CVE-2017-1000367

####2.备份

- Docker备份
	
		如果题目在Docker上运行，且没有提供ssh的口令，则备份起来相对麻烦。
		
		容器保存为镜像
			docker commit nginx_container mynginx_image

		备份Docker镜像
			docker  save -o mynginx.tar mynginx_image

		镜像恢复
			先停止相应docker
				docker ps -a
				docker stop $CONTAINER_ID
			再删除镜像
				docker rmi <image id> 
			然后恢复镜像
				docker load -i mynginx.tar
			最后重新启动container
				docker run -it <image name:id>
		
		注：向docker中拷贝文件：
			docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-

- web目录备份
		
 		本地备份： tar -zcvf web.tar.gz /var/www/html/
        FileZilla备份
		基于时间备份，每10分钟一次

- Mysql备份

		备份mysql数据库
			mysqldump -u 用户名 -p 密码 数据库名 > back.sql　　//备份指定数据库
			mysqldump --all-databases > bak.sql　　　　//备份所有数据库
	
		还原mysql数据库
			mysql -u 用户名 -p 密码 数据库名 < bak.sql 		


		数据库降权


- 初始进程、端口、权限备份 
		
		查看当前连接网络的进程并备份：
			netstat -antulp | grep EST
			sudo netstat -antulp > network.txt 

		查看当前所有进程并备份：
			ps -aux > ps.txt

####3.启动防御机制

- 安装防火墙

		WAF、安全狗

- 启用通用管制

		./ctf-firewall.sh

- 添加.htaccess配置文件

		？？？


- 开启文件监控

		后台运行：
			./SimpleMonitor_64 &



- 启动log记录系统

####4.口令修改

- 改掉SSH弱口令

		SSH登录后，passwd修改密码
		
- 修改ssh端口（慎用）

		Redhat:
			vi /etc/ssh/ssh_config 
			vi /etc/ssh/sshd_config
			service sshd restart
		linux:
			/usr/sbin/sshd -p 1433


- web后台弱口令


###二、初期漏洞查杀及修补
---
####1.扫描漏洞

- D盾
- Seay
- shelldetector.com

####2.修补漏洞
- 将上传页面的action地址修改为*
- 修补反序列化和命令执行，在seebug等地找补丁



###三、安全检查
---
####1.用户检查
		
		awk -F: '{if($3==0)print $1}' /etc/passwd  // 检查root用户
		userdel  ???  // 删除异常用户

####2.进程检查
	
		ps -aux  //检查所有进程
		kill -9 ???  //删除异常进程

####3.网络检查

		检测所有的tcp连接数量及状态
			netstat -ant|awk '{print $5 "\t" $6}' |grep "[1-9][0-9]*\."|sed -e 's/::ffff://' -e 's/:[0-9]*//'|sort|uniq -c|sort -rn
	　　
		查看页面访问排名前十的IP
			cat /var/log/apache2/access.log  | cut -f1 -d " " | sort | uniq -c | sort -k 1 -r | head -10
	　　
		查看页面访问排名前十的URL
			cat /var/log/apache2/access.log  | cut -f4 -d " " | sort | uniq -c | sort -k 1 -r | head -10　　

####4.计划任务检查

		crontab -l   //查看所有用户的计划任务
		crontab -r   //删除所有用户的计划任务

		
####黑名单（慎用）

####

###常用命令

###应急排查

###防护免杀


