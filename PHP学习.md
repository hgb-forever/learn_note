# 一、PHP概述与工作流程

> PHP（Hypertext Preprocessor 超文本预处理器）是一种在服务端执行的脚本语言，用户动态开发网站。

PHP脚本主要应用与三个领域：

- 服务端脚本
- 命令行脚本
- 编写桌面应用程序

PHP程序工作流程

![image-20210520144332572](E:\hgb\learn_note\images\image-20210520144332572.png)

# 二、搭建PHP环境

# 三、PHP语法

## 基本的PHP语法

PHP 脚本可以放在文档中的任何位置。

PHP 脚本以 **<?php** 开始，以 **?>** 结束：

```php
<?php
// PHP 代码
?>
```

PHP 文件的默认文件扩展名是 ".php"。

PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。

下面，我们提供了一个简单的 PHP 文件实例，它可以向浏览器输出文本 "Hello World!"：

```php+HTML
<!DOCTYPE html>
<html>
<body>

<h1>My first PHP page</h1>

<?php
echo "Hello World!";
?>

</body>
</html>
```

PHP 中的每个代码行都必须以分号结束。分号是一种分隔符，用于把指令集区分开来。

通过 PHP，有两种在浏览器输出文本的基础指令：**echo** 和 **print**。

## PHP中的注释

```php+HTML
<!DOCTYPE html>
<html>
<body>

<?php
// 这是 PHP 单行注释

/*
这是
PHP 多行
注释
*/

?>
</body>
</html>
```

# 四、PHP变量

> PHP是一门弱类型语言

PHP 变量规则：

- 变量以 $ 符号开始，后面跟着变量的名称
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母数字字符以及下划线（A-z、0-9 和 _ ）
- 变量名不能包含空格
- 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）

PHP 没有声明变量的命令，变量在您第一次赋值给它的时候被创建。

## PHP变量的作用域

PHP 有四种不同的变量作用域：

- local
- global
- static
- parameter

### 局部和全局作用域

在所有函数外部定义的变量，拥有全局作用域。除了函数外，全局变量可以被脚本中的任何部分访问，要在一个函数中访问一个全局变量，需要使用 global 关键字。

在 PHP 函数内部声明的变量是局部变量，仅能在函数内部访问：

```php+HTML
<?php
$x=5; // 全局变量

function myTest()
{
    $y=10; // 局部变量
    echo "<p>测试函数内变量:<p>";
    echo "变量 x 为: $x";
    echo "<br>";
    echo "变量 y 为: $y";
} 

myTest();

echo "<p>测试函数外变量:<p>";
echo "变量 x 为: $x";
echo "<br>";
echo "变量 y 为: $y";
?>
```

函数内访问函数外定义的全局变量需要在函数内用global关键字声明变量

```php
<?php
$x=5;
$y=10;
 
function myTest()
{
    global $x,$y;
    $y=$x+$y;
}
 
myTest();
echo $y; // 输出 15
?>
```

PHP 将所有全局变量存储在一个名为 $GLOBALS[*index*] 的数组中。 *index* 保存变量的名称。这个数组可以在函数内部访问，也可以直接用来更新全局变量。

上面的实例可以写成这样：

```php
<?php
$x=5;
$y=10;
 
function myTest()
{
    $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
} 
 
myTest();
echo $y;
?>
```

### Static 作用域

当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。

要做到这一点，请在您第一次声明变量时使用 **static** 关键字：

```php
<?php
function myTest()
{
    static $x=0;
    echo $x;
    $x++;
    echo PHP_EOL;    // 换行符
}
 
myTest();
myTest();
myTest();
?>
```

### 参数作用域

参数是通过调用代码将值传递给函数的局部变量。

参数是在参数列表中声明的，作为函数声明的一部分：

```php
<?php
function myTest($x)
{
    echo $x;
}
myTest(5);
?>
```

## PHP 常量

可以使用define()定义常量，也可以使用关键字const定义常量，常量不能带$符号

```php
<?php
// 不区分大小写的常量名
define("GREETING", "欢迎访问 Runoob.com", true);
echo greeting;  // 输出 "欢迎访问 Runoob.com"
?>
```



# 五、PHP输出语句

echo 和 print 区别:

- echo - 可以输出一个或多个字符串
- print - 只允许输出一个字符串，返回值总为 1

**提示：**echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

## PHP echo 语句

echo 是一个语言结构，使用的时候可以不用加括号，也可以加上括号： echo 或 echo()。

**显示字符串**

下面的实例演示了如何使用 echo 命令输出字符串（字符串可以包含 HTML 标签）：

```php
<?php
echo "<h2>PHP 很有趣!</h2>";
echo "Hello world!<br>";
echo "我要学 PHP!<br>";
echo "这是一个", "字符串，", "使用了", "多个", "参数。";
?>
```

**显示变量**

下面的实例演示了如何使用 echo 命令输出变量和字符串：

```php
<?php
$txt1="学习 PHP";
$txt2="RUNOOB.COM";
$cars=array("Volvo","BMW","Toyota");
 
echo $txt1;
echo "<br>";
echo "在 $txt2 学习 PHP ";
echo "<br>";
echo "我车的品牌是 {$cars[0]}";
?>
```

## PHP print 语句

print 同样是一个语言结构，可以使用括号，也可以不使用括号： print 或 print()。

**显示字符串**

下面的实例演示了如何使用 print 命令输出字符串（字符串可以包含 HTML 标签）：

```php
<?php
print "<h2>PHP 很有趣!</h2>";
print "Hello world!<br>";
print "我要学习 PHP!";
?>
```

**显示变量**

下面的实例演示了如何使用 print 命令输出变量和字符串

```php
<?php
$txt1="学习 PHP";
$txt2="RUNOOB.COM";
$cars=array("Volvo","BMW","Toyota");
 
print $txt1;
print "<br>";
print "在 $txt2 学习 PHP ";
print "<br>";
print "我车的品牌是 {$cars[0]}";
?>
```

## PHP EOF(heredoc)

-  1.PHP 定界符 **EOF** 的作用就是按照原样，包括换行格式什么的，输出在其内部的东西；
-  2.在 PHP 定界符 **EOF** 中的任何特殊字符都不需要转义；
-  3.PHP 定界符 **EOF**
- EOF 中是会解析 html 格式内容的，并且在双引号内的内容也有转义效果。

```php
<?php
$name="变量会被解析";
$a=<<<EOF
$name<br><a>html格式会被解析</a><br/>双引号和Html格式外的其他内容都不会被解析
"双引号外所有被排列好的格式都会被保留"
"但是双引号内会保留转义"符"的转义效果,比如table:\t和换行：\n下一行"
下一行
EOF;
echo $a;
?>  <?php
```

# 六、PHP数据类型

String（字符串）,

 Integer（整型）, 

Float（浮点型）,

 Boolean（布尔型）,

 Array（数组）, 

Object（对象）, 

NULL（空值）。

# 七、PHP条件语句

## if语句

**在若干条件之一成立时执行一个代码块**，请使用 if....elseif...else 语句。.

语法

```php
<?php
$t=date("H");
if ($t<"10")
{
    echo "Have a good morning!";
}
elseif ($t<"20")
{
    echo "Have a good day!";
}
else
{
    echo "Have a good night!";
}
?>
```

## switch语句

```php
<?php
$favcolor="red";
switch ($favcolor)
{
case "red":
    echo "你喜欢的颜色是红色!";
    break;
case "blue":
    echo "你喜欢的颜色是蓝色!";
    break;
case "green":
    echo "你喜欢的颜色是绿色!";
    break;
default:
    echo "你喜欢的颜色不是 红, 蓝, 或绿色!";
}
?>
```

# 八、 PHP 中创建数组

## array() 函数

用于创建数组：

array();

在 PHP 中，有三种类型的数组：

- **数值数组** - 带有数字 ID 键的数组
- **关联数组** - 带有指定的键的数组，每个键关联一个值
- **多维数组** - 包含一个或多个数组的数组

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>
```

## count（）函数

获取数组长度

count() 函数用于返回数组的长度（元素的数量）：

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
?>
```

## 遍历数组

```php
<?php
$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);
 
for($x=0;$x<$arrlength;$x++)
{
    echo $cars[$x];
    echo "<br>";
}
?>
```

## PHP 关联数组

关联数组是使用您分配给数组的指定的键的数组。类似于Java的Map

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>
```

## 遍历关联数组

遍历并打印关联数组中的所有值，您可以使用 foreach 循环，如下所示：

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```

# 九、PHP 数组排序

------

数组中的元素可以按字母或数字顺序进行降序或升序排列。

------

## PHP - 数组排序函数

在本章中，我们将一一介绍下列 PHP 数组排序函数：

- sort($arr) - 对数组进行升序排列
- rsort() - 对数组进行降序排列
- asort() - 根据关联数组的值，对数组进行升序排列
- ksort() - 根据关联数组的键，对数组进行升序排列
- arsort() - 根据关联数组的值，对数组进行降序排列
- krsort() - 根据关联数组的键，对数组进行降序排列



# 十、循环

while

```php
while (条件)
{
    要执行的代码;
}
```

do...while

```php
do
{
    要执行的代码;
}
while (条件);
```

for

```php
for (初始值; 条件; 增量)
{
    要执行的代码;
}
```

foreach

遍历数组

```php
foreach ($array as $value)
{
    要执行代码;
}
```

遍历关联数组

```php
<?php
$x=array("Google","Runoob","Taobao");
foreach ($x as $value)
{
    echo $value . PHP_EOL;
}
?>
```

# 十一、函数

> 用户定义的函数和语言关键字对大小写不敏感。

实例1：无参

```php
<?php
function writeName()
{
    echo "Kai Jim Refsnes";
}
 
echo "My name is ";
writeName();
?>
```

实例2：有参

```php
<?php
function writeName($fname)
{
    echo $fname . " Refsnes.<br>";
}
 
echo "My name is ";
writeName("Kai Jim");
echo "My sister's name is ";
writeName("Hege");
echo "My brother's name is ";
writeName("Stale");
?>
```

实例3：有返回值

```php
<?php
function add($x,$y)
{
    $total=$x+$y;
    return $total;
}
 
echo "1 + 16 = " . add(1,16);
?>
```

# 十二、魔术常量

根据使用位置的不同，输出的内容可能不同

常用的有：

\__LINE__

\__FILE__

\__DIR__

\__FUNCTION__

\__CLASS__

\__NAMESPACE__

# 十三、命名空间

在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。所有非 PHP 代码包括空白符都不能出现在命名空间的声明之前。

```php
<?php
declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
namespace MyProject {

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

# 十四、PHP高级

## PHP date() 函数

PHP date() 函数可把时间戳格式化为可读性更好的日期和时间。

![Tip](E:\hgb\learnnotes\images\lamp.gif)时间戳是一个字符序列，表示一定的事件发生的日期/时间。

```php
<?php
echo date("Y/m/d") . "<br>";
echo date("Y.m.d") . "<br>";
echo date("Y-m-d");
?>

2016/10/21
2016.10.21
2016-10-21
```



## PHP 包含

PHP include 和 require 语句

### 基础实例

假设您有一个标准的页头文件，名为 "header.php"。如需在页面中引用这个页头文件，请使用 include/require：

```php+HTML
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>

<?php include 'header.php'; ?>
<h1>欢迎来到我的主页!</h1>
<p>一些文本。</p>

</body>
</html>
```

> 可以使用引用文件中的变量

## PHP 文件处理

fopen()函数用于打开文件

fclose()关闭文件

feof()检测文件末尾

fgets()逐行读取

fgetc()逐字符读取

实例:

```php
<?php
$file = fopen("welcome.txt", "r") or exit("无法打开文件!");
// 读取文件每一行，直到文件结尾
while(!feof($file))
{
    echo fgets($file). "<br>";
}
fclose($file);
?>
```

## 文件上传

### 创建脚本

"upload_file.php" 文件含有供上传文件的代码：

```php
<?php
if ($_FILES["file"]["error"] > 0)
{
    echo "错误：" . $_FILES["file"]["error"] . "<br>";
}
else
{
    echo "上传文件名: " . $_FILES["file"]["name"] . "<br>";
    echo "文件类型: " . $_FILES["file"]["type"] . "<br>";
    echo "文件大小: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
    echo "文件临时存储的位置: " . $_FILES["file"]["tmp_name"];
}
?>
```

### 上传限制

```php
<?php
// 允许上传的图片后缀
$allowedExts = array("gif", "jpeg", "jpg", "png");
$temp = explode(".", $_FILES["file"]["name"]);
$extension = end($temp);        // 获取文件后缀名
if ((($_FILES["file"]["type"] == "image/gif")
|| ($_FILES["file"]["type"] == "image/jpeg")
|| ($_FILES["file"]["type"] == "image/jpg")
|| ($_FILES["file"]["type"] == "image/pjpeg")
|| ($_FILES["file"]["type"] == "image/x-png")
|| ($_FILES["file"]["type"] == "image/png"))
&& ($_FILES["file"]["size"] < 204800)    // 小于 200 kb
&& in_array($extension, $allowedExts))
{
    if ($_FILES["file"]["error"] > 0)
    {
        echo "错误：: " . $_FILES["file"]["error"] . "<br>";
    }
    else
    {
        echo "上传文件名: " . $_FILES["file"]["name"] . "<br>";
        echo "文件类型: " . $_FILES["file"]["type"] . "<br>";
        echo "文件大小: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
        echo "文件临时存储的位置: " . $_FILES["file"]["tmp_name"];
    }
}
else
{
    echo "非法的文件格式";
}
?>
```

### 保存被上传的文件

```php
<?php
// 允许上传的图片后缀
$allowedExts = array("gif", "jpeg", "jpg", "png");
$temp = explode(".", $_FILES["file"]["name"]);
echo $_FILES["file"]["size"];
$extension = end($temp);     // 获取文件后缀名
if ((($_FILES["file"]["type"] == "image/gif")
|| ($_FILES["file"]["type"] == "image/jpeg")
|| ($_FILES["file"]["type"] == "image/jpg")
|| ($_FILES["file"]["type"] == "image/pjpeg")
|| ($_FILES["file"]["type"] == "image/x-png")
|| ($_FILES["file"]["type"] == "image/png"))
&& ($_FILES["file"]["size"] < 204800)   // 小于 200 kb
&& in_array($extension, $allowedExts))
{
    if ($_FILES["file"]["error"] > 0)
    {
        echo "错误：: " . $_FILES["file"]["error"] . "<br>";
    }
    else
    {
        echo "上传文件名: " . $_FILES["file"]["name"] . "<br>";
        echo "文件类型: " . $_FILES["file"]["type"] . "<br>";
        echo "文件大小: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
        echo "文件临时存储的位置: " . $_FILES["file"]["tmp_name"] . "<br>";
        
        // 判断当前目录下的 upload 目录是否存在该文件
        // 如果没有 upload 目录，你需要创建它，upload 目录权限为 777
        if (file_exists("upload/" . $_FILES["file"]["name"]))
        {
            echo $_FILES["file"]["name"] . " 文件已经存在。 ";
        }
        else
        {
            // 如果 upload 目录不存在该文件则将文件上传到 upload 目录下
            move_uploaded_file($_FILES["file"]["tmp_name"], "upload/" . $_FILES["file"]["name"]);
            echo "文件存储在: " . "upload/" . $_FILES["file"]["name"];
        }
    }
}
else
{
    echo "非法的文件格式";
}
?>
```

## Cookie

setcookie() 函数用于设置 cookie。

```php
<?php
setcookie("user", "runoob", time()+3600);
?>
<html>
```

使用全局变量数组$_COOKIE获取cookie

```php
<?php
// 输出 cookie 值
echo $_COOKIE["user"];

// 查看所有 cookie
print_r($_COOKIE);
?>
```

当删除 cookie 时，您应当使过期日期变更为过去的时间点。

删除的实例：

```php
<?php
// 设置 cookie 过期时间为过去 1 小时
setcookie("user", "", time()-3600);
?>
```

## Session

### 开始 PHP Session

在您把用户信息存储到 PHP session 中之前，首先必须启动会话。

**注释：**session_start() 函数必须位于 <html> 标签之前：

```php
<?php session_start(); ?>
 
<html>
<body>
 
</body>
</html>
```

上面的代码会向服务器注册用户的会话，以便您可以开始保存用户信息，同时会为用户会话分配一个 UID。

### 存储和获取Session

```php
<?php
session_start();
// 存储 session 数据
$_SESSION['views']=1;
?>
 
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
 
<?php
// 检索 session 数据
echo "浏览量：". $_SESSION['views'];
?>
 
</body>
</html>
```

### 删除Session信息

```php
<?php
session_start();
if(isset($_SESSION['views'])) //删除部分信息
{
    unset($_SESSION['views']);
}
?>
```

或者

```php
<?php
session_destroy(); //彻底销毁session
?>
```

## Error

die()函数返回提示并且终止程序

用户可以自定义错误处理器，需保证接受至少两个参数

```php
<?php
// 错误处理函数
function customError($errno, $errstr)
{
    echo "<b>Error:</b> [$errno] $errstr<br>";
    echo "脚本结束";
    die();
}

// 设置错误处理函数
set_error_handler("customError",E_USER_WARNING);

// 触发错误
$test=2;
if ($test>1)
{
    trigger_error("变量值必须小于等于 1",E_USER_WARNING);
}
?>
```

通过修改trigger_error()的第二个参数来设置错误触发级别。

## 异常处理

PHP异常处理和Java一样，使用try{}catch(exception $e){}代码块，也可以自定义异常类。

set_exception_handler() 函数可设置处理所有未捕获异常的用户定义函数。

```php
<?php
function myException($exception)
{
    echo "<b>Exception:</b> " , $exception->getMessage();
}
 
set_exception_handler('myException');
 
throw new Exception('Uncaught Exception occurred');
?>
```

## 过滤器

如需过滤变量，请使用下面的过滤器函数之一：

- filter_var() - 通过一个指定的过滤器来过滤单一的变量
- filter_var_array() - 通过相同的或不同的过滤器来过滤多个变量
- filter_input - 获取一个输入变量，并对它进行过滤
- filter_input_array - 获取多个输入变量，并通过相同的或不同的过滤器对它们进行过滤

更加精确得过滤应该使用option参数

## JSON

| 函数            | 描述                                          |
| :-------------- | :-------------------------------------------- |
| json_encode     | 对变量进行 JSON 编码                          |
| json_decode     | 对 JSON 格式的字符串进行解码，转换为 PHP 变量 |
| json_last_error | 返回最后发生的错误                            |

```php
<?php
   $arr = array('runoob' => '菜鸟教程', 'taobao' => '淘宝网');
   echo json_encode($arr); // 编码中文
   echo PHP_EOL;  // 换行符
   echo json_encode($arr, JSON_UNESCAPED_UNICODE);  // 不编码中文
?>
```

```php
<?php
   $json = '{"a":1,"b":2,"c":3,"d":4,"e":5}';

   var_dump(json_decode($json));
   var_dump(json_decode($json, true)); //转化为数组
?>
```



# PHP 网络请求

## PHP $_REQUEST

PHP $_REQUEST 用于收集HTML表单提交的数据。

以下实例显示了一个输入字段（input）及提交按钮(submit)的表单(form)。 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至<form>标签中 action 属性中指定的脚本文件。 在这个实例中，我们指定文件来处理表单数据。如果你希望其他的PHP文件来处理该数据，你可以修改该指定的脚本文件名。 然后，我们可以使用超级全局变量 $_REQUEST 来收集表单中的 input 字段数据:

```php+HTML
<html>
<body>
 
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>
 
<?php 
$name = $_REQUEST['fname']; 
echo $name; 
?>
 
</body>
</html>
```

## POST请求

使用$_POST收集表单数据

```php+HTML
<html>
<body>
 
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>
 
<?php 
$name = $_POST['fname']; 
echo $name; 
?>
 
</body>
</html>
```

## GET请求

```html
<html>
<body>

<a href="test_get.php?subject=PHP&web=runoob.com">Test $GET</a>

</body>
</html>
```

当用户点击链接 "Test $GET", 参数 "subject" 和 "web" 将发送至"test_get.php",你可以在 "test_get.php" 文件中使用 $_GET 变量来获取这些数据。

以下实例显示了 "test_get.php" 文件的代码:

```php+HTML
<html>
<body>
 
<?php 
echo "Study " . $_GET['subject'] . " @ " . $_GET['web'];
?>
 
</body>
</html>
```

# PHP 特殊符号

@加在变量或者函数前面用于屏蔽错误信息，列如：

```php
@define("hgb","awesome");
```

# 连接数据库

MySQl

Redis

## Pdo

配置PHP文件以支持Pdo，php.ini

```
extension=php_pdo.dll
```

添加各数据库的Pdo支持

```
;extension=php_pdo_firebird.dll
;extension=php_pdo_informix.dll
;extension=php_pdo_mssql.dll
;extension=php_pdo_mysql.dll
;extension=php_pdo_oci.dll
;extension=php_pdo_oci8.dll
;extension=php_pdo_odbc.dll
;extension=php_pdo_pgsql.dll
;extension=php_pdo_sqlite.dll
```

实例

```php
<?php
$dbms='mysql';     //数据库类型
$host='localhost'; //数据库主机名
$dbName='test';    //使用的数据库
$user='root';      //数据库连接用户名
$pass='';          //对应的密码
$dsn="$dbms:host=$host;dbname=$dbName";


try {
    $dbh = new PDO($dsn, $user, $pass); //初始化一个PDO对象
    echo "连接成功<br/>";
    /*你还可以进行一次搜索操作
    foreach ($dbh->query('SELECT * from FOO') as $row) {
        print_r($row); //你可以用 echo($GLOBAL); 来看到这些值
    }
    */
    $dbh = null;
} catch (PDOException $e) {
    die ("Error!: " . $e->getMessage() . "<br/>");
}
//默认这个不是长连接，如果需要数据库长连接，需要最后加一个参数：array(PDO::ATTR_PERSISTENT => true) 变成这样：
$db = new PDO($dsn, $user, $pass, array(PDO::ATTR_PERSISTENT => true));

?>
```

## 获取数据

使用fetch()或fetchALL()获取数据，需添加获取规则

```
 $total = $res2->fetch(PDO::FETCH_ASSOC);
```

