---
title: 正则表达式
date: 2017-09-14
tags:
- 正则表达式
---

[Javascript 标准参考教程 - RegExp对象 by 阮一峰](http://javascript.ruanyifeng.com/stdlib/regexp.html)


## 写法	
1. ` var regex = /xyz/i;  // 字面量 `
2. ` var regex = new RegExp('xyz', 'i');  // RegExp构造函数 `

区别：

两种方式新建正则表达式的时机不一样；
字面量写法在编译时新建正则表达式，第二种在运行时新建。


## 属性
- regex.ignoreCase
- regex.global
- regex.multiline
- regex.lastIndex
- regex.source


## 使用方式
- regex.test(string)
- string.match(regex)

### regex.test(str) : boolean

```
/cat/.test('cats and dogs') // true
```
> 1. 如果正则表达式带有 g 修饰符，则每一次 test 方法都从上一次结束的位置开始向后匹配。
> 2. 如果正则模式是一个空字符串，则匹配所有字符串。

```
new RegExp('').test('abc')  // true
```

### str.match(regex) : array || null
lastIndex 无效


## 其他方法
### regex.exec(str) : array || null
返回每一个匹配成功的子字符串
> 如果正则表示式包含圆括号（即含有“组匹配”），则返回的数组会包括多个成员。第一个成员是整个匹配成功的结果，后面的成员就是圆括号对应的匹配成功的组。也就是说，第二个成员对应第一个括号，第三个成员对应第二个括号，以此类推。整个数组的length属性等于组匹配的数量再加1。

```
var s = '_x_x';
var r = /_(x)/;

r.exec(s) // ["_x", "x"]
```

```
var r = /a(b+)a/;
var arr = r.exec('_abbba_aba_');

arr // ["abbba", "bbb"]

arr.index // 1
arr.input // "_abbba_aba_"
```

### str.search(regex) : int
- 返回 index 或者 -1
- -g & lastIndex 无效

### str.replace(regex, replacement) ：string
- -g 替换所有匹配，否则替换第一个匹配
- $ replace方法的第二个参数可以使用美元符号$，用来指代所替换的内容。
	- $& 指代匹配的子字符串。
	- $` 指代匹配结果前面的文本。
	- $' 指代匹配结果后面的文本。
	- $n 指代匹配成功的第n组内容，n是从1开始的自然数。
	- $$ 指代美元符号$。
	
	```
	'hello world'.replace(/(\w+)\s(\w+)/, '$2 $1')
	// "world hello"
	
	'abc'.replace('b', '[$`-$&-$\']')
	// "a[a-b-c]c"
	```
- > replace方法的第二个参数还可以是一个函数，将每一个匹配内容替换为函数返回值。
```
'3 and 5'.replace(/[0-9]+/g, match => 2 * match) // "6 and 10"
```
- 函数多个参数
	> 作为replace方法第二个参数的替换函数，可以接受多个参数。第一个参数是捕捉到的内容，第二个参数是捕捉到的组匹配（有多少个组匹配，就有多少个对应的参数）。此外，最后还可以添加两个参数，倒数第二个参数是捕捉到的内容在整个字符串中的位置（比如从第五个位置开始），最后一个参数是原字符串。
	
	```
	template.replace(
	  /(<span id=")(.*?)(">)(<\/span>)/g,
	  function(match, $1, $2, $3, $4){
	    return $1 + $2 + $3 + prices[$2] + $4;
	  }
	);
	```

### str.split(regex, numOfArray) : array

## 元字符
### 点字符(.)
> 点字符（.）匹配除回车（\r）、换行(\n) 、行分隔符（\u2028）和段分隔符（\u2029）以外的所有单个字符。

### 位置字符
- ^ 表示开头
- $ 表示结尾
` /^test$/.test('test') // true // 从开始位置到结束位置只有 test `

### 选择符 |
- /a( |t)b/.test('atb') // true

### 转义符 \
> 需要特别注意的是，如果使用RegExp方法生成正则对象，转义需要使用两个斜杠，因为字符串内部会先转义一次。

以下字符需要转义

- ^
- .
- [
- $
- (
- )
- |
- *
- +
- ?
- {
- \

### 特殊字符
- \cX 表示Ctrl-[X]，其中的X是A-Z之中任一个英文字母，用来匹配控制字符。
- [\b] 匹配退格键(U+0008)，不要与\b混淆。
- \n 匹配换行键。
- \r 匹配回车键。
- \t 匹配制表符tab（U+0009）。
- \v 匹配垂直制表符（U+000B）。
- \f 匹配换页符（U+000C）。
- \0 匹配null字符（U+0000）。
- \xhh 匹配一个以两位十六进制数（\x00-\xFF）表示的字符。
- \uhhhh 匹配一个以四位十六进制数（\u0000-\uFFFF）表示的unicode字符。

### 字符类

#### 
- [] 字符类  // `/[abc]/.test('apple') // true` 包含类中的字符
- ^ 脱字符  // `/[^abc]/.test('apple') // false` 
- - 连字符  // /[a-z]/.test('b') // true

### 预定义模式
- \d 匹配0-9之间的任一数字，相当于[0-9]。
- \D 匹配所有0-9以外的字符，相当于[^0-9]。
- \w 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]。
- \W 除所有字母、数字和下划线以外的字符，相当于[^A-Za-z0-9_]。
- \s 匹配空格（包括制表符、空格符、断行符等），相等于[\t\r\n\v\f]。
- \S 匹配非空格的字符，相当于[^\t\r\n\v\f]。
- \b 匹配词的边界。
- \B 匹配非词边界，即在词的内部。

### 重复类
> 模式的精确匹配次数，使用大括号（{}）表示。{n} 表示恰好重复 n 次，{n,} 表示至少重复 n 次，{n,m} 表示重复不少于 n 次，不多于 m 次。

```
/lo{2}k/.test('look') // true
/lo{2,5}k/.test('looook') // true
```

### 量词符
- ? 问号表示某个模式出现0次或1次，等同于 {0, 1}。
- * 星号表示某个模式出现0次或多次，等同于 {0,}。
- + 加号表示某个模式出现1次或多次，等同于 {1,}。

### 贪婪模式
> 最大可能匹配，即匹配直到下一个字符不满足匹配规则为止。

- *?：表示某个模式出现0次或多次，匹配时采用非贪婪模式。
- +?：表示某个模式出现1次或多次，匹配时采用非贪婪模式。

### 修饰符
- g // global 从上一次匹配处往下匹配，可用于搜索和替换
- i // ignoreCase 忽略大小写
- m // multiline

	```
	/world$/.test('hello world\n') // false
	/world$/m.test('hello world\n') // true
	/^b/m.test('a\nb') // true
	```
	
### 组匹配
> 正则表达式的括号表示分组匹配，括号中的模式可以用来匹配分组的内容。

```
/fred+/.test('fredd') // true
/(fred)+/.test('fredfred') // true
```
```
var tagName = /<([^>]+)>[^<]*<\/\1>/;

tagName.exec("<b>bold</b>")[1]
// 'b'
```

#### 非捕获组
> (?:x)称为非捕获组（Non-capturing group），表示不返回该组匹配的内容，即匹配的结果中不计入这个括号。

```
var m = 'abc'.match(/(?:.)b(.)/);
m // ["abc", "c"]
```

#### 先行断言
> x(?=y)称为先行断言（Positive look-ahead），x只有在y前面才匹配，y不会被计入返回结果。比如，要匹配后面跟着百分号的数字，可以写成/\d+(?=%)/。

#### 先行否定断言
> x(?!y)称为先行否定断言（Negative look-ahead），x只有不在y前面才匹配，y不会被计入返回结果。比如，要匹配后面跟的不是百分号的数字，就要写成/\d+(?!%)/。