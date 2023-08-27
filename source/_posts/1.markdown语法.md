---
title: 1.markdown语法
date: 2023-08-27
tags: ['星海']
categories: 星海
id: 1.markdown语法
---
<!-- more -->
# 1.markdown语法
# 目录
- [1.1快捷键](#section.1)<a name="context.1"> </a>
- [1.2文字(文本)显示](#section.2)<a name="context.2"> </a>
- [2.1无序列表](#section.3)<a name="context.3"> </a>
- [2.2有序列表](#section.4)<a name="context.4"> </a>
- [2.3任务列表](#section.5)<a name="context.5"> </a>
- [4.1行内代码](#section.6)<a name="context.6"> </a>
- [4.2代码块](#section.7)<a name="context.7"> </a>
------------------------------------------------  
## [1.1快捷键](#context.1)<a name="section.1"> </a>

| 名称               | 语法                                     | 快捷键                                                       |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------ |
| 标题               | 用#表示，#一级标题，##二级标题，以此类推 | ctrl+1/2/3/4     ctrl+0快速将文本调整成普通文本     ctrl+加号/减号对标题级别进行加减 |
| 字体加粗           | 用** **包裹起来                          | ctrl+B                                                       |
| 斜体字             | 用* *包裹起来                            | ctrl+L                                                       |
| 加粗斜体           | 用*** ***包裹起来                        | ctrl+B  / ctrl+L                                             |
| 删除线             | 用~~ ~~ 包裹起来                         | shift+atl+5                                                  |
| 下划线             | 用<u> <u>包裹起来                        | ctrl+U                                                       |
| 引用               | 在文字开头添加>表示引用说明              | ctrl+Q                                                       |
| 高亮               | 用== ==包裹起来                          | 无快捷键                                                     |
| 插入表格           |                                          | ctrl+T                                                       |
| 在表格下方插入一行 |                                          | ctrl+enter                                                   |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |
|                    |                                          |                                                              |



## [1.2文字(文本)显示](#context.2)<a name="section.2"> </a>

1. 上下标：

​     `x^2^`

​    `H~2~O`

效果：

​    x^2^

   H~2~O

2.文本居中

代码:

```
<center>内容</kbd>
```

效果：
<center>内容</kbd>

3.快捷键显示
代码：

```
<kbd>内容</kbd>
```

效果：
<kbd>内容</kbd>

4.加粗
代码：

```
<b>加粗</b>
```

效果：
<b>加粗</b>

4.倾斜
代码：

```
<i>倾斜</i>
```

效果：<i>倾斜</i>

5.上下标
代码：

```
开始<sup>123hi你好</sup>
开始<sub>321hi你好</sub>
```

效果：
开始<sup>123hi你好</sup>
开始<sub>321hi你好</sub>

# 2.列表

## [2.1无序列表](#context.3)<a name="section.3"> </a>

代码：

 `*/-/+ + 空格`

效果：

1.只有同一级别：

+ 1

+ 2

+ 3

	

	2.子集类

	- 1
		- 2
			- 3

	快捷键：ctrl+shift+]

## [2.2有序列表](#context.4)<a name="section.4"> </a>

代码：

`数字+.+空格`

效果：

1. 第一个标题

2. 第二个标题

3. 第三个标题

	- 子内容1
	- 子内容2

4. 第四个标题
	快捷键为ctrl+shift+[

## [2.3任务列表](#context.5)<a name="section.5"> </a>

	代码：

	`- [ ]吃早饭`

	`- [x]背单词`
	效果：

	- [ ] 吃早饭

	- [ ] 背单词

快捷键：ctrl+shift+x

# 3.区块显示

代码：

`>+回车`

效果：

>这是最外层代码块
>
>> 这是内层代码块
>
>
>
>> > 这是最内层代码块

# 代码显示

## [4.1行内代码](#context.6)<a name="section.6"> </a>

代码：

`int a=0;`

快捷键：ctrl+shift+`

## [4.2代码块](#context.7)<a name="section.7"> </a>

```
//输出 cout
//输入 cin
```

快捷键：ctrl+shift+K

# 5.链接

代码：

```
www.baidu,com
[百度](https://www.baidu.com)
[百度])(https://www.baidu.com "https://www.baidu.com")
```

效果：
[百度](https://www.baidu.com)
[百度])(https://www.baidu.com "https://www.baidu.com")

# 6.脚注

对文本进行解释说明
代码：

```
[^文本]
[^文本]：解释说明
```

效果：

​    c++[^①]

c[^②]

[^①]:这是一种编程语言
[^②]: 这是一种简单编程语言

# 7.插入图片

代码：

```
![不显示的文字]（图片路径 "图片标题"）
```


效果：![]()

快捷键：ctrl+shift+I

# 8.表格

快捷键：

插入表格：ctrl+T
在最后一行添加一行表格：ctrl+enter
在一行表格里面再添加一行：shift+enter

# 9.流程图



# 10.表情符号

代码：

```
:happy: /:cry: /:man:
```

效果：
:happy: /:cry: /:man:
