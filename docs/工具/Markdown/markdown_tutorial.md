---
title: Markdown入门
---

## 标题

1. 使用 = 和 - 标记一级和二级标题

```md
# 我是一级标题

## 我是二级标题
```

2. 使用#表示 1 - 6 级标题

```md
# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题
```

## 段落

Markdown 的段落没有特殊格式，直接编写文字即可。

### 字体

字体包括一下几种：

```md
_斜体_
**粗体**
**_粗斜体_**
```

### 分割线

可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。

### 删除线

如果段落上的文字要添加删除线，只需要在文字的两端加上两个波浪线 ~~ 即可。

### 下划线

下划线可以通过 HTML 的 u 标签来实现。

```md
<u>带下划线的文本</u>
```

### 脚注

脚注是对文本的补充说明。
Markdown 脚注的格式如下:

```md
[^要注明的文本]
```

## 列表

无序列表使用 `*`、`+` 或 `—` 标记，标记后面需要有一个空格。

```md
- 第一线
- 第二项
- 第三项
```

无序列表使用 `数字`加 `.`表示。

```md
1. first item
2. second item
```

### 列表嵌套

列表嵌套只需在子列表中的选项前面添加两个或四个空格即可：

```md
1. 第一项：
   - 第一项嵌套的第一个元素
   - 第一项嵌套的第二个元素
2. 第二项：
   - 第二项嵌套的第一个元素
   - 第二项嵌套的第二个元素
```

## 区块

Markdown 区块引用是在段落开头使用 > 符号 ，然后后面紧跟一个空格符号。

```md
> 区块引用
> 菜鸟教程
> 学的不仅是技术更是梦想
```

## 代码

1. 如果是段落上的一个函数或片段的代码可以用反引号把它包起来（`）；
2. 你也可以用 ``` 包裹一段代码，并指定一种语言（也可以不指定）；

## 链接

可以使用 `[链接名称](链接地址)`的格式指定链接，也可以直接使用 `<http://example.com>`的形式。

```md
[百度](https://www.baidu.com)

<https://www.baidu.com>
```

### 高级链接

可以通过变量指定链接地址: [链接名称][变量名]，变量的定义放在文档的结尾。

```md
[链接地址][百度]

[百度]: https://www.baidu.com
```

## 图片

图片语法的格式如下：

```md
![alt属性文本](图片地址 '可选标题')
```

目前不支持设置图片大小，如果需要可以使用 html 的 `img`标签。

## 表格

Markdown 制作表格使用 | 来分隔不同的单元格，使用 - 来分隔表头和其他行。

```md
| 表头   | 表头   |
| ------ | ------ |
| 单元格 | 单元格 |
| 单元格 | 单元格 |
```

### 对齐方式

我们可以设置表格的对齐方式：

-: 设置内容和标题栏居右对齐。
:- 设置内容和标题栏居左对齐。
:-: 设置内容和标题栏居中对齐。

```md
| 左对齐 | 右对齐 | 居中对齐 |
| :----- | -----: | :------: |
| 单元格 | 单元格 |  单元格  |
| 单元格 | 单元格 |  单元格  |
```
