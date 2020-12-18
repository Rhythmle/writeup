###<center>[HCTF 2018]WarmUp 1</center>
打开页面就是一个大大的滑稽脸，右键查看代码。
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!--source.php-->
    
    <br><img src="https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg" /></body>
</html>
```
可以看到，代码提示了source.php，直接访问。
<div align=center><img src ="01.jpg"/></div>
这里题目给了一个hint，打开看看，题目给的是flag的所在的文件名。先记下，以备后用。
<div align=center><img src ="02.jpg"/></div>
回头看看source.php里的代码。这里分为两块，第一块是checkFile函数，第二块是一个if判断。<br>
<div align=center><img src ="03.jpg"/></div>
先看这个if语句，先传入一个参数file，file需要满足下面三个条件：<br>
<font size=2>1、file不为空<br></font>
<font size=2>2、file是一个字符串<br></font>
<font size=2>3、file需要通过checkFile函数的检查</font><br>
既然如此，那就看看checkFile函数。
<div align=center><img src ="04.jpg"/></div>
我们的目标是要checkFile()返回true，且include包含的文件必须是hint里给出的文件。<br>
看一下代码流程：<br>
<font size=2>1、白名单检查<br></font>
<font size=2>2、取?前的字符串<br></font>
<font size=2>3、白名单检查<br></font>
<font size=2>4、url解码后第二次截取?之前的字符串<br></font>
<font size=2>5、白名单检查<br></font>
整个流程中存在字符串截取，一次url解码，所以构造字符串时可以将?进行url编码。<br>
这样，第一次截取能够全部被保留并赋值给$_page，第二次截取就会抛弃?后的字符串，从而通过检查。<br>
<code>payload=/?file=source.php%3f../../../../../../../../../ffffllllaaaagggg</code><br>
ffffllllaaaagggg并不是在网站根目录下，所以可以遍历目录得到flag。<br>