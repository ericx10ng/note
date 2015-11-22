# 安装
        yum install -y libxslt libxslt-devel  php-pdo php-xml php-mbstring php-process php-devel
        
        vi /etc/php.ini
        short_open_tag = Off
        magic_quotes_gpc = Off
        
        wget http://pecl.php.net/get/APC-3.1.13.tgz
        phpize
        ./configure --enable-apc
        echo "apc__extension=/usr/lib64/php/modules/apc.so" >> /etc/php.ini
        ln -s /usr/lib64/php/modules/apc.so /usr/lib/
        
        拷贝authpuppy到/var/www/


# 配置
        index index.php index.html index.htm;
        root /var/www/;
        
        location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
            try_files $uri =404;
        }

        location ~ \.php$ {
            include        fastcgi_params;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        }
         
        location ~ /authpuppy/web/ {
            try_files $uri $uri/ /authpuppy/web/index.php?q=$uri&$args;
            include        fastcgi_params;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        }


# 问题

* 配置authpuppy时，db连接一直失败：
    * 找到出错的字符串所在代码位置："Impossible to connect to the database"
    * 修改代码，把异常信息打印出来：$this->getUser()->setFlash('error', $e, false);
    * 异常：exception 'Doctrine_Connection_Exception' with message 'Couldn't locate driver named mysql'
    * 解决办法：安装php-mysql, yum install php-mysql

* session写失败, admin登录不写权限添加进去
    * 写权限添加 /var/lib/php/session/

参考：
* http://www.cnblogs.com/MarkGrid/p/4113408.html
* http://bbs.csdn.net/topics/390036982

