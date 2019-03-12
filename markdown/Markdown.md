[#](#) Markdown基础写作

![Markdown logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/64px-Markdown-mark.svg.png)

Markdown，轻量级的标记语言，由John Gruber与Aaron Swartz在2004年共同创造，语法简单，功能多样，如纯文本般易于编写，且实现种类丰富，因而受到广泛的使用。常见的使用场景是，1)日常工作记录，2)用于编写git仓库的README文件，3)编写其他的说明性的文档。

2016年，RFC 7763已正式引入新的MIME（多用途互联网邮件扩展）类型，`text/markdown`，该类型采用原生markdown作为标准。markdown作为一种语言，有很多变种语言，每个变种语言可能采用了不同的语言解释器，所以，同样的纯文本Markdown内容，在不同的环境（语言变种以及解释器）下展现出来的效果可能不同。本文采用Github Flavoured Markdown(GFM)，这是一个Github于2017年发布的Markdown规定，适用于Github平台。其实，GFM的大部分内容对于其他的平台以及解释器也是适用的。

本文档主要介绍Github Flavoured Markdown的基础写作知识。目录如下

1. 标题
2. 引用
3. 图片
4. 链接
    * 文本链接
    * 图片链接
5. 字体
6. 线条
    * 分割线
    * 删除线
7. 列表
    * 有序列表
    * 无序列表
    * 列表嵌套
    * 任务列表
8. 表格
9. 代码
10. 任务列表
11. 表情包
12. HTML标签混用
13. 参考资料

## 标题

采用`#`来表示标题，有6个等级，代表着不同的字体大小

* 纯文本内容，注意`#`后面的空格

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

* 实际效果

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

## 引用

使用`>`符号做内容引用

* 纯文本内容
```
> 四川蜀天梦图数据科技有限公司成立于2018年
```

* 实际效果
> 四川蜀天梦图数据科技有限公司成立于2018年

***注意*** 引用结束的时候，需要在其后放一个空行，以表示引用结束，若不是空行，该内容会仍被纳入引用。

关于引用的嵌套，只需要增加`>`的个数即可形成嵌套
```
> 第一级
>> 第二级
>>> 第三级
```
> 第一级
>> 第二级
>>> 第三级

## 图片

插入图片的语法是`![图片404替换文本](图片URL)`。

* 纯文本内容

```
![github mark](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)
```

* 实际效果

![arch](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)

## 链接

链接分为文本链接和图片链接两种

### 文本链接
文本链接的基础语法是`[链接文字](URL)`或者`[链接文字][链接标签]`，前者十分好理解，后者则主要是为了重复利用同一个URL，这个URL一般定义在文件末尾

| 类型                 | 代码                           | 实例                         |
| :-:                  | :-:                            | :-:                          |
| `[链接文字](URL)`    | `[github](https://github.com)` | [github](https://github.com) |
| [链接文字][链接标签] | `[github][github]`             | [github][github]             |

针对链接标签这种情况，在文件末尾的标签定义如下
```
[github]:https://github.com "github"
```

当然，GFM也支持直接的url，比如 https://github.com，这需要在链接前面放一个空格。

### 图片链接

图片链接其实是文本链接，只是其文本内容被替换为了图片。其语法为`[![图片404替换文字](URL)](链接URL) [![图片404文字](图片URL)][链接标签]`，点击图片时，跳出去的超链接是链接URL或者链接标签对应的超链接URL。

* 纯文本

```
[![github mark](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)](https://github.com/)
```

* 实际效果(点击该图片，将会跳转到github主页)


[![github mark](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png)](https://github.com/)

## 字体

字体主要用`*`控制，不同的数量有不同的含义，具体见下表

| 类型 | 代码       | 实例     |
| :-:  | :-:        | :-:      |
| 斜体 | `*斜体*`   | *斜体*   |
| 粗体 | `**粗体**` | **粗体** |
| 斜粗体|`***斜粗体***`|***斜粗体***|

## 线条

线条分两种，分割线，以及删除线

### 分割线

* 纯文本

```
---
```

* 实际效果

---

### 删除线

* 纯文本
```
~~我是一段被删除的文本~~
```
* 实际效果

~~我是一段被删除的文本~~

## 列表

列表分两种，有序列表以及无序列表，有序列表的规则是`数字 点号 空格 内容`，无序列表的规则是`星号 空格 内容`

### 有序列表

* 纯文本
```
1. C
2. C++
3. Java
```
* 实际效果

1. C
2. C++
3. Java

### 无序列表

* 纯文本
```
* Python
* JavaScript
* Bash
```
实际效果

* Python
* JavaScript
* Bash

### 列表嵌套

列表嵌套，只需将一个列表嵌入另一个列表中即可，可以有序无序列表互相嵌套。注意，嵌套的下一级需要缩进4个空格。

* 纯文本
```
1. 脚本语言
    * Bash
    * Python
    * Groovy
2. 非脚本语言
    1. C
    2. C++
    3. Java
```
* 实际效果

1. 脚本语言
    * Bash
    * Python
    * Groovy
2. 非脚本语言
    1. C
    2. C++

### 任务列表

GFM支持任务列表，用于标记已完成事项和未完成事项。

* 纯文本

```
* [ ] 未完成事项
* [x] 已完成事项
```
* 实际效果

* [ ] 未完成事项
* [x] 已完成事项


## 表格

表格的语法如下，注意表头对齐的方式。

* 纯文本

```
| 表头1 | 表头2 | 表头3 |
| :-    | :-:   | -:    |
| 本内容将居左对齐 | 本内容将居中对齐 | 本内容将居右对齐 |
```

* 实际效果

| 表头1              | 表头2              | 表头3              |
| :-                 | :-:                | -:                 |
| 本列表头将居左对齐 | 本列表头将居中对齐 | 本列表头将居右对齐 |

## 代码

代码分两种，行内代码以及块级代码，行内代码用一对反引号括起来，比如\`var a = b + c;\`，块级代码则用三对反引号括起来，比如下面，在&#96;&#96;&#96;后面的语言代表着对应的代码高亮方案。

* 纯文本

>&#96;&#96;&#96;javascript
>
>var obj = {
>
>  name: 'stmt',
>
>  location: 'TianFu Talant Center'
>
>}
>
>&#96;&#96;&#96;

* 实际效果

```javascript
var obj = {
  name: 'stmt',
  location: 'TianFu Talant Center'
}
```

diff是一种特殊的高亮方案，用于git版本比对的时候，显示删除和新增的代码，只需在新增的和删除的代码行前面写上`+-`即可。

* 纯文本

>&#96;&#96;&#96;diff
>
> \- var date = '2019-2-11'
>
> \+ var date = '2019-2-12'
>
>&#96;&#96;&#96;

* 实际效果

```diff
- var date = '2019-2-11'
+ var date = '2019-2-12'
```
## 表情包

GFM支持表情包，表情包代码可以从[这里](https://github.com/ikatyang/emoji-cheat-sheet)获取，只需要使用`:code:`即可使用表情包，比如戴眼镜的表情包可以用`:sunglasses:`表示:sunglasses:

## HTML标签混用

Github Flavoured Markdown除了基础的语法外，还可以与HTML标签混用，比如 `<span style="color:red; font-size:1.4em">红色字体</span>`就会显示如下，这就给文件排版提供了更大的可能性。

<span style="color:red; font-size:1.4em">红色字体</span>`

## 参考资料
* [Markdown wikipedia](https://en.wikipedia.org/wiki/Markdown)
* [Matering Markdown](https://guides.github.com/features/mastering-markdown/)


[github]:https://github.com "github"
