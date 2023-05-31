实验4_实现图像分类APP

下载TEL Classify 原代码 编译gradle后运行程序
首次运行后请求拍照录像权限

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/1.png)

在gradle.start中添加 TensorFlow Lite Model
![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/2.png)

检查代码中的TODO项
![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/3.png)

添加依赖以及逻辑处理代码
导入依赖

    private val flowerModel = FlowerModel.newInstance(ctx)

在TODO1下添加

    val tfImage = TensorImage.fromBitmap(toBitmap(imageProxy))

在TODO3下添加

    val outputs = flowerModel.process(tfImage)
        .probabilityAsCategoryList.apply {
        sortByDescending { it.score } // Sort with highest confidence first
        }.take(MAX_RESULT_DISPLAY) // take the top results

在TODO4下添加

    for (output in outputs) {
        items.add(Recognition(output.label, output.score))
    }

注释掉原模拟概率代码

    ///for (i in 0 until MAX_RESULT_DISPLAY){
    ///    items.add(Recognition("Fake label $i", Random.nextFloat()))
    ///}

启动编译，对模型性能进行测试
玫瑰

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/4.png)

郁金香

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/5.png)

向日葵

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/6.png)

蒲公英

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/7.png)

雏菊

![](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E5%9B%9B/image/8.png)