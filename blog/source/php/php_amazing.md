title: "PHP 数值转换、进制转换和位运算"
date: 2018-03-26 19:58:32 +0800
update: 2018-03-26 20:01:32 +0800
author: me
tags:
    - PHP
    - Tech
preview: PHP 数值转换、进制、位运算的一个总结，避免踩坑

---



## 引子 

```
$str1 = "234";
var_dump(intval($str1)); 		// int(234)
var_dump((int)$str1);			// int(234)

$str2 = "234liukaining";
var_dump(intval($str2));		// int(234)
var_dump((int)$str2);			// int(234)

$str3 = "liukaining234";
var_dump(intval($str3));		// int(0)
var_dump((int)$str3);			// int(0)


$int1 = 234;
var_dump($str1);			
var_dump($int1);
var_dump($str1 == $int1);		// true
var_dump($str1 === $int1);	// false


$int2 = $str1 + 222;			
var_dump($int2);		// int(456)
$str2 = $int1 + "222";
var_dump($str2);		// int(456)


$float1 = 11.22;
var_dump($float1 + $str1);		 // float(245.22)	
var_dump($float1 + '22.33');		// float(33.55)

$strNum = "123456789";		
var_dump($strNum);					// string(9) "123456789"
var_dump($strNum[2]);				// string(1) "3"
var_dump($strNum{2});				// string(1) "3"

var_dump("very amazing {$strNum[2]}");		// string(14) "very amazing 3"
var_dump($strNum{strlen($strNum)-1});		// string(1) "9"

$strNum{strlen($strNum)-1} = "w";			
var_dump($strNum);							// string(9) "12345678w"

var_dump((int)((0.1+0.7) * 10));			// int(7)
var_dump(((0.1+0.7) * 10));	// float(8)

var_dump(0123);			// int(83)
var_dump("0123");			// string(4) "0123"
var_dump("0123"+0);		// int(123)
var_dump("0123"+2);		// int(125)

var_dump((int)((0.01 + 0.8) * 10)); // int(8)
var_dump((0.01 + 0.8) * 10));// float(8.1)
```



## 基本介绍

### Integer

- 定义：`integer` 是整数集合的某个数，整数集合为 `Z = {...,-2,-1,0,1,2,...}`

- 表示，可以用十进制，十六进制，八进制，二进制表示

	- 十进制：普通数字
	- 十六进制：0x + 普通数字
	- 八进制：0 + 普通数字
	- 二进制：0b + 普通数字
	
- 整型数字长和平台有关，32位系统，最大为`2147483647` （32位有符号），64位系统最大为`9223372036854775807`（64位有符号），不支持无符号的 `integer`，最大值为常量：`PHP_INT_MAX`

- 如果给定的数字超过上一条提到的`integer`的范围，或者运算结果超过了`integer`的范围，则会自动转为 `float `类型

- 可以用`(int)` 或者`(integer)` 强制将某个值明确的转换为整型，也可以通过函数 `intval()`  转换

	- 布尔值转换为整数时，`FLASE`  产出 `0`, `TRUE` 产出`1`
	
	- `float ` 转换为整数时，会`向下取整`，但是如果超过可以转换的整数范围，则结果是未定义的（无效的）
	
	- 不能将未知的分数强制转换为整数，结果有时候会不符合预期，例如：
	
	``` 
	var_dump((int)(0.1 + 0.7) * 10);
	
	输出： 7 而不是 8 
	```

### String

- 定义：一个字符串 string 就是由一系列的字符组成，其中每个字符等同于一个字节

- 表示

	- 单引号包围  ` 'liukaining' `
	
	- 双引号包围 `"liukaining"`
	
	- heredoc 和 newdoc （暂不介绍）

- 单引号字符串特性

	- 可以跨越多行，不对格式做更改
	
	- 要表示单引号自身，需要前面加反斜线用来转义` \'` 
	
	- 要表示反斜线自身，需要两个反斜线 `\\`
	
	- 除以上单引号和反斜线之外，不解析其他的任何转义序列，即不会转义`\r  \n  \t `   等
	
	- 不解析变量，写什么就展示什么
	
-  双引号字符串特性

	- 可以跨越多行，不对格式做更改
	
	- 可以解析 `\n（换行）, \r（回车）, \t（tab）, \v（竖线）, \e（esc）, \f（换页）, \\（反斜线）, \$（美元标记）, \"（双引号）, \[0-7]{1,3}（八进制）,\x[0-9A-Fa-f]{1,2}（十六进制）`   这些转义符号，其他的则不转义
	
	- 可以解析变量
	
		- 简单规则：`遇到美元符号($) 时候，会组合尽量多的标识来形成一个合法的变量名`，可以用花括号`{}` 明确变量名的界线；可以利用简单规则解析的包括变量名、数组的索引、对象的属性
		
		- 复杂规则：花括号语法，即`任何具有 string 表达的标量、变量、数组元素、对象属性等，可以像在 string 以外的地方写出表达式，然后再用花括号｛｝包围起来即可` ，
		
- 字符串的字符可以通过数组索引（方括号包含数字）的方式去访问，即可以把字符串当作是字符数组

- 字符串的字符也可以通过花括号去访问，但不建议使用
```
$str = "liukaining";
echo $str[2];     // 输出 u 
echo $str{3}   // 输出 k
echo $str[strlen($str)-1];   // 输出 g
```

- 字符串连接使点`.` ，而不是加号`+`，一般加号`+` 会对前后做运算

- 转换：一个值可以用`(string)` 或者 `strval()` 函数转变成字符串

	- 如果某个表达式需要字符串，则会自动转换，例如 `echo ` 
	
	- 布尔值 `TRUE` 会被转换成 `"1"`,`FLASE` 会被转换成`""` 
	
	- 整数和浮点数会被转换成字面样式的 string 
	
	- 数组会被转换成字符串`Array`
	
	- 对象会被转换成字符串`Object`
	
	- `NULL` 会被转换成空字符串`""`
	
	- 序列化函数 `serialize() ` 可以将大部分的php值变成string 用作存储格式
	
- 字符串转换为数值规则：

	- 如果该字符串没有非数字的字符，并且数字值在整型范围内，则使用该数值，取`integer` 或者 `float`
	
    - 如果该字符串中有 `. e E - `等，可以视情况考虑正负、小数点、指数等，取 `integer` 或者 `float` 
    
    - 如果该字符串开始的部分是合法的数值，后面是字符串，则取合法的数值部分；合法的数值部分包括正负号、一个或者多个数字、小数点、指数等
    
    - 如果开始部分是非数字的字符串，则取 `0 `
    
    - 字符串转变数字以后如果参与运算，则先按照以上规则取得相应的值，再参与运算  


### 进制转换

- 十进制转换二进制

	- `decbin()`  输入一个十进制，返回二进制的字符串，32位系统能转换的最大数字是 `4294967295`, 结果是 32 个 1； 64位系统能转换的最大数字是 `9223372036854775807`,结果是 63 个 1  
	
	- 负整数最高位是`1` ,  64 位能转的最小的负数整数是  `-9223372036854775807`,结果是首尾是1，中间62个0
 
- 二进制转换十进制

	- `bindec()` 输入一个二进制字符串，返回一个十进制的等价的值
	
	- 输入的二进制字符串都会被解释成无符号的整数，最高位不是符号位

	- 输入的参数必须是字符串

- 在任意进制之间转换数字

	- `base_convert()` 第一个参数是要转换的数值，第二个参数是第一个参数数值的进制，第三个参数是需要转换的进制，返回一个字符串

	- 第二个参数和第三个参数，只能在 `2 和 36` 之间取

	- 处理浮点数会有精度丢失


### 位运算


- 定义

例子 | 名称 | 结果 | 速记
---|---|---|--- 
`$a&$b` | And 按位与 | 把`$a` 和 `$b` 都为`1` 的位设为`1`   | 全 `1` 则 `1`
`$a｜$b`| Or 按位或 | 把`$a` 和 `$b`  任何一个为 `1` 的位设为`1` |有 `1` 则 `1`  
`$a^$b` | Xor 按位异或 |把 `$a` 和 `$b` 一个为`1`另一个为`0` 的位设为`1` |不同则`1` 
`~ $a` | Not 按位取反 | 把 `$a` 中的`0`的位变成`1` ，`1`的位变成`0` |`1` `0` 互换 
`$a<<$b`| Shift left 左移 | 把`$a`的位向左移动`$b`次，每一次都表示`乘以2` |移出则丢，低位补零
`$a>>$b` | Shift right 右移 | 把`$a`的位向右移动`$b`次，每一次都表示`除以2` |移出则丢，正负保留 


- 应用口诀 `清零取位要用与$ ,某位变 1 要用或|,取反交换用异或^`

- 例子  `$num`  是要被操作的数， `$mask` 是构造的中间操作数

	- 清零特定位：`$mask` 特定位置是`0`，其他位置是`1`，执行：` $num = $num & $mask`
	
	- 取特定位：`$mask` 特定位置是`1`，其他位置是`0`，执行：` $num = $num & $mask`
	
	- 特定位置变成`1`，其他位置不变 : `$mask` 特定位置是`1`，其他位置是`0`，执行：` $num = $num | $mask`
	
	- 特定位置取反 : `$mask` 特定位置是`1`，其他位置是`0`，执行：` $num = $num ^ $mask`

	- 判断某个 `int` 的变量是奇数还是偶数  
	```
	$num & 1 == 0  偶数
	$num & 1 == 1  奇数
	```
	
- 应用：权限控制
	
```
define('VIEW',1 << 0);
define('INSERT',1 << 1);
define('UPDATE',1 << 2);
define('DEL',1 << 3);

var_dump(VIEW);   							// 1
var_dump(INSERT);  							// 2
var_dump(UPDATE);  							// 4
var_dump(DEL);     							// 8

$user1 = VIEW | INSERT ;   			 	// 1 + 2 = 3 
$user2 = VIEW | UPDATE ;  			 		// 1 + 4 = 5
$boss = VIEW | INSERT | UPDATE | DEL;    	// 1 + 2 + 4 + 8 = 15


var_dump(($user1 & VIEW) == VIEW);          	// true
var_dump(($user1 & INSERT) == INSERT); 		// true
var_dump(($user1 & UPDATE) == UPDATE); 		// false
var_dump(($user1 & DEL) == DEL);				// false

var_dump(($user2 & VIEW) == VIEW); 		// true
var_dump(($user2 & INSERT) == INSERT);		// false
var_dump(($user2 & UPDATE) == UPDATE);		// true
var_dump(($user2 & DEL) == DEL);			// false

var_dump(($boss & VIEW) == VIEW);			// true
var_dump(($boss & INSERT) == INSERT);		// true
var_dump(($boss & UPDATE) == UPDATE);		// true
var_dump(($boss & DEL) == DEL);			// true

```

- 应用：状态组合

```
define('IS_DEL',1 << 0);            // 1
define('IS_GOOD',1 << 1);			// 2
define('IS_TOP',1 << 2);			// 4
define('IS_HOT',1 << 3);			// 8
define('IS_DISPATCH',1 << 4);		// 16

function do_del($post_status){
    return $post_status | IS_DEL;
}

function do_good($post_status){
    return $post_status | IS_GOOD;
}

function do_top($post_status){
    return $post_status | IS_TOP;
}

function do_hot($post_status){
    return $post_status | IS_HOT;
}
function do_dispatch($post_status){
    return $post_status | IS_DISPATCH;
}

$post_status = 0;			// 初始状态

$post_status = do_good($post_status);			// do good， 0+2=2
$post_status = do_hot($post_status);			// do hot，2+8=10
$post_status = do_top($post_status);			// do top，10+4=14 
							
var_dump($post_status);							// 14

var_dump(($post_status & IS_DEL) == IS_DEL);				// false
var_dump(($post_status & IS_HOT) == IS_HOT);				// true
var_dump(($post_status & IS_GOOD) == IS_GOOD);				// true
var_dump(($post_status & IS_TOP) == IS_TOP);				// true
var_dump(($post_status & IS_DISPATCH) == IS_DISPATCH);		//false
```



## 参考资料



http://php.net/manual/zh/function.base-convert.php

https://juejin.im/entry/58b7caea61ff4b006cd68e1a

http://php.net/manual/zh/language.types.string.php

http://php.net/manual/zh/language.types.integer.php

http://www.111cn.net/phper/php/56158.htm



