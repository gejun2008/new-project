# new-project

Window XAMPP mysql connect remote linux database

xampp 面板中mysql一行有 start admin config logs。
start： 启动mysql服务，启动后start就会变成stop。
admin：启动后可以通过phpmyadmin进行mysql的UI管理与操作。必须start 点击后admin才能被点击。
config：对启动的mysql的服务器进行一些配置，包括是否允许bind-address连接，
logs： 日志记录，可以查看运行的信息与错误信息。


1. linux 开放远程连接的权限。
进入mysql命令中，运行：GRANT ALL PRIVILEGES ON * . * TO  'root'@'%' IDENTIFIED BY ‘root’；
sql语句的意思：授权root用户,%表示通过任意主机连接方式，密码是indentified后面的root字符串。
执行flush privilege；
最后exit；

2. 授权完成后在xampp的mysql的bin目录下通过mysql -h hostaddress -u root -proot进行连接测试。如果可以连接success表示mysql可以访问。
有可能会出现1046的错误。这个时候需要在linux服务器中对mysql的access权限进行确认。

3. window平台有个比较好的mysql查看工具，navicatformysql，通过这个工具可以查看表之间的关系。
为了不污染远程的mysql数据库，可以采用备份远程数据为本地数据库。备份的方式可以采用命令行。
3-1 备份linux服务器端mysql的数据库，在window的cmd命令行进入mysql的bin目录运行mysqldump -h ip地址 -uusername -ppassword -A >~/name.sql
备份到本地后。
3-2 加载为本地数据库，命令如下：source "路径名"+/name.sql

4. 备份远程数据库与恢复本地数据库可以采用navicat软件很快捷操作。右击需要备份的数据库，选择“转储sql文件”，选择保存文件的路径。然后点击
本地数据库连接，新建一个与远程数据库同名的空数据库，然后右击这个数据库选择“运行sql文件”，选择刚才保存文件的即可。

5. 为了保持与远程数据库连接方式一致包括用户名与密码，本地连接也需要去设置登录用户名与密码，默认xampp的mysql服务器是root用户名密码为空。
这个默认登录的配置项可以通过C:\xampp\phpMyAdmin\config.inc.php进行配置。
配置项为
$cfg['Servers'][$i]['auth_type'] = 'config';
$cfg['Servers'][$i]['user'] = 'root';
$cfg['Servers'][$i]['password'] = '';
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['AllowNoPassword'] = true;
当前可以通过打开xampp中mysql的admin，创建一个与远程数据库连接方式的用户名与密码即可。然后navicat和程序代码都可以采用这个用户名与密码去登录数据库。

6. 最后就可以通过navicat同时管理本地与远程的数据库，navicat可以同时多个连接，所以使用上很方便。
