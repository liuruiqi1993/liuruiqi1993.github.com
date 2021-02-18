# MVC与MVVM的区别

https://www.php.cn/faq/417707.html

MVC
![https://img.php.cn/upload/article/000/000/028/5cc15a3793af8801.jpg]
MVC是包括view视图层、controller控制层、model数据层。各部分之间的通信都是单向的。

View 传送指令到 ControllerController 完成业务逻辑后，要求 Model 改变状态Model 将新的数据发送到 View，用户得到反馈



MVVM
![https://img.php.cn/upload/article/000/000/028/5cc159f5ea5e9940.jpg]
MVVM包括view视图层、model数据层、viewmodel层。各部分通信都是双向的。采用双向数据绑定，View的变动，自动反映在 ViewModel，反之亦然。其中ViewModel层，就是View和Model层的粘合剂，他是一个放置用户输入验证逻辑，视图显示逻辑，发起网络请求和其他各种各样的代码的极好的地方。说白了，就是把原来ViewController层的业务逻辑和页面逻辑等剥离出来放到ViewModel层



MVC与MVVM的区别

在MVC里，View是可以直接访问Model的，所以View里会包含Model信息以及一些业务逻辑。 MVC模型关注的是Model的不变，所以在MVC模型里，Model不依赖于View，但是 View是依赖于Model的。不仅如此，因为有一些业务逻辑在View里实现了，导致要更改View也是比较困难的，至少那些业务逻辑是无法重用的。

MVVM在概念上是真正将页面与数据逻辑分离的模式，它把数据绑定工作放到一个JS里去实现，而这个JS文件的主要功能是完成数据的绑定，即把model绑定到UI的元素上。此外MVVM另一个重要特性双向绑定，它更方便你去同时维护页面上都依赖于某个字段的N个区域，而不用手动更新它们。
