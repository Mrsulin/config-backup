### docke 安装和启动mysql

docker search mysql

docker pull mysql

docker run -d -p 3308:3306 -e MYSQL_ROOT_PASSWORD=root --name plimsmysql mysql



##### mysql8修改认证方式

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';