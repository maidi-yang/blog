---
title: html2canvas/dom-to-image + jspdf实现网页截屏
date: 2019-08-07 14:33:49
tags: [js，pdf生成]
categories: [技术,前端]
---
1.html2canvas网页截屏
 ````
 html2canvas($('#page')[0], {
                onrendered: function (canvas) {
                    document.body.appendChild(canvas);

                    var contentWidth = canvas.width;
                    var contentHeight = canvas.height;

                    var size = {
                        width: 595.28,
                        height: 841.89
                    }
                    //一页pdf显示html页面生成的canvas高度;
                    var pageHeight = contentWidth / size.width * size.height;
                    //未生成pdf的html页面高度
                    var leftHeight = contentHeight;
                    //页面偏移
                    var position = 0;
                    //a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
                    var imgWidth = size.width;
                    var imgHeight = size.width/contentWidth * contentHeight;

                    var pageData = canvas.toDataURL('image/jpeg', 1.0);

                    var pdf = new jsPDF('', 'pt', 'a4');

                    //有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
                    //当内容未超过pdf一页显示的范围，无需分页
                    if (leftHeight < pageHeight) {
                        pdf.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight );
                    } else {
                        while(leftHeight > 0) {
                            pdf.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight)
                            leftHeight -= pageHeight;
                            position -= 841.89;
                            //避免添加空白页
                            if(leftHeight > 0) {
                                pdf.addPage();
                            }
                        }
                    }

                    pdf.save("content.pdf");
                }
            });
````
放大失真；处理复杂svg时与实际页面不同

这种方法在火狐有下划线<U>标签上浮bug


2. domtoimage代码生图片
````
domtoimage.toPng($('#canvas')[0])
                .then(function (dataUrl) {
                    var pdf = new jsPDF();

                    pdf.addImage(dataUrl, 'PNG', 0, 0);
                    pdf.save("content.pdf");
                });
````
放大失真；对于复杂svg很友好，基本不出现与实际页面不同的情况



如果要保证精确度，可以使用domtoimage.toSvg（svg的defs重用不支持）
