OCR功能研究学习

# tesseract-ocr

## 安装

*   tesseract：引擎
    
*   tessdata：训练数据，二选一
    
    *   tessdata-fast：平衡了速度与准确性的版本【推荐】
        
        *   osd：用于定向和分段，11MB
            
        *   chi\_sim：用于提取简体中文的训练数据，2.4MB
            
        *   eng：用于提取英文的训练数据（默认语言），4.0MB
            
        *   其他
            
    *   tessdata-best：牺牲速度换取准确率提升
        

## 使用

*   分辨率：推荐300dpi，测试表明，mupdf默认72dpi提取文字错误率较高
    
*   语言：必须设置将要提取的语言，语言可以组合，但耗时可能会线性增加
    
*   训练集：tessdata目录
    

# mupdf集成tesseract-ocr

## fz\_ocr\_device

提取页面中的文字，`fz_ocr_device`分两种工作模式，目前默认使用修正文字的工作模式

*   不使用displaylist（ 一般PDF文件都能良好工作，但无法修复unicode值）：
    
    *   将页面中的非文本事件转发给用户
        
    *   将页面中的所有事件绘制到灰度图片中
        
    *   当页面关闭时
        
        *   使用ocr引擎[提取](https://github.com/ArtifexSoftware/mupdf/blob/1.25.2/source/fitz/tessocr.cpp#L231)pixmap中的文字
            
        *   设置固定字体`Courier`（仅支持英文）
            
        *   将识别出的文字通过文本绘制事件转发给用户
            
*   使用displaylist
    
    *   将页面中的所有事件保存到distplaylist
        
    *   将页面中的所有事件绘制到灰度图片中
        
    *   当页面关闭时
        
        *   使用ocr引擎[提取](https://github.com/ArtifexSoftware/mupdf/blob/1.25.2/source/fitz/tessocr.cpp#L231)pixmap中的文字
            
        *   使用重写设备重新播放所有事件并转发给用户
            
            *   播放文字事件时，匹配ocr已提取的文字，修正文字unicode值
                
            *   重写设备使用正确的 Unicode 值重写文本对象
                
        *   OCR 识别出的任何字符，如果重写步骤未使用，则将作为不可见文本发送。（因此能够正确完成搜索/复制/剪切操作）
            

## mudraw

当输出文件格式包含ocr时，底层使用`fz_ocr_device`，固定使用displaylist方式。

代码见[mudraw跟踪使用ocr提取文字过程](https://github.com/ArtifexSoftware/mupdf/blob/1.25.2/source/tools/mudraw.c#L716)和 [mudraw使用ocr提取文字](https://github.com/ArtifexSoftware/mupdf/blob/1.25.2/source/tools/mudraw.c#L852)。

## fz\_write\_pixmap\_as\_pdfocr

使用`pdfocr_band_writer`，将包含文字的图片生成pdf文档，且二值化后生成pixmap，

*   创建`pdfocr_band_writer`环境
    
    *   `pdfocr_write_header`
        
        *   过程填充pdf文件头部信息
            
        *   创建仅alpha通道（二值化）的pixmap
            
    *   `pdfocr_write_band`
        
        *   填充pixmap
            
    *   `pdfocr_write_trailer`
        
        *   使用ocr引擎[提取](https://github.com/ArtifexSoftware/mupdf/blob/1.25.2/source/fitz/tessocr.cpp#L231)pixmap中的文字
            
        *   `flush_words`将文字写入buffer
            
        *   将buffer以stream形式生成pdf文件的obj对象
            

# pymupdf使用mupdf的tesseract-ocr功能

实质上是调用了`fz_write_pixmap_as_pdfocr`把图片生成pdf