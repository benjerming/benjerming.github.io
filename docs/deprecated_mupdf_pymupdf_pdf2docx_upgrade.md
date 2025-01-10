# mupdf_pymupdf_pdf2docx升级

## mupdf升级

1. 检出版本
2. 新建分支
3. 使用`make python`生成python/c++相关源码，和build/**/mupdf.py文件，并删除其它文件
4. 修改mupdf.py中导出的模块名_mupdf->zjpdf
5. 合并cmake文件
6. 使用andrdoi studio编译
7. 提交修改

## pymupdf升级

1. 检出版本
2. 新建分支
3. 合并build.py和setup.py文件
4. 修改导入模块_mupdf和_extra->zjpdf
5. 使用python3.8打包
6. 提交修改

## pdf2docx升级
