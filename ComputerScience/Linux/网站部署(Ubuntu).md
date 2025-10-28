1. 安装 nginx
	```bash
	sudo apt install nginx certbot python3-certbot-nginx -y
	```
2. 域名解析
3. 配置 nginx
	1. 将源码放在 /var/www/项目名 下
	2. 执行权限：sudo chown -R $USER:$USER /var/www/thyonox.com/html
	3. nginx 配置文件，在 /etc/nginx/sites-available 目录下创建thyonox.com 或者 thyonox.conf
		```bash
		server {
		    listen 80;
		    listen [::]:80;
		    server_name thyonox.com www.thyonox.com;
		
		    root /var/www/thyonox.com/html;
		    index index.html index.htm;
		
		    location / {
		        try_files $uri $uri/ =404;
		    }
		}
		
		server {
		    listen 80;
		    listen [::]:80;
		    server_name panel.thyonox.com;
		
		    location / {
		        proxy_pass http://localhost:40186;
		        proxy_set_header Host $host;
		        proxy_set_header X-Real-IP $remote_addr;
		        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		        proxy_set_header X-Forwarded-Proto $scheme;
		    }
		}
		
		server {
		    listen 80;
		    listen [::]:80;
		    server_name proxy.thyonox.com;
		
		    location / {
		        proxy_pass http://localhost:9999;
		        proxy_set_header Host $host;
		        proxy_set_header X-Real-IP $remote_addr;
		        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		        proxy_set_header X-Forwarded-Proto $scheme;
		    }
		}
		```
4. 启用配置（http://ip 验证）
	```bash
	sudo ln -s /etc/nginx/sites-available/thyonox.conf /etc/nginx/sites-enabled/
	sudo nginx -t
	sudo systemctl reload nginx
	```
5. 申请证书（https://domain 验证）
	```bash
	sudo certbot --nginx -d thyonox.com -d www.thyonox.com -d panel.thyonox.com -d proxy.thyonox.com
	
	```