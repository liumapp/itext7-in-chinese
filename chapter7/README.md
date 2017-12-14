ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-7-creating-pdfua-and-pdfa-documents](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-7-creating-pdfua-and-pdfa-documents)

## 第7章：创建PDF/UA和PDF/A文档

### 创建可访问的PDF文档

```
PdfDocument pdf = new PdfDocument(new PdfWriter(dest),new WriterProperties().addXmpMetadata()));
Document document = new Document(pdf);
//Setting some required parameters
pdf.setTagged();
pdf.getCatalog().setLang(new PdfString("en-US"));
pdf.getCatalog().setViewerPreferences(
        new PdfViewerPreferences().setDisplayDocTitle(true));
PdfDocumentInfo info = pdf.getDocumentInfo();
info.setTitle("iText7 PDF/UA example");
//Fonts need to be embedded
PdfFont font = PdfFontFactory.createFont(FONT, PdfEncodings.WINANSI, true);
Paragraph p = new Paragraph();
p.setFont(font);
p.add(new Text("The quick brown "));
Image foxImage = new Image(ImageFactory.getImage(FOX));
//PDF/UA: Set alt text
foxImage.getAccessibilityProperties().setAlternateDescription("Fox");
p.add(foxImage);
p.add(" jumps over the lazy ");
Image dogImage = new Image(ImageFactory.getImage(DOG));
//PDF/UA: Set alt text
dogImage.getAccessibilityProperties().setAlternateDescription("Dog");
p.add(dogImage);
document.add(p);
document.close();
```

![Figure 7.1: a PDF/UA document and its structure](https://developers.itextpdf.com/sites/default/files/C07F01.png)
<p align="center">图7.1:一个PDF/UA文档及它的结构</p>

### 创建长期保存的PDF（一）

```
//Initialize PDFA document with output intent
PdfADocument pdf = new PdfADocument(new PdfWriter(dest),
    PdfAConformanceLevel.PDF_A_1B,
    new PdfOutputIntent("Custom", "", "http://www.color.org",
            "sRGB IEC61966-2.1", new FileInputStream(INTENT)));
Document document = new Document(pdf);
//Fonts need to be embedded
PdfFont font = PdfFontFactory.createFont(FONT, PdfEncodings.WINANSI, true);
Paragraph p = new Paragraph();
p.setFont(font);
p.add(new Text("The quick brown "));
Image foxImage = new Image(ImageFactory.getImage(FOX));
p.add(foxImage);
p.add(" jumps over the lazy ");
Image dogImage = new Image(ImageFactory.getImage(DOG));
p.add(dogImage);
document.add(p);
document.close();
```

![Figure 7.2: a PDF/A-1 level B document](https://developers.itextpdf.com/sites/default/files/C07F02.png)
<p align="center">图7.2:一个PDF/A-1的B级文档</p>

```
//Initialize PDFA document with output intent
PdfADocument pdf = new PdfADocument(new PdfWriter(dest),
    PdfAConformanceLevel.PDF_A_1A,
    new PdfOutputIntent("Custom", "", "http://www.color.org",
            "sRGB IEC61966-2.1", new FileInputStream(INTENT)));
Document document = new Document(pdf);
//Setting some required parameters
pdf.setTagged();
//Fonts need to be embedded
PdfFont font = PdfFontFactory.createFont(FONT, PdfEncodings.WINANSI, true);
Paragraph p = new Paragraph();
p.setFont(font);
p.add(new Text("The quick brown "));
Image foxImage = new Image(ImageFactory.getImage(FOX));
//Set alt text
foxImage.getAccessibilityProperties().setAlternateDescription("Fox");
p.add(foxImage);
p.add(" jumps over the lazy ");
Image dogImage = new Image(ImageFactory.getImage(DOG));
//Set alt text
dogImage.getAccessibilityProperties().setAlternateDescription("Dog");
p.add(dogImage);
document.add(p);
document.close();
```

![Figure 7.3: a PDF/A-1 level A document](https://developers.itextpdf.com/sites/default/files/C07F03.png)
<p align="center">图7.3: 一个PDF/A-1的A级文档</p>

### 为长期保存创建PDF(二、三)

```
PdfADocument pdf = new PdfADocument(new PdfWriter(dest),
    PdfAConformanceLevel.PDF_A_3A,
    new PdfOutputIntent("Custom", "", "http://www.color.org",
            "sRGB IEC61966-2.1", new FileInputStream(INTENT)));
Document document = new Document(pdf, PageSize.A4.rotate());
//Setting some required parameters
pdf.setTagged();
pdf.getCatalog().setLang(new PdfString("en-US"));
pdf.getCatalog().setViewerPreferences(
        new PdfViewerPreferences().setDisplayDocTitle(true));
PdfDocumentInfo info = pdf.getDocumentInfo();
info.setTitle("iText7 PDF/A-3 example");
//Add attachment
PdfDictionary parameters = new PdfDictionary();
parameters.put(PdfName.ModDate, new PdfDate().getPdfObject());
PdfFileSpec fileSpec = PdfFileSpec.createEmbeddedFileSpec(
    pdf, Files.readAllBytes(Paths.get(DATA)), "united_states.csv",
    "united_states.csv", new PdfName("text/csv"), parameters,
    PdfName.Data, false);
fileSpec.put(new PdfName("AFRelationship"), new PdfName("Data"));
pdf.addFileAttachment("united_states.csv", fileSpec);
PdfArray array = new PdfArray();
array.add(fileSpec.getPdfObject().getIndirectReference());
pdf.getCatalog().put(new PdfName("AF"), array);
//Embed fonts
PdfFont font = PdfFontFactory.createFont(FONT, true);
PdfFont bold = PdfFontFactory.createFont(BOLD_FONT, true);
// Create content
Table table = new Table(new float[]{4, 1, 3, 4, 3, 3, 3, 3, 1});
table.setWidthPercent(100);
BufferedReader br = new BufferedReader(new FileReader(DATA));
String line = br.readLine();
process(table, line, bold, true);
while ((line = br.readLine()) != null) {
    process(table, line, font, false);
}
br.close();
document.add(table);
//Close document
document.close();
```

![Figure 7.4: a PDF/A-3 level A document](https://developers.itextpdf.com/sites/default/files/C07F04.png)
<p align="center">图7.4:一个PDF/A-3的A级文档</p>

![Figure 7.5: a PDF/A-3 level A document and its attachment](https://developers.itextpdf.com/sites/default/files/C07F05.png)
<p align="center">图7.5:一个PDF/A-3的A级文档及附件</p>

### 合并PDF/A文档

![Figure 7.6: merging 2 PDF/A level A documents](https://developers.itextpdf.com/sites/default/files/C07F06.png)
<p align="center">图7.6:合并2个PDF/A的A级文档</p>

```
PdfADocument pdf = new PdfADocument(new PdfWriter(dest),
    PdfAConformanceLevel.PDF_A_1A,
    new PdfOutputIntent("Custom", "", "http://www.color.org",
            "sRGB IEC61966-2.1", new FileInputStream(INTENT)));
//Setting some required parameters
pdf.setTagged();
pdf.getCatalog().setLang(new PdfString("en-US"));
pdf.getCatalog().setViewerPreferences(
        new PdfViewerPreferences().setDisplayDocTitle(true));
PdfDocumentInfo info = pdf.getDocumentInfo();
info.setTitle("iText7 PDF/A-1a example");
//Create PdfMerger instance
PdfMerger merger = new PdfMerger(pdf);
//Add pages from the first document
PdfDocument firstSourcePdf = new PdfDocument(new PdfReader(SRC1));
merger.addPages(firstSourcePdf, 1, firstSourcePdf.getNumberOfPages());
//Add pages from the second pdf document
PdfDocument secondSourcePdf = new PdfDocument(new PdfReader(SRC2));
merger.addPages(secondSourcePdf, 1, secondSourcePdf.getNumberOfPages());
//Merge
merger.merge();
//Close the documents
firstSourcePdf.close();
secondSourcePdf.close();
pdf.close();
```

### 总结


