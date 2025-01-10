# 合并mupdf和zjpdf

设置annot的appearance代码几乎完全重构了，所以在创建annot的时候，设置外貌这块代码需要重新整理逻辑：
当前逻辑：

1. 创建markup类型的annot。
2. 根据markup类型(underline,strick out,highlight)，创建颜色和线条属性
3. 使用颜色和线条设置annot外貌

新版本不提供根据颜色线条设置外貌的接口了，转而使用以下API设置外貌：

1. 创建annot时自动根据类型设置颜色
2. 使用font，size，color设置默认外貌（没有线条属性）
3. 使用整页更新API,一次性更新一页内所以annot的外貌
