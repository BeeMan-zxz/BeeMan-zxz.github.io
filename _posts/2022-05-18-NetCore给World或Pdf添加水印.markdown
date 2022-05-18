---
layout:     post
title:      "NetCore给World或Pdf添加水印"
subtitle:   "NetCore给World或Pdf添加水印"
date:       2022-05-18 15:00
author:     "ZXZ"
header-style:   "text"
header-img: "img/post-bg-rwd.jpg"
tags:
    - pdf
    - asp.net core
    - world
---


# NetCore给World或Pdf添加水印

框架：net Core 3.1

包：

World：FreeSpire.Doc 7.11.0

PDF：Spire.PDF  8.1.0

![image-20220518162324852](/img/image-20220518162324852.png)

因为两个包的命名空间都是 Spire 无法通知引用所以建了两个项目



# 给PDF文件添加文字水印

```c#
		/// <summary>
        /// 给PDF文件添加文字水印
        /// </summary>
        /// <param name="pdfPath">目标文件路径</param>
        /// <param name="targetPath">保存路径</param>
        /// <param name="marks">文字水印内容</param>
        /// <returns></returns>
        public static void FontWaterMarkToFile()
        {
            Spire.Pdf.PdfDocument pdf1 = new PdfDocument();
            pdf1.LoadFromFile(@"D:\广联达BIM课件新版.pdf");

            for (int i = 0; i < pdf1.Pages.Count; i++)
            {
                PdfPageBase page = pdf1.Pages[0];
                page = pdf1.Pages[i];
                PdfTilingBrush brush = new PdfTilingBrush(new SizeF(page.Canvas.ClientSize.Width / 2, 											page.Canvas.ClientSize.Height / 3));
                brush.Graphics.SetTransparency(0.3f);
                brush.Graphics.Save();
                brush.Graphics.TranslateTransform(brush.Size.Width / 2, brush.Size.Height / 2);
                brush.Graphics.RotateTransform(-45);
                brush.Graphics.DrawString(i.ToString(), new Spire.Pdf.Graphics.PdfFont(PdfFontFamily.Helvetica, 24), 							PdfBrushes.Blue, 0, 0, new PdfStringFormat(PdfTextAlignment.Center));
                brush.Graphics.Restore();
                brush.Graphics.SetTransparency(1);
                page.Canvas.DrawRectangle(brush, new RectangleF(new PointF(0, 0), page.Canvas.ClientSize));
            }

            //保存文档
            pdf1.SaveToFile(@"D:\Output.pdf");
           
        }
```

# 给World文件添加文字水印

```c#
		/// <summary>
        /// 给World文件添加文字水印
        /// </summary>
        public static void FontWaterMark()
        {           
            Spire.Doc.Document doc1 = new Spire.Doc.Document();
            doc1.LoadFromFile(@"D:\面试题.docx");


            //设置文本水印
            TextWatermark txtWatermark = new TextWatermark();
            txtWatermark.Text = "Microsoft";
            txtWatermark.FontSize = 90;
            txtWatermark.Layout = WatermarkLayout.Diagonal;
            doc1.Watermark = txtWatermark;

            doc1.SaveToFile("水印.docx");

        }
```

