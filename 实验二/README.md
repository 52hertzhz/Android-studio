实验2_1_实现第一个Kotlin应用
设置第一个页面的fragment页面布局
正中间文本行为点击计数器
文本下方为3个按钮
第一个按钮命名为toast用于弹出提示框
第二个按钮命名为count用于点击增加计数
第三个按钮命名为random用于跳转到第二个随机数页面
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/screenBackground"
    tools:context=".FirstFragment">

    <TextView
        android:id="@+id/textview_first"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-condensed"
        android:text="@string/hello_first_fragment"
        android:textColor="@color/white"
        android:textSize="72sp"
        android:textStyle="bold"
        android:visibility="visible"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.3"
        />

    <Button
        android:id="@+id/random_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="24dp"
        android:background="@color/buttonBackground"
        android:text="@string/random_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textview_first" />

    <Button
        android:id="@+id/toast_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="24dp"
        android:background="@color/buttonBackground"
        android:text="@string/toast_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textview_first" />

    <Button
        android:id="@+id/count_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/count_button_text"
        app:layout_constraintStart_toStartOf="@id/toast_button"
        app:layout_constraintEnd_toEndOf="@id/random_button"
        app:layout_constraintTop_toBottomOf="@id/textview_first"
        app:layout_constraintBottom_toBottomOf="parent"
        android:background="@color/buttonBackground"
        />
</androidx.constraintlayout.widget.ConstraintLayout>
设置第二个fragment的页面布局
左上方文本行提示中间文本随机数范围
中间文本用于显示随机数
最下方按钮命名为previous用于回到第一个页面
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/screenBackground2"
    tools:context=".SecondFragment">

    <TextView
        android:id="@+id/textview_header"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="24dp"
        android:layout_marginLeft="24dp"
        android:layout_marginTop="24dp"
        android:layout_marginEnd="24dp"
        android:layout_marginRight="24dp"
        android:text="@string/random_heading"
        android:textColor="@color/colorPrimaryDark"
        android:textSize="24sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button_second"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/previous"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <TextView
        android:id="@+id/textview_random"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/r"
        android:textColor="@android:color/white"
        android:textSize="72sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toTopOf="@id/button_second"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textview_header"
        app:layout_constraintVertical_bias="0.45" />
</androidx.constraintlayout.widget.ConstraintLayout>


设置代码自动补全
点击File>Settings>查找Auto Import选项，在Java和Kotlin部分，勾选Add Unambiguous Imports on the fly



在onViewCreated方法中添加命令
TOAST按钮添加一个toast消息
使用绑定机制设置按钮randomButton的响应事件action_FirstFragment_to_SecondFragment
binding.randomButton.setOnClickListener {
    findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)
}

为TOAST按钮添加事件myToast.show()，使用**findViewById()**查找按钮id，id为R.id.toast_button
// find the toast_button by its ID and set a click listener
view.findViewById<Button>(R.id.toast_button).setOnClickListener {
   // create a Toast with some text, to appear for a short time
   val myToast = Toast.makeText(context, "Hello Toast!", Toast.LENGTH_LONG)
   // show the Toast
   myToast.show()
}
运行效果：

使Count按钮更新屏幕的数字
view.findViewById<Button>(R.id.count_button).setOnClickListener {
   countMe(view)
}

自定义countMe方法
private fun countMe(view: View) {
   // Get the text view
   val showCountTextView = view.findViewById<TextView>(R.id.textview_first)

   // Get the value of the text view.
   val countString = showCountTextView.text.toString()

   // Convert value to a number and increment it
   var count = countString.toInt()
   count++

   // Display the new value in the text view.
   showCountTextView.text = count.toString()
}

运行效果：


启用SafeArgs
Gradle（Project），在plugins节添加

id 'androidx.navigation.safeargs.kotlin' version '2.5.0-alpha01' apply false

Gradle（module）在plugins节添加

id 'androidx.navigation.safeargs'

修改java版本java8->java11



navigation文件夹->nav_graph.xml->点击design
打开导航视图，点击FirstFragment，查看其属性。
在Actions栏中可以看到导航至SecondFragment
同理，查看SecondFragment的属性栏
点击Arguments **+**符号
弹出的对话框中，添加参数myArg，类型为整型Integer


FirstFragment添加代码，向SecondFragment发数据
删掉onViewCreated()方法中原先的事件处理代码，
获取TextView中文本并转化为整数值
val showCountTextView = view.findViewById<TextView>(R.id.textview_first)
val currentCount = showCountTextView.text.toString().toInt()

将数值currentCount 传参给传递函数
val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(currentCount)
添加导航
findNavController().navigate(action)

完整添加修改后的onViewCreated函数
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

    // find the toast_button by its ID and set a click listener
        view.findViewById<Button>(R.id.toast_button).setOnClickListener {
            // create a Toast with some text, to appear for a short time
            val myToast = Toast.makeText(context, "Hello Toast!", Toast.LENGTH_LONG)
            // show the Toast
            myToast.show()
        }
        view.findViewById<Button>(R.id.count_button).setOnClickListener {
            countMe(view)
        }

        binding.randomButton.setOnClickListener {
//            findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)
            val showCountTextView = view.findViewById<TextView>(R.id.textview_first)
            val currentCount = showCountTextView.text.toString().toInt()
            val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(currentCount)
            findNavController().navigate(action)
        }

    }
添加SecondFragment的代码
导入navArgs包
import androidx.navigation.fragment.navArgs
onViewCreated()函数外之前添加代码
val args: SecondFragmentArgs by navArgs()
获取FirstFragment传递过来的参数列表，获取数值赋值给count，并在textview_header中显示
val count = args.myArg
val countText = getString(R.string.random_heading, count)
view.findViewById<TextView>(R.id.textview_header).text = countText
在0~count范围随机生成一个整数，并在textview_random中显示
val random = java.util.Random()
var randomNumber = 0
if (count > 0) {
   randomNumber = random.nextInt(count + 1)
}
完整的onViewCreated函数
val args: SecondFragmentArgs by navArgs()

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    val count = args.myArg

    val countText = getString(R.string.random_heading, count)
    view.findViewById<TextView>(R.id.textview_header).text = countText
    val random = java.util.Random()
    var randomNumber = 0
    if (count > 0) {
        randomNumber = random.nextInt(count + 1)
    }
    view.findViewById<TextView>(R.id.textview_random).text = randomNumber.toString()

    binding.buttonSecond.setOnClickListener {
        findNavController().navigate(R.id.action_SecondFragment_to_FirstFragment)
    }
}
运行效果：
