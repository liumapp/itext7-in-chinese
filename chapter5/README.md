ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-5-manipulating-existing-pdf-document](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-5-manipulating-existing-pdf-document)

## 第5章：操纵一个现有的PDF文档

第1章到第3章的例子中，我们总是从头开始用iText创建一个新的PDF文档。第四章的最后几个例子中用的是一个现有的PDF文档，通过对这个PDF表格进行数据的填写，让它不再具有交互性或者带了一些预设值。接下来的第五章里，我们将继续使用现有的PDF。首先通过PdfReader加载一个现有的文件，然后用reader对象来创建一个新的PdfDocument。

### 添加注解及内容

在前面的章节中，我们使用了一个现有的PDF表单job_application.pdf，并填写了其中的相关字段。在这一章中，我们会更进一步。我们将开始添加一个文本注解，一些文本和一个新的复选框。如图5.1所示。

![Figure 5.1: an updated form](https://developers.itextpdf.com/sites/default/files/C05F01.png)
<p align="center">图5.1:可修改的表单</p>

我们将重复之前AddAnnotationsAndContent例子中的代码。

```
PdfDocument pdfDoc =
    new PdfDocument(new PdfReader(src), new PdfWriter(dest));
// add content
pdfDoc.close();
```

```
PdfAnnotation ann = new PdfTextAnnotation(new Rectangle(400, 795, 0, 0))
    .setTitle(new PdfString("iText"))
    .setContents("Please, fill out the form.")
    .setOpen(true);
pdfDoc.getFirstPage().addAnnotation(ann);
```

```
PdfCanvas canvas = new PdfCanvas(pdfDoc.getFirstPage());
canvas.beginText().setFontAndSize(
        PdfFontFactory.createFont(FontConstants.HELVETICA), 12)
        .moveText(265, 597)
        .showText("I agree to the terms and conditions.")
        .endText();
```

```
PdfAcroForm form = PdfAcroForm.getAcroForm(pdfDoc, true);
PdfButtonFormField checkField = PdfFormField.createCheckBox(
        pdfDoc, new Rectangle(245, 594, 15, 15),
        "agreement", "Off", PdfFormField.TYPE_CHECK);
checkField.setRequired(true);
form.addField(checkField);
```

```
form.getField("reset").setAction(PdfAction.createResetForm(
    new String[]{"name", "language", "experience1", "experience2",
    "experience3", "shift", "info", "agreement"}, 0));
```

### 修改表单字段属性

```
PdfAcroForm form = PdfAcroForm.getAcroForm(pdfDoc, true);
Map<String, PdfFormField> fields = form.getFormFields();
fields.get("name").setValue("James Bond").setBackgroundColor(Color.ORANGE);
fields.get("language").setValue("English");
fields.get("experience1").setValue("Yes");
fields.get("experience2").setValue("Yes");
fields.get("experience3").setValue("Yes");
List<PdfString> options = new ArrayList<PdfString>();
options.add(new PdfString("Any"));
options.add(new PdfString("8.30 am - 12.30 pm"));
options.add(new PdfString("12.30 pm - 4.30 pm"));
options.add(new PdfString("4.30 pm - 8.30 pm"));
options.add(new PdfString("8.30 pm - 12.30 am"));
options.add(new PdfString("12.30 am - 4.30 am"));
options.add(new PdfString("4.30 am - 8.30 am"));
PdfArray arr = new PdfArray(options);
fields.get("shift").setOptions(arr);
fields.get("shift").setValue("Any");
PdfFont courier = PdfFontFactory.createFont(FontConstants.COURIER);
fields.get("info")
    .setValue("I was 38 years old when I became a 007 agent.", courier, 7);
```

![Figure 5.2: updated form with highlighted fields](https://developers.itextpdf.com/sites/default/files/C05F02.png)
<p align="center">图5.2:用高亮字段修改表单</p>

![Figure 5.3: updated form, no highlighting](https://developers.itextpdf.com/sites/default/files/C05F03.png
<p align="center">图5.3:不用高亮字段修改表单</p>

### 添加页眉、页脚及水印

![Figure 5.4: UFO sightings report](https://developers.itextpdf.com/sites/default/files/C05F04.png)
<p align="center">图5.4:UFO目击报告</p>

![Figure 5.5: UFO sightings report with header, footer, and watermark](https://developers.itextpdf.com/sites/default/files/C05F05_0.png)
<p align="center">图5.5:有页眉、页脚及水印的UFO目击报告</p>

```
PdfDocument pdfDoc =
    new PdfDocument(new PdfReader(src), new PdfWriter(dest));
Document document = new Document(pdfDoc);
Rectangle pageSize;
PdfCanvas canvas;
int n = pdfDoc.getNumberOfPages();
for (int i = 1; i <= n; i++) {
    PdfPage page = pdfDoc.getPage(i);
    pageSize = page.getPageSize();
    canvas = new PdfCanvas(page);
    // add new content
}
pdfDoc.close();
```

```
//Draw header text
canvas.beginText().setFontAndSize(
        PdfFontFactory.createFont(FontConstants.HELVETICA), 7)
        .moveText(pageSize.getWidth() / 2 - 24, pageSize.getHeight() - 10)
        .showText("I want to believe")
        .endText();
//Draw footer line
canvas.setStrokeColor(Color.BLACK)
        .setLineWidth(.2f)
        .moveTo(pageSize.getWidth() / 2 - 30, 20)
        .lineTo(pageSize.getWidth() / 2 + 30, 20).stroke();
//Draw page number
canvas.beginText().setFontAndSize(
        PdfFontFactory.createFont(FontConstants.HELVETICA), 7)
        .moveText(pageSize.getWidth() / 2 - 7, 10)
        .showText(String.valueOf(i))
        .showText(" of ")
        .showText(String.valueOf(n))
        .endText();
//Draw watermark
Paragraph p = new Paragraph("CONFIDENTIAL").setFontSize(60);
canvas.saveState();
PdfExtGState gs1 = new PdfExtGState().setFillOpacity(0.2f);
canvas.setExtGState(gs1);
document.showTextAligned(p,
        pageSize.getWidth() / 2, pageSize.getHeight() / 2,
        pdfDoc.getPageNumber(page),
        TextAlignment.CENTER, VerticalAlignment.MIDDLE, 45);
canvas.restoreState();
```

### 修改页面大小和方向

![Figure 5.6: changed page size and orientation](https://developers.itextpdf.com/sites/default/files/C05F06.png)
<p align="center">图5.6:修改页面大小和方向</p>

```
PdfDocument pdfDoc =
    new PdfDocument(new PdfReader(src), new PdfWriter(dest));
float margin = 72;
for (int i = 1; i <= pdfDoc.getNumberOfPages(); i++) {
    PdfPage page = pdfDoc.getPage(i);
    // change page size
    Rectangle mediaBox = page.getMediaBox();
    Rectangle newMediaBox = new Rectangle(
            mediaBox.getLeft() - margin, mediaBox.getBottom() - margin,
            mediaBox.getWidth() + margin * 2, mediaBox.getHeight() + margin * 2);
    page.setMediaBox(newMediaBox);
    // add border
    PdfCanvas over = new PdfCanvas(page);
    over.setStrokeColor(Color.GRAY);
    over.rectangle(mediaBox.getLeft(), mediaBox.getBottom(),
            mediaBox.getWidth(), mediaBox.getHeight());
    over.stroke();
    // change rotation of the even pages
    if (i % 2 == 0) {
        page.setRotation(180);
    }
}
pdfDoc.close();
```

### 总结