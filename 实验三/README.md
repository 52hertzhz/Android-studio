Jupyter Notebook基础教程

创建notebook文件并重命名与试用

验证kernel中运行的cell代码具有延续性（先运行的代码可以被后运行的使用）

简单的Python程序的例子

导入相关库，读入csv文件，并显示开头5条记录

查看结尾5条记录，重新设置属性名，查看是否完成数据条目加载

查看属性列的数据类型，发现属性列profit的部分数据类型不符合期望float，用正则表达式进行检查

将profit的属性列包含非数字的记录数量，进行可视化操作

单独年份属性列profit包含非数值元素占总体比例较少，可以采取删除的方式进行清洗

使用matplotlib进行绘图
将属性列按照年份year进行分组，绘制平均利润和收入


观察到公司利润曲线急剧下滑，收入曲线没有急剧下滑，对数据结果进行标准差处理
发现不同公司之间的收入和利润差距惊人

导出Notebooks
使用"File > Download as"导出markdown格式的数据，除此之外还有pdf、html等格式，在线
协同方式需借助Github或者Google Colab平台

Jupyter Notebook扩展工具

拓展代码补全功能


