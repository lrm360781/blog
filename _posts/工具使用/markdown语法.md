---
title: markdown语法
date: 2019-05-07 23:11:51
tags: 
    - 工具使用
    - .md
---
## 标题
使用“#”表示1-6级标题，一级标题对应一个“#”，n个“#”对应n级标题，上线为6级标题。
语法格式为“#”后面接空格，然后写标题，实例如下：
```yaml
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
使用==--表示标题
```yaml
一级标题
==       #两个=‘等号’以上
二级标题
--       #两个-‘减号’以上

```

## 段落
   markdown段落没有特殊格式，直接编写就行，段落的换行是使用两个以上空格加上回车。
    
   也可以用一个空行来开始新的段落
   
### 字体
```yaml
*斜体文字*
_斜体文本_
**粗体文本**
__斜体文本__
***粗斜体文本***
___粗斜体文本___
```
### 分割线
你可以在一行中用三个以上的 *、-、_来建立一个分隔线，行内不能有其他东西。你也可以在这些符号之间插入空格。
### 删除线
如果段落上的文字要添加删除线，只要在文字的两端加上两个波浪线~~即可，实例如下：

~~测试~~

### 下划线
下划线可以通过html的标签来实现：u标签
<u>测试

## 列表标记
### 无序列表
无序列表使用 * 、 + 、 - 作为列表标记：
```yaml
* 第一项   #前后换行
* 第二项  
* 第三项
```
### 有序列表
有序列表使用数字加上 . 号来表示：
```yaml
1. 第一项
2. 第二项
3. 第三项
```
### 列表嵌套
列表嵌套只需在子列表中的选项添加四个空格即可：
```yaml
1. 第一项：
    - 嵌套元素
    - 嵌套元素
2. 第二项：
    - 嵌套元素
    - 嵌套元素
```
## 区块
区块引用是在段落开头使用>符号，后接一个空格符号:
```yaml
> 区块引用
```
## 代码
如果是段落上的一个函数或片段的代码可以用反引号把它包起来（`）
### 代码区块
代码区块使用 4 个空格或者一个制表符（Tab 键）
你也可以用 ``` 包裹一段代码，并指定一种语言（也可以不指定）


## 链接
链接格式为中括号[链接名称]后接小括号(链接地址)

## 图片链接
[参考我的另外一篇博客](https://www.rms360.top/2019/01/02/hexo%E6%8F%92%E5%85%A5%E5%9B%BE%E7%89%87/)

## 表格
Markword制作表格使用 | 来分隔不同的单元格，使用 - 来分隔表头与其他行。

我们可以设置表格的对齐方式：
* -: 设置内容和标题居右对齐
* :- 设置内容和标题居左对齐
* :-: 设置内容和标题居中对齐