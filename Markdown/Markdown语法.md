[在 VSCode 中用 Markdown 做「数字化」学习笔记](https://orangex4.cool/post/notes-in-markdown)

[MPE使用教程](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/)

# 一级标题

## 二级标题

### 三级标题

每写完一个段落要隔一行空行.

就像这样, 隔了一行空行.

---
分割线
**重点加粗**
*斜体*
~~删除线~~
==高亮==

---

列表:

* 无序列表
  * 嵌套无序列表
  * 嵌套无序列表
* 无序列表
* 无序列表

1. 有序列表 1
   1. 嵌套有序列表 1
   2. 嵌套有序列表 2
2. 有序列表 2
3. 有序列表 3

---

引用文本:

> 引用别人说的话
> 就这样写
> By. OrangeX4

---

这是 `行内代码` 语法.

代码块语法:

```python
print("Hello, World!")
```

代码行数的显示:
``` javascript {.line-numbers}
function add(x, y) {
  return x + y
}
```

支持语言：objectivec，swift，javescript，java，c,python

---

使用网络图片：
***只显示文字，超链接***
[OrangeX4's Blog](https://pic2.zhimg.com/80/v2-c35303b43639b37a18b8893e906d7435_720w.png)

***显示图片，不显示文字***
![OrangeX's Avatar](https://orangex4.cool/images/icons/profile.jpg)


也可以在本地用相对地址:
[Other](other.md)
![OrangeX's Avatar](images/profile.jpg)

---

表格:

| 表头 | 表头 |
| ---- | ---- |
| 内容 | 内容 |
| 内容 | 内容 |

---

注释:

<!-- 你看不见我 -->