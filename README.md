# Paulus
 Chinese translation of Apple guidance documents.


## 翻译流程

首先认领各自要翻译的章节，然后从 develop 分支切出自己的翻译分支，翻译完之后向 develop 分支提交 merge request 并申请校阅；
校阅完成后合入 develop 分支，再向 main 分支提交 merge request，复审通过之后合入 main 分支。

### 翻译规范

关于标题，大标题使用 h1，二级标题使用 h2，小标题使用 h3，尽量避免使用 h4 及更小的标题。对应 markdown 语法，为 `#`，`##` 和 `###`

关于文件位置，在仓库的根目录下新建要翻译内容的大标题，在该文件夹下以章节名称做为二级目录名，注意需要在名称前面加上章节序号、英文符号“.” 和空格
以 [Auto Layout Guide](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/index.html#//apple_ref/doc/uid/TP40010853-CH7-SW1) 为例，该章节的位置为：
``` javascript
// /${title}/`no. ${chapter name}`/`no. ${post name}`
Auto Layout Guide/1. Getting Started/1. Understanding Auto Layout
```