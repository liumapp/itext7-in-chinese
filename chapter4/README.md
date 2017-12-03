ready to translate:[https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-4-making-pdf-interactive](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-4-making-pdf-interactive)
## PDF交互
>标签：[Java](https://developers.itextpdf.com/tags/java)
[annotations](https://developers.itextpdf.com/tags/annotations)
[forms](https://developers.itextpdf.com/tags/forms)
[AcroForm](https://developers.itextpdf.com/tags/acroform)

![](https://developers.itextpdf.com/sites/default/files/C04F01.png "图4.1:一个文本注释
")

**图4.1:一个文本注释**

在前面的章节中，我们通过向页面添加内容来创建PDF文档。如果我们添加高级对象（如段落）或低级指令（例如lineTo（），moveTo（），stroke（）），iText将所有内容转换为写入到其中的PDF语法，更多的内容流。在本章中，我们将添加不同性质的内容。我们将添加交互功能，称为注释。注释不是内容流的一部分。他们通常被添加在现有的内容之上。有许多不同类型的注释，其中许多允许用户交互。
### 添加注释