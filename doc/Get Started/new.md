## 开始旅程
 > 记住：Medoo 是很简单的！

### 要求
* PHP 5.1 及以上, 极力推荐 PHP 5.4 及以上 并开启 PDO 支持
* 安装了例如 MySQL, MSSQL, SQLite 等等数据库
* 确保 php_pdo_xxx extension 可用并且在 php.ini 有安装配置
* 懂得一点关于 SQL 的知识

### 提示
在 PHP 5.4 及以上版本，你可以用 [] 来表示数组。 Medoo 文档里的所有代码是用 `[]` 来代替传统的 `array()` 的。

```PHP
// On PHP 5.1
$data = array("foo", "bar");
 
// On PHP 5.4+
$data = ["foo", "bar"];
```

### Php_pdo 扩展列表
* MySQL, MariaDB -> php_pdo_mysql
* MSSQL (Windows) -> php_pdo_sqlsrv
* MSSQL (Liunx/UNIX) -> php_pdo_dblib
* Oracle -> php_pdo_oci
* SQLite -> php_pdo_sqlite
* PostgreSQL -> php_pdo_pgsql
* Sybase -> php_pdo_dblib

### 安装
下载 `medoo.php` 后把它放在你的项目目录里，准备好了吗？

### 一点简单的设置
你可以用下面给出的三种方式来配置 Medoo，设置好之后就可以连接到数据库了。

```PHP
// 在独立的文件中配置 Medoo
require  'medoo.php';
 
$database = new medoo([
	// 必填参数
	'database_type' => 'mysql',      //数据库类型
	'database_name' => 'name',       //数据库名
	'server' => 'localhost',         //数据库服务器
	'username' => 'your_username',   //数据库用户名
	'password' => 'your_password',   //数据库密码
 
	// 可选参数
	'port' => 3306,                  //端口
	'charset' => 'utf8',             //字符编码
	// 如果使用 driver_option 来连接数据库，请参阅 http://www.php.net/manual/en/pdo.setattribute.php
	'option' => [
		PDO::ATTR_CASE => PDO::CASE_NATURAL
	]
]);
 
$database->insert("account", [
	"user_name" => "foo",
	"email" => "foo@bar.com"
]);
 
// 或者直接打开 medoo.php ，修改顶部的相关信息即可
// 当你的项目有多个文件需要使用到数据库的时候，采用这种方法就可以不用多次配置 Medoo 了
// MySQL, MariaDB MSSQL, PostgreSQL, Sybase 的 $database_type 按照下面列表来填写
// MySQL -> mysql
// MariaDB -> mariadb
// MSSQL -> mssql
// PostgreSQL -> pgsql
// Oracle -> oracle
// Sybase -> sybase
class medoo
{
	protected $database_type = 'mysql'; // 数据库类型
 
	protected $server = 'localhost';
	
	protected $username = 'your_username';
	
	protected $password = 'your_password';
 
	// Optional
	protected $database_name = 'my_datebase';
 
	protected $port = 3306;
 
	protected $charset = 'utf8';
	....
 
// 配置完成，在你的项目文件中可以使用 Medoo 了
require_once 'medoo.php';
 
$database = new medoo();
 
$database->insert("account", [
	"user_name" => "foo",
	"email" => "foo@bar.com"
]);
 
// 当你的项目需要用到多个数据库时，你也可以不填写 $database_name 
// 然后再 new 对象的时候当把数据库名作参数
class medoo
{
	protected $database_type = 'mysql'; // The type name of database
 
	protected $server = 'localhost';
	
	protected $username = 'your_username';
	
	protected $password = 'your_password';
 
	// Optional
	protected $database_name = '';
 
$database = new medoo('my_database');
```

### 如果 MSSQL
 > 如果你用的是 MSSQL，那么你需要安装额外的扩展。Windows 下安装 pdo_sqlsrv 。Liunx/UNIX 则安装 pdo_dblib 。不建议使用 pdo_mssql ，因为它太古老了，而且即将被踢出 PHP 的家门。

### For SQLite
// 直接在 medoo.php 修改数据库类型即可
class medoo
{
	protected $database_type = 'sqlite';
 
	// For SQLite [optional]
	protected $database_file = 'my/database/path/database.db';
 
// 开始吧！
require_once 'medoo.php';
 
$database = new medoo('my/database/path/database.db');
 
// 或者把配置写在单独的文件里
$database = new medoo([
	'database_type' => 'sqlite',
	'database_file' => 'my/database/path/database.db'
]);
 
$database->insert("account", [
	"user_name" => "foo",
	"email" => "foo@bar.com"
]);

### PHP PDO 驱动安装
Medoo 需要 PHP with PDO 的支持，如果此前你没有安装过，那么按照下面的步骤来安装：
```PHP
// 打开 php.ini ，然后删掉你要安装的那个数据库扩展前面的 ';' 。
 
// 找到这行
;extension=php_pdo_mysql.dll
 
// 改成这样
extension=php_pdo_mysql.dll
 
// 保存，然后重启 PHP 或者 Apache
 
// 如果 PDO 安装成功，那么你可以在 phpinfo() 中看到
```