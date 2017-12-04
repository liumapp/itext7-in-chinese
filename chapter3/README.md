ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-3-using-renderers-and-event-handlers](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-3-using-renderers-and-event-handlers)

## 第3章：渲染器和事件处理程序

在本教程的第1章中，我们创建了一个具有指定页面大小和页边距的文档（明确或者含蓄的进行了定义），并且在该文档对象中添加了段落和列表等基本的构建块。iText能够确保在页面上很好的组织内容。我们还创建了一个表格对象来导出CSV文件的内容，而且导出的表格样式也相当好看。但如果这些都还不够用呢？如果我们还想要更好的控制内容在页面上的布局呢？如果您对Table类绘制的矩形边框不满意怎么办？如果您想在所有生成页面的指定位置上添加内容怎么办？

在第二章中，为了满足一些特定的需要，你必须写明绝对位置的坐标来绘制内容，但这是否真的有必要呢？通过“星球大战”的例子，我们很清楚这样写的结果就是得到一段难以维护的复杂代码。答案是没有必要的，我们可以通过基本构建模块的高级方法和低级方法混合使用的方式来操作更复杂的页面布局。而如何去操作就是第三章要说明的内容。

### 文档渲染器

假设我们要将一段文本和几张图片添加到一个文档上，但不希望文本横跨整个页面，而是像图3.1那样被分成三列。

![Figure 3.1: Text and images organized in columns](https://developers.itextpdf.com/sites/default/files/C03F01.png)
<p align="center">图3.1: 文本和图片按列排放</p>

具体代码请看[NewYorkTimes](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/examples/chapter-3#1742-c03e01_newyorktimes.java)这个案例：

```
PdfDocument pdf = new PdfDocument(new PdfWriter(dest));
PageSize ps = PageSize.A5;
Document document = new Document(pdf, ps);
 
//Set column parameters
float offSet = 36;
float columnWidth = (ps.getWidth() - offSet * 2 + 10) / 3;
float columnHeight = ps.getHeight() - offSet * 2;
 
//Define column areas
Rectangle[] columns = {
    new Rectangle(offSet - 5, offSet, columnWidth, columnHeight),
    new Rectangle(offSet + columnWidth, offSet, columnWidth, columnHeight),
    new Rectangle(
        offSet + columnWidth * 2 + 5, offSet, columnWidth, columnHeight)};
document.setRenderer(new ColumnDocumentRenderer(document, columns));
 
// adding content
Image inst = new Image(ImageDataFactory.create(INST_IMG)).setWidth(columnWidth);
String articleInstagram = new String(
    Files.readAllBytes(Paths.get(INST_TXT)), StandardCharsets.UTF_8);
 
 // The method addArticle is defined in the full  NewYorkTimes sample
NewYorkTimes.addArticle(document,
    "Instagram May Change Your Feed, Personalizing It With an Algorithm",
    "By MIKE ISAAC MARCH 15, 2016", inst, articleInstagram);
 
document.close();
```

前面五行代码是非常标准的样板代码，在第一章的时候就已经介绍过了。从第五行开始，我们定义了几个新的参数：

* 代码中的offSet这个变量，被用于定义顶部和底部的边距，以及右侧和左侧的边距。

* columnWidth这个变量代表每一列的宽度，这个值是通过将可用页面宽度除以3计算得出（因为我们只要3列）。这句话的“可用页面宽度”是整个页面宽度减去左右边距，再从边距中减去5个用户单位，以便列与列之间可以又一个小的间距。

* columnHeight这个变量代表每一列的高度，通过整个页面的高度减去顶部和底部的边距计算得出。

我们接下来使用这些变量又定义了三个Rectangle矩形对象：

* 第一个矩形的左下角坐标为（X = offSet - 5 , Y = offSet），宽为columnWidth，高为columnHeight。

* 第二个矩形的左下角坐标为（X = offSet + columnWidth , Y = offSet），宽为columnWidth，高为columnHeight。

* 第三个矩形的左下角坐标为（X = offSet + columnWidth * 2 + 5 , Y = offSet），宽为columnWidth，高为heightColumn。

我们把这三个Rectangle矩形对象放置在一个名为columns的数组中，并把它作为构造函数的参数来实例化一个ColumnDocumentRenderer。新实例化出来的ColumnDocumentRenderer又被作为DocumentRenderer的类传入我们的文档实例中，接下来，我们添加到文档的所有内容都将被放置在刚定义的三个Rectangle所对应的列中。

在第19行，我们创建了一个Image对象并把它进行了适当的缩放以适应列的宽度。在第20和21行中，我们读取了一个文本文件的内容并放入一个字符串变量中。这一系列的对象将被作为我们自定义方法addArticle()的参数。

```
public static void addArticle(
    Document doc, String title, String author, Image img, String text)
    throws IOException {
    Paragraph p1 = new Paragraph(title)
            .setFont(timesNewRomanBold)
            .setFontSize(14);
    doc.add(p1);
    doc.add(img);
    Paragraph p2 = new Paragraph()
            .setFont(timesNewRoman)
            .setFontSize(7)
            .setFontColor(Color.GRAY)
            .add(author);
    doc.add(p2);
    Paragraph p3 = new Paragraph()
            .setFont(timesNewRoman)
            .setFontSize(10)
            .add(text);
    doc.add(p3);
}
```

上述代码并没有引入新的概念。timesNewRoman和timesNewRomanBold这两个PdfFont对象都是NewYorkTimes类的静态成员变量，这样的形式去引用字体比我们上一章的做法要更加容易。接下来，看一个稍微复杂一点的例子。

### 方块渲染器的使用

之前的案例中，有一个包含美国各州信息的CSV文件被我们读取内容发布到PDF上来，为此，我们在表格对象中创建了一系列的单元格对象，但我们没有定义背景颜色，也没有定义边框的样式，全部都是使用的默认值。




![Figure 3.2: a table with colored cells and rounded borders](https://developers.itextpdf.com/sites/default/files/C03F02_0.png)

![Figure 3.3: repeating background color and watermark](https://developers.itextpdf.com/sites/default/files/C03F03_1.png)

