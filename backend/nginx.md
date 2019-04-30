# install on debian 9
[info](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-debian-9)

# nginx with nodejs
[intro](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-debian-9)

[openssl self-signed ssl](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-debian-9)

[ssl for free](https://www.sslforfree.com/create?domains=firmware.sx-technology.com.tw)

## setting reverse proxy
```conf
# /etc/nginx/sites-available/<file>
server {
        listen 443 ssl;

        server_name firmware.sx-technology.com.tw;

        ssl_certificate /etc/nginx/ssl/sslforfree/certificate.crt;
        ssl_certificate_key /etc/nginx/ssl/sslforfree/private.key;

        #include ssl/ssl-params.conf;

        location / {
            proxy_pass http://127.0.0.1:8888;
            proxy_set_header Host $host:$server_port;
        }

        #location /.well-known {
            #root /var/www/html/.well-known;  
        #}
}

server {
	listen 80;

	#root /var/www/html;

	# Add index.php to the list if you are using PHP
	#index index.html index.htm index.nginx-debian.html;

	server_name firmware.sx-technology.com.tw;
        
        #root /var/www/html;
    
        #index index.nginx-debian.html;
 
        #location / {
             #proxy_pass http://127.0.0.1:8888;
             #proxy_set_header Host $host;
        #}

        
        return 302 https://$server_name$request_uri;
}
```
## setting load balance
```conf
# /etc/nginx/sites-available/<file>
# loadbalance 的方式可使用其他方式，default是round-robin
# 還有least-connected and ip-hash兩種
upstream loadbalanceApp {
    server test1.com weight=2;
    server test2.com weight=3;
}

server {
        listen 443 ssl;

        server_name firmware.sx-technology.com.tw;

        ssl_certificate /etc/nginx/ssl/sslforfree/certificate.crt;
        ssl_certificate_key /etc/nginx/ssl/sslforfree/private.key;

        #include ssl/ssl-params.conf;

        location / {
            proxy_pass http://loadbalanceApp;
            # server_port 是有可能nginx listen其他port的時候會偵測不到需要加這個
            proxy_set_header Host $host:$server_port;
            # 如果做loadbalance，想要記錄使用者ip都會偵測到reverse proxy那台的ip
            # 所以要 set 下面兩個 header
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        #location /.well-known {
            #root /var/www/html/.well-known;  
        #}
}
```
