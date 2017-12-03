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
<<<<<<< HEAD
	PdfWriter writer = new PdfWriter(dest);
	PdfDocument pdf = new PdfDocument(writer);
	Document document = new Document(pdf);
	document.add(new Paragraph("Hello World!"));
	document.close();
=======
PdfWriter writer = new PdfWriter(dest);
PdfDocument pdf = new PdfDocument(writer);
Document document = new Document(pdf);
document.add(new Paragraph("Hello World!"));
document.close();
>>>>>>> fd3575065a1959e47f96e3814a06e543d8de78a8
```

接下来我们一行一行分析这个例子：

<<<<<<< HEAD
1. 

We create a PdfWriter instance. PdfWriter is an object that can write a PDF file. It doesn't know much about the actual content of the PDF document it is writing. The PdfWriter doesn't know what the document is about, it just writes different file parts and different objects that make up a valid document once the file structure is completed. In this case, we pass a String parameter, named dest, that contains a path to a file, for instance results/chapter01/hello_world.pdf. The constructor also accepts an OutputStream as parameter. For instance: if we wanted to write a web application, we could have created a ServletOutputStream; if we wanted to create a PDF document in memory, we could have used a ByteArrayOutputStream; and so on.


The PdfWriter knows what to write because it listens to a PdfDocument. The PdfDocument manages the content that is added, distributes that content over different pages, and keeps track of whatever information is relevant for that content. In chapter 7, we'll discover that there are various flavors of PdfDocument classes a PdfWriter can listen to.
Once we've created a PdfWriter and a PdfDocument, we're done with all the low-level, PDF-specific code. We create a Document that takes the PdfDocument as parameter. Now that we have the document object, we can forget that we're creating PDF.
We create a Paragraph containing the text "Hello World" and we add that paragraph to the document object.
We close the document. Our PDF has been created.
=======
1. 首先我们实例化了一个PdfWriter的对象writer。writer这个对象可以编写PDF文件，但它不一定很清楚它所创建PDF文档的内容是什么。当文件的结构被确定的时候，writer将编写不同的文件部分和不同的对象，来构成一个合法的文档文件。在这种情况下，我们通过传递一个包含了文件路径，名为dest的字符串参数，例如：results/chapter01/hello_world.pdf。构造函数也会接受一个输出流OutPutStream作为参数。例如：如果我们想写一个web应用程序，我们可以创建一个ServletOutPutStream，如果我们想在内存中创建一个PDF文档，我们可以使用一个ByteArrayOutputStream等等。

2. 一个PdfWriter通过监听PdfDocument来获取应该编写的内容。PdfDocument将会管理那些被添加的，分配在不同页面中的内容，并且去关联跟这些内容相关的信息。在第七章中，我们将会发现一个PdfWriter可以监听一系列不同的PdfDocument类。

3. 一旦我们创建了一个PdfWriter和一个PdfDocument，我们就完成了所有具有PDF特征的低层次代码。在第三行代码中，我们创建了一个以PdfDocument为参数的文档对象，有了它之后，我们就可以不用去管创建PDF什么的了。

4. 第四行代码中，我们创建了一个包含“Hello World”这段文本信息的Paragraph对象，并且将这个对象添加到了文档对象中。

5. 第五行代码我们关闭了文档对象，然后我们的PDF就已经创建好了。

图1.1展示了最终的结果：

![Figure 1.1: Hello World example](https://developers.itextpdf.com/sites/default/files/C01F01.png)
<p align="center">图1.1: Hello World案例</p>

接下来让我们添加一些复杂的内容，比如改改字体，组织一些列表之类的文本，参考图1.2。

![Figure 1.2: List example](https://developers.itextpdf.com/sites/default/files/C01F02.png)
<p align="center">图1.2: List案例</p>

这个例子[RickaStley](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/examples/chapter-1#1724-c01e02_rickastley.java)写明了是如何做到的：

```
PdfWriter writer = new PdfWriter(dest);
PdfDocument pdf = new PdfDocument(writer);
Document document = new Document(pdf);
// Create a PdfFont
PdfFont font = PdfFontFactory.createFont(FontConstants.TIMES_ROMAN);
// Add a Paragraph
document.add(new Paragraph("iText is:").setFont(font));
// Create a List
List list = new List()
    .setSymbolIndent(12)
    .setListSymbol("\u2022")
    .setFont(font);
// Add ListItem objects
list.add(new ListItem("Never gonna give you up"))
    .add(new ListItem("Never gonna let you down"))
    .add(new ListItem("Never gonna run around and desert you"))
    .add(new ListItem("Never gonna make you cry"))
    .add(new ListItem("Never gonna say goodbye"))
    .add(new ListItem("Never gonna tell a lie and hurt you"));
// Add the list
document.add(list);
document.close();
```

1到第3行，以及第22行的代码跟Hello World的示例是相同的，但现在我们已经不是仅仅添加一个段落了。iText通常会使用Helvetica作为默认的文本字体。如果你想换一个字体的话，那么你需要先创建一个PdfFont的实例。你可以通过PdfFontFactory这个类来获得字体（第5行）。我们使用这个字体对象来改变一个段落（第7行）和一个列表（第9行）的字体。这是一个无序列表（第11行），列表项的列表缩进用了12个用户单位（第10行）。然后我们添加了6个列对象（第14－19行）到文档列表中。

这难道不是一件很有意思的事情吗？接下来，让我们介绍一些图片。图1.3介绍了如何添加一张狐狸和一张狗的图像到一个段落中。

![图1.3:Image案例](https://developers.itextpdf.com/sites/default/files/C01F03_0.png)
<p align="center">图1.3:Image案例</p>
>>>>>>> fd3575065a1959e47f96e3814a06e543d8de78a8



