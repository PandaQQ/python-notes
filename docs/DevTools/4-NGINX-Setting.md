# NGINX SETTING

> http redirect to https:

```
if($server_port = 80) {
	   return 301 https://$host$request_uri;
}
```







```
server {
#       listen 80 default_server;
#       listen [::]:80 default_server;
#

        listen 443 ssl;
        ssl_certificate /etc/ssl/certs/key.crt;
        ssl_certificate_key /etc/ssl/private.key;
				ssl_protocols TLSv1.2;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.1-fpm.sock;
				fastcgi_buffering off;
         }


        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ /index.php?$args;
        }

	location ~* \.(txt|xml|js)$ {
                expires 1d;
        }

        location ~* \.(css)$ {
                expires 1d;
        }

        location ~* \.(flv|ico|pdf|avi|mov|ppt|doc|mp3|wmv|wav|mp4|m4v|ogg|webm|aac|eot|ttf|otf|woff|svg)$ {
                expires 1d;
        }

        location ~* \.(jpg|jpeg|png|gif|swf|webp)$ {
                expires 1d;
        }

        if($server_port = 80) {
	   			return 301 https://$host$request_uri;
				}
				
				client_max_body_size 100M;
				location /api/ {
        	proxy_set_header Host $http_host;
        	proxy_set_header  X-Real-IP $remote_addr;
        	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        	proxy_set_header X-Forwarded-Proto $scheme;
        	rewrite ^/api/(.*)$ /$1 break;
        	proxy_pass http://0.0.0.0:8000/;
     }
				
}
```

