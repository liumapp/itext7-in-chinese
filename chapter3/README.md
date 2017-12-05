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

> 一般而言，一个表单的单元格没有背景颜色，边框也是由0.5个用户单位的黑色矩形组成。

我们现在把另外一个[premier_league.csv](http://gitlab.itextsupport.com/itext7/samples/raw/develop/publications/jumpstart/src/main/resources/data/premier_league.csv)数据源放入一个表格中，但是这次稍微做了一些调整，请参考图3.2.

![Figure 3.2: a table with colored cells and rounded borders](https://developers.itextpdf.com/sites/default/files/C03F02_0.png)
<p align="center">图3.2:五颜六色的表格</p>

我们不会重复样板代码，因为它与之前例子中的代码相同，除了这一行：

```
PageSize ps = new PageSize(842, 680);
```

我们在之前的案例都是用的A4纸大小，这次用自定义的842 * 680 pt（17.7 ＊ 9.4 in）。写起来就跟[PremierLeague](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/examples/chapter-3#1743-c03e02_premierleague.java)这个案例所示范的那样简单。

```
PdfFont font = PdfFontFactory.createFont(FontConstants.HELVETICA);
PdfFont bold = PdfFontFactory.createFont(FontConstants.HELVETICA_BOLD);
Table table = new Table(new float[]{1.5f, 7, 2, 2, 2, 2, 3, 4, 4, 2});
table.setWidthPercent(100)
        .setTextAlignment(TextAlignment.CENTER)
        .setHorizontalAlignment(HorizontalAlignment.CENTER);
BufferedReader br = new BufferedReader(new FileReader(DATA));
String line = br.readLine();
process(table, line, bold, true);
while ((line = br.readLine()) != null) {
    process(table, line, font, false);
}
br.close();
document.add(table);
```

上面的代码跟这个[UnitedStates](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/examples/chapter-1#1726-c01e04_unitedstates.java)例子比起来，只有细微的差别。在这个例子中，我们把表格的文本内容设置为了居中对齐，并修改了表格本身的水平对齐方式，当然，这并不重要，因为表格本来就占用了可用宽度的100%。下面要说的这个process()方法相对来说要更有趣一点。

```
public void process(Table table, String line, PdfFont font, boolean isHeader) {
    StringTokenizer tokenizer = new StringTokenizer(line, ";");
    int columnNumber = 0;
    while (tokenizer.hasMoreTokens()) {
        if (isHeader) {
            Cell cell = new Cell().add(new Paragraph(tokenizer.nextToken()));
            cell.setNextRenderer(new RoundedCornersCellRenderer(cell));
            cell.setPadding(5).setBorder(null);
            table.addHeaderCell(cell);
        } else {
            columnNumber++;
            Cell cell = new Cell().add(new Paragraph(tokenizer.nextToken()));
            cell.setFont(font).setBorder(new SolidBorder(Color.BLACK, 0.5f));
            switch (columnNumber) {
                case 4:
                    cell.setBackgroundColor(greenColor);
                    break;
                case 5:
                    cell.setBackgroundColor(yellowColor);
                    break;
                case 6:
                    cell.setBackgroundColor(redColor);
                    break;
                default:
                    cell.setBackgroundColor(blueColor);
                    break;
            }
            table.addCell(cell);
        }
    }
}
```

先从最普通的单元格开始说起，在第16，19，22以及25行中，我们根据列号改变了背景颜色。

在第13行，设置了单元格内容的字体，并使用setBorder()方法替换了默认的边框，这个方法将边框重新定义为一个0.5pt线宽的黑色实边框。

> SolidBorder是Border类的一个子类，它也有像DashedBorder、DottedBorder、DoubleBorder等等这样的兄弟类。如果您在这些类中找不到想要的边界类，您可以自行对它们进行扩展，这些现有的实现应该可以为您提供一些灵感，比如，您可以通过创建自己的CellRenderer来实现。

我们在第7行使用了一个自定义的RoundedCornersCellRenderer()，在第8行，给单元格的内容定义了填充色，然后把边框设置为null。如果setBorder(null)这个方法不存在，则会绘制两个边框：一个是由iText自己绘制的，一个是由我们下面将要说的单元格渲染器来绘制。

```
private class RoundedCornersCellRenderer extends CellRenderer {
    public RoundedCornersCellRenderer(Cell modelElement) {
        super(modelElement);
    }
 
    @Override
    public void drawBorder(DrawContext drawContext) {
        Rectangle rectangle = getOccupiedAreaBBox();
        float llx = rectangle.getX() + 1;
        float lly = rectangle.getY() + 1;
        float urx = rectangle.getX() + getOccupiedAreaBBox().getWidth() - 1;
        float ury = rectangle.getY() + getOccupiedAreaBBox().getHeight() - 1;
        PdfCanvas canvas = drawContext.getCanvas();
        float r = 4;
        float b = 0.4477f;
        canvas.moveTo(llx, lly).lineTo(urx, lly).lineTo(urx, ury - r)
                .curveTo(urx, ury - r * b, urx - r * b, ury, urx - r, ury)
                .lineTo(llx + r, ury)
                .curveTo(llx + r * b, ury, llx, ury - r * b, llx, ury - r)
                .lineTo(llx, lly).stroke();
        super.drawBorder(drawContext);
    }
}
```

CellRenderer类是BlockRenderer类的特殊实现。

> BlockRenderer类可以用在段落、列表这样的块元素上。继承自它的渲染器类也允许您通过重写draw()方法来自定义功能，比如：创建一个段落的的自定义背景。ps:CellRenderer也有一个drawBorder()方法噢。

我们通过重写drawBorder()方法来绘制一个顶部更丰满的矩形（第6到21行）。getOccupiedAreaBBox()这个方法返回了一个Rectangle矩形对象，可以使用它来找到块元素的边界框(第8行)。像getX()，getY()，getWidth()以及getHeight()这样的方法就是用来定义单元格的左下角和右上角的坐标(第9-12行)。

drawContext这个对象的getCanvas()方法允许我们获取一个PdfCanvas的实例（第13行）。我们用一系列直线和曲线把边界绘制了出来（14-20行）。上面写的例子就很好的演示了：在绘制由单元格组成的表的过程中，如何把相关对象的高级方法和我们之前几乎手动创建PDF的低级方法相结合，以便精确地绘制出符合需求的边界。

> 虽然绘制曲线的代码涉及到一些数学知识，但也不是什么很深奥的东西。大部分常见的边框的种类也都被iText包含在内了，所以您也不必担心引擎里面包含的数学知识。

关于BlockRenderer还有很多的内容需要介绍，但我们就此打住，把那些知识留在下一篇教程里去详细叙述。接下来，让我们用一在每一个页面上自动添加背景、页眉、页脚、水印和页码的例子来结束这一章节。

### 事件处理

![Figure 3.3: repeating background color and watermark](https://developers.itextpdf.com/sites/default/files/C03F03_1.png)



