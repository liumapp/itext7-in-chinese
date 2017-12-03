ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-1-introducing-basic-building-blocks](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-1-introducing-basic-building-blocks)

## 章节1：介绍基本的构建块

假若时光倒流到2000年，那个时候为了解决两个非常明确的问题，我创建了iText:

1. 在90年代，大多数的PDF文档都是Adobe Illustrator或者Acrobat Distiller这样的桌面应用程序来手动创建的。当时，我需要在一个供互联网用户访问的web应用程序中批量处理PDF文件，以一种无人值守的模式，或者一个批处理模式。这些PDF文件不能通过用户手动操作来生成，因为它们的内容一般都比较庞大，且是不可预知的（需要通过基于用户的输入和数据库查询到的结算计算得出）。在1998年，我写的第一个PDF库解决了这个问题。随后我将这个库部署在一台web服务器上，并且一个部署在这台服务器的Java应用程序通过调用这个库生成了数千份PDF文档。

2. 


I soon faced a second problem: a developer couldn't use my first PDF library without consulting the PDF specification. As it turned out, I was the only member in my team who understood my code. This also meant that I was the only person who could fix my code if something broke. That's not a healthy situation in a software project. I solved this problem by rewriting my first PDF library from scratch, ensuring that knowing the PDF specification inside-out became optional instead of mandatory. I achieved this by introducing the concept of a Document to which a developer can add Paragraph, List, Image, Chunk, and other high-level objects. By combining these intuitive building blocks, a developer could easily create a PDF document programmatically. The code to create a PDF document became easier to read and easier to understand, especially by people who didn't write that code.
我很快就面临一个问题:开发人员不能使用我的第一个PDF库没有咨询PDF格式规范。事实证明,我是唯一一个在我的团队成员理解我的代码。这也意味着我是唯一能修复我的代码的人如果东西坏了。这不是一个健康的情况在软件项目中。我解决了这个问题通过重写第一从头PDF库,确保知道PDF规范由内而外成为可选的,而不是强制性的。我实现了这个引入的概念文档,开发人员可以添加段落,列表,形象,块和其他高级对象。通过结合这些直观的构件,开发人员可以很容易地通过编程方式创建一个PDF文档。的代码创建一个PDF文档变得容易阅读和容易理解,特别是那些没有写代码。