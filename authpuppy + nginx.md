# 安装
        yum install -y libxslt libxslt-devel  php-pdo php-xml php-mbstring php-process php-devel
        
        vi etc/php.ini
        short_open_tag = Off
        magic_quotes_gpc = Off
        
        wget http://pecl.php.net/get/APC-3.1.13.tgz
        
        echo "apc__extension=/usr/lib64/php/modules/apc.so" >> /etc/php.ini
        
        ln -s /usr/lib64/php/modules/apc.so /usr/lib/
        
        拷贝authpuppy到/var/www/


# 配置
        index index.php index.html index.htm;
        root /var/www/;
        
        location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
                try_files $uri =404;
        }
        
        location ~ /authpuppy/web/ {
            include        fastcgi_params;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        }
