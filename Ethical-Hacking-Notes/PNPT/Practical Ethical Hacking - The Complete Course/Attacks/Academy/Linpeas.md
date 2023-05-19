 Searching passwords in config PHP files
$cfg['Servers'][$i]['AllowNoPassword'] = false;                              
$cfg['Servers'][$i]['AllowNoPassword'] = false;
$cfg['Servers'][$i]['AllowNoPassword'] = false;
$cfg['ShowChgPassword'] = true;
$mysql_password = "My_V3ryS3cur3_P4ss";
$mysql_password = "My_V3ryS3cur3_P4ss";
"If you search for config.php"
/var/www/html/academy/includes/config.php
- when opening the file
	- $ cat /var/www/html/academy/includes/config.php
$mysql_hostname = "localhost";
$mysql_user = "grimmie";
$mysql_password = "My_V3ryS3cur3_P4ss";
$mysql_database = "onlinecourse";
$bd = mysqli_connect($mysql_hostname, $mysql_user, $mysql_password, $mysql_database) or die("Could not connect database");


* * * * * /home/grimmie/backup.sh
