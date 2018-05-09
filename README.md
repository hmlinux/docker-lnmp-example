#Docker手动创建并启动LNMP网站平台#1

创建docker容器组内部网络
    
    # docker network create lnmp

查看docker所有网络
    
    # docker network ls

创建一个MySQL数据卷
    
    # docker volume create mysql-vol

查看数据卷信息
    
    # docker volume inspect mysql-vol

创建并启动MySQL容器(MySQL8.0)
    
    # docker run -d -it \
    --name lnmp_mysql \
    --net lnmp \
    -p 172.16.10.14:3306:3306 \
    --mount src=mysql-vol,dst=/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    mysql --character-set-server=utf8 \
    --default-authentication-plugin=mysql_native_password


创建并启动MySQL容器(MySQL5.6)
    
    # docker run -d -it \
    --name lnmp_mysql \
    --net lnmp \
    -p 172.16.10.14:3306:3306 \
    --mount src=mysql-vol,dst=/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    mysql5.6 --character-set-server=utf8


连接通过MySQL容器创建的MySQL服务

    # mysql -uroot -p123456 -h172.16.10.14


创建数据库
    
    # docker exec lnmp_mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD" -e "create database wordpress"'

查看数据库
    
    # docker exec lnmp_mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD" -e "show databases"'

创建并启动PHP环境的容器
    
    # docker run -itd \
    --name lnmp_web \
    --net lnmp \
    -p 172.16.10.14:80:80 \
    --mount type=bind,src=/app/wwwroot,dst=/var/www/html richarvey/nginx-php-fpm


    # tar -zxf wordpress-4.9.5.tar.gz -C /app/wwwroot/

    # docker exec -it 302036f0c801 sh
    /var/www/html # chown -R nginx:nginx wordpress


Web URL
    
    http://172.16.10.14/wordpress/

