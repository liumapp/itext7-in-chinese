ready to translate : [https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-3-using-renderers-and-event-handlers](https://developers.itextpdf.com/content/itext-7-jump-start-tutorial/chapter-3-using-renderers-and-event-handlers)

## 第3章：使用渲染器和事件处理程序

在本教程的第1章中，我们创建了一个具有指定页面大小和页边距的文档（明确或者含蓄的进行了定义），并且在该文档对象中添加了段落和列表等基本的构建块。iText能够确保在页面上很好的组织内容。我们还创建了一个表格对象来导出CSV文件的内容，而且导出的表格样式也相当好看。但如果这些都还不够用呢？如果我们还想要更好的控制内容在页面上的布局呢？如果您对Table类绘制的矩形边框不满意怎么办？如果您想在所有生成页面的指定位置上添加内容怎么办？



Should you draw all the content at absolute positions as was explained in the second chapter to meet these specific requirements? By playing with the Star Wars examples, we've experienced that this could lead to code that is quite complex (and code that is hard to maintain). Surely there must be way to combine the high-level approach using the basic building blocks with a more low-level approach that allows us to have more influence on the layout. That's what this third chapter is about.
你是否应该按照第二章的说明将绝对位置的内容画出来，以满足这些特定的要求？通过玩“星球大战”的例子，我们已经体会到这可能会导致代码非常复杂（代码难以维护）。当然，必须有办法将使用基本构建模块的高层次方法与更低层次的方法相结合，从而使我们对布局有更多的影响。这就是第三章的内容。