---
layout:  post
title:  Markdown基本语法
tags: markdown
---

### Markdown基本语法

### IDE
 推荐Mou.很多语法Mou的帮助中有的。可以直接导出html pdf格式。而且可以选择导出的主题。
      
### 基本语法
* #### 标题 
 用#进行区分标记，几个代表几级标题，＃和标题前加空格。
* #### 字体  
  把字体放在*或者_的两面。比如用 \*\*强调\*\* 表现为 **强调** 
* #### 列表
  用 * 表示无序，直接数字表示有序列表。列表可以进行嵌套。可以和其他都可以进行嵌套。
* #### 换行
	只是使用换行是不行的。需要进行换行后添加2个或者多个空格。才能显示出换行效果。
* #### 删除
	文字前后各放在2个\~\~.	 表现~~Strikethrough~~
* #### 引用
  用>表示引用。可以使用 > >进行嵌套
* #### 內链代码 * 
	可以把内容放在｀｀内部
* #### 链接
  1. 图片 \![](){ImgCap}{/ImgCap}
  2. 链接地址 用法An \[example](http://url.com/ "Title") 表现An [example](http://url.com/ "Title")
  3. 邮箱 An email \<example@example.com> link. 表现 An email <example@example.com> link.
  4. 锚点 就是在刚才的链接上添加一个id。eg。  \[This is an example](id:anchor1)  使用锚点 Click this \[link](#anchor1) [link](#anchor1) 
* #### 表格

First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell

* #### Header
 使用header1  ======  header2 ------  
* #### 补充说明或者叫脚注
That's some text with a footnote.[
^1]
[^1]: And that's the footnote.  
 