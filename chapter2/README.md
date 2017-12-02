ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-2-adding-low-level-content](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-2-adding-low-level-content)
## 章节2：添加底层内容
当我们谈论iText文档中的底层内容时，我们总是会引用写入PDF内容流的PDF语法。PDF定义了一系列运算符，比如我们在iText中为其创建moveTo（）方法的m，为其创建了lineTo（）方法的m，以及为其创建了stroke（）方法的S。通过在PDF中组合这些操作数—或者在iText中组合这些方法—您可以绘制路径和形状。

我们来看一个小例子：
```
-406 0 m
406 0 l
S
```
这是一个PDF语法：移动到位置（X = -406; Y = 0），然后构造一个路径到位置（X = 406; Y = 0）；最后笔画这条线—在这个上下文中，“抚摸”是指绘画。如果我们想用iText创建这个PDF语法片段，就像这样：
```
canvas.moveTo(-406, 0)
.lineTo(406, 0)
.stroke();
```
这看起来很简单，不是吗？但是，我们使用的画布对象是什么？让我们来看看几个例子来找出答案。
### 在画布上画直线
假设我们想创建一个如图2.1所示的PDF。

![](https://developers.itextpdf.com/sites/default/files/C02F01.png "图2.1：绘制X和Y轴")
**图2.1：绘制X和Y轴**

显示X和Y轴的PDF是使用Axes示例创建的。让我们一步一步检查这个例子。
```
PdfDocument pdf = new PdfDocument(new PdfWriter(dest));
PageSize ps = PageSize.A4.rotate();
PdfPage page = pdf.addNewPage(ps);
PdfCanvas canvas = new PdfCanvas(page);
// Draw the axes
pdf.close();
```
跳出来的第一件事就是我们不再使用Document对象。就像在前一章中一样，我们创建了一个PdfWriter（第2行）和一个PdfDocument对象，而不是创建一个具有默认或特定页面大小的Document，我们创建一个带有特定PageSize（第2行）的PdfPage（第3行）。在这种情况下，我们使用横向的A4页面。一旦我们有一个PdfPage实例，我们用它来创建一个PdfCanvas（第4行）。我们将使用这个画布对象来创建一个PDF操作符和操作数序列。只要我们完成绘制和绘制任何路径和形状我们要添加到页面，我们关闭PdfDocument（第6行）。
```
在前一章中，我们用document.close（）关闭了Document对象。这隐式关闭了PdfDocument对象。现在没有Document对象，我们必须关闭PdfDocument对象。
```
在PDF中，所有测量均以用户单位完成。默认情况下，一个用户单位对应一个点。这意味着一英寸有72个用户单位。在PDF中，X轴指向右侧，Y轴指向上。如果使用PageSize对象创建页面大小，则坐标系的原点应该位于页面的左下角。所有我们用作操作数的坐标，例如m或l，都使用这个坐标系。我们可以通过改变*当前的变换矩阵*来改变坐标系。
### 坐标系和变换矩阵