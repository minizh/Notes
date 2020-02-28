
###1. 标题

####a. 一级标题
```
	方式一： #1.一级标题
	方式二： 一级标题+分割线
		   =============
```

####b. 二级标题加分割线

```
二级标题
-------
```
####c. 1-6标题：
```
#1级标题
##
...
######6级标题
```

###3. 分割线
```
*****
-----
```

###4. 首行缩进 

```
中文： &emsp; 
英文： &nbsp;
```
  &emsp;&emsp;段落的前后要有空行，所谓的空行  
  是指没有文字内容。若想在段内强制换行的方式是使用两个以上空格加上回车（引用中换行省略回车）。

###5. 区块引用
在段落的每行或者只在第一行使用符号>,还可使用多个嵌套引用，如：
```
> 区块引用   
>> 嵌套引用
```

效果：
> 区块引用
>> 嵌套引用

###6. 代码区块
	代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。  
如普通段落：
 
首尾加制表符 

```
void main()
{
	printf("Hello, Markdown.");
}
```
**每行加四个空格 < *首尾空需空行隔开* >：**

    void main()
	{
		printf("Hello, Markdown.");
	}

	<div class="footer">
   		© 2004 Foo Corporation
	</div>
	
###7. 强调
在强调内容两侧分别加上*或者_，如：

	*斜体*，_斜体_
	**粗体**，__粗体__

效果：

*斜体*，_斜体_  
**粗体**，__粗体__

###8. 列表
使用·、+、或-标记**无序列表**，如：

	-（+*） 第一项 -（+*） 第二项 - （+*）第三项

*`注意：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。`*

效果：

- 第一项  
- 第二项  
- 第三项

**有序列表**的标记方式是将上述的符号换成数字,并辅以.，如：

	1. 第一项
	2. 第二项
	3. 第三项

效果：

1. 第一项
2. 第二项
3. 第三项

###9.链接
链接可以由两种形式生成：行内式和参考式。
行内式：

	[younghz的Markdown库](https://github.com/younghz/Markdown "Markdown")。

效果：

[younghz的Markdown库](https://github.com/younghz/Markdown "Markdown")。

参考式：

	[younghz的Markdown库1][1]
	[younghz的Markdown库2][2]
	[1]:https://github.com/younghz/Markdown "Markdown"
	[2]:https://github.com/younghz/Markdown "Markdown"

效果：

[younghz的Markdown库1][1]  
[younghz的Markdown库2][2]
[1]:https://github.com/younghz/Markdown "Markdown"
[2]:https://github.com/younghz/Markdown "Markdown"


>注意： 上述的[1]:https:://github.com/younghz/Markdown "Markdown"不出现在区块中   

###10. 图片
添加图片的形式和链接相似，只需在链接的基础上前方加一个！。

	![百度一下](https://www.baidu.com/img/bd_logo1.png "百度一下")

[![百度一下](C:\Users\zwx571841\Desktop\bd_logo1.png "百度一下")](http://www.baidu.com)

###11. 反斜杠\
相当于反转义作用。使符号成为普通符号。

	\``
	\&nbsp;

效果：  

\``  
\&nbsp;  



###12. 符号 \`
起到标记作用。如：

	`ctrl+a`

效果：

`ctrl+a`
