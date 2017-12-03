ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-1-introducing-basic-building-blocks](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-1-introducing-basic-building-blocks)

## 章节1：介绍基本的构建块

假若时光倒流到2000年，那个时候为了解决两个非常明确的问题，我创建了iText:

1. 在90年代，大多数的PDF文档都是Adobe Illustrator或者Acrobat Distiller这样的桌面应用程序来手动创建的。当时，我需要在一个供互联网用户访问的web应用程序中批量处理PDF文件，以一种无人值守的模式，或者一个批处理模式。这些PDF文件不能通过用户手动操作来生成，因为它们的内容一般都比较庞大，且是不可预知的（需要通过基于用户的输入和数据库查询到的结算计算得出）。在1998年，我写的第一个PDF库解决了这个问题。随后我将这个库部署在一台web服务器上，并且一个部署在这台服务器的Java应用程序通过调用这个库生成了数千份PDF文档。

2. 很快我就碰到了第二个问题：开发人员在没有了解清楚PDF格式规范的情况下，是没有办法使用我第一个PDF库的。事实证明，我是唯一一个在我的团队中能够理解我代码的人。这也意味着如果出了问题，我是唯一一个能够修复代码的人。在一个软件项目中，这可不是一个好的情况。我通过从头到尾重写了一遍那个PDF库来解决这个问题，确保开发人员没有必要一定要了解PDF的格式规范。我是通过引入文档的概念来实现的，所谓文档，就是一个允许开发者添加段落、列表、图片、块和其他高级对象的一个类。通过结合这些直观的构建，一个开发者可以很容易地通过编程的方式来创建一个PDF文档。并且创建PDF文档的代码也变得更加容易阅读和理解，尤其是对于那些没有写过这类代码的人来说。

这只是一个iText7的启动教程，所以我们不会讲太多的细节，但我们可以先从一些包含了这些基本构建块的例子开始讲起。

### 引入iText的基本构建块

很多的编程教程都是以一个Hello World案例来开始，这篇教程也不例外。

以下就是iText7的一个Hello World例子：

```
	PdfWriter writer = new PdfWriter(dest);
	PdfDocument pdf = new PdfDocument(writer);
	Document document = new Document(pdf);
	document.add(new Paragraph("Hello World!"));
	document.close();
```

接下来我们一行一行分析这个例子：

1. 

We create a PdfWriter instance. PdfWriter is an object that can write a PDF file. It doesn't know much about the actual content of the PDF document it is writing. The PdfWriter doesn't know what the document is about, it just writes different file parts and different objects that make up a valid document once the file structure is completed. In this case, we pass a String parameter, named dest, that contains a path to a file, for instance results/chapter01/hello_world.pdf. The constructor also accepts an OutputStream as parameter. For instance: if we wanted to write a web application, we could have created a ServletOutputStream; if we wanted to create a PDF document in memory, we could have used a ByteArrayOutputStream; and so on.


The PdfWriter knows what to write because it listens to a PdfDocument. The PdfDocument manages the content that is added, distributes that content over different pages, and keeps track of whatever information is relevant for that content. In chapter 7, we'll discover that there are various flavors of PdfDocument classes a PdfWriter can listen to.
Once we've created a PdfWriter and a PdfDocument, we're done with all the low-level, PDF-specific code. We create a Document that takes the PdfDocument as parameter. Now that we have the document object, we can forget that we're creating PDF.
We create a Paragraph containing the text "Hello World" and we add that paragraph to the document object.
We close the document. Our PDF has been created.

