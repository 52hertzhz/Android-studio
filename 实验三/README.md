#Jupyter Notebook基础教程

创建notebook文件并重命名与试用

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/1.png)

验证kernel中运行的cell代码具有延续性（先运行的代码可以被后运行的使用）

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/2.png)

##简单的Python程序的例子

导入相关库，读入csv文件，并显示开头5条记录

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/3.png)

查看结尾5条记录，重新设置属性名，查看是否完成数据条目加载

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/4.png)

查看属性列的数据类型，发现属性列profit的部分数据类型不符合期望float，用正则表达式进行检查

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/5.png)

将profit的属性列包含非数字的记录数量，进行可视化操作

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/6.png)

单独年份属性列profit包含非数值元素占总体比例较少，可以采取删除的方式进行清洗

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/7.png)

##使用matplotlib进行绘图
将属性列按照年份year进行分组，绘制平均利润和收入

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/8.png)

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/9.png)

观察到公司利润曲线急剧下滑，收入曲线没有急剧下滑，对数据结果进行标准差处理
发现不同公司之间的收入和利润差距惊人

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/10.png)

##导出Notebooks
使用"File > Download as"导出markdown格式的数据，除此之外还有pdf、html等格式，在线
协同方式需借助Github或者Google Colab平台

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/11.png)

##Jupyter Notebook扩展工具

拓展代码补全功能

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/12.png)

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%B8%89/image/13.png)

