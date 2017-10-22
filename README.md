**不想看废话的同学可以直接到分割线下面看关于JSON.stringify()的提醒**

今天在处理数据的时候发生了一个灵异事件。

话不多说，上代码：

![](http://upload-images.jianshu.io/upload_images/7538454-27fcbc229fe8db32.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图所示，把存在localStorage里的数组取出来，再往里面push新的对象。其中选择用Map储存grades 的值，然后将数据往Map里面加。最后
localStorage.studentInfo = JSON.stringify(studentInfo);  把localStorage.studentInfo中存放的对象数组更新。
当成功添加了一条学生信息后，在localStorage中的数据竟然是这样的：

![](http://upload-images.jianshu.io/upload_images/7538454-2f800166b6720d91.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如大家所见，grades中的Map和其中的数据变成了一对{}
原本想着是不是代码有问题，然后在debug中watch grades值的变化：
![](http://upload-images.jianshu.io/upload_images/7538454-b1833691e931205b.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行完代码后，成绩是被存进了Map中的。
然后watch  JSON.stirngify(studentInfo)

![](http://upload-images.jianshu.io/upload_images/7538454-38f890652c47474e.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

神奇的事情发生了，grades里面只剩下一对{}。所以问题应该是出在JSON.stringify的过程中了，然后去翻官方文档：（参见[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)）

-------------------------------------------我是分割线--------------------------------------------------------------

功能大家都知道，将各种数据类型转换成字符串。注意事项如下：
![](http://upload-images.jianshu.io/upload_images/7538454-9a03bb413f58ad94.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以stringify并不能将所有的数据类型在不丢失信息的情况下转换成字符串，上面的Map就在转换的过程中变成了一对{}，解决方法就是用stringify可处理的数据结构替换Map，然后我把grades用对象存就好了：
![](http://upload-images.jianshu.io/upload_images/7538454-6b9c678f77c7705b.png!thumbnail?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

（PS：以上debug过程由陈老板完成，本人记录。原本觉得是灵异事件的我通过观察老板debug收获还是挺多的，以后能够抱着不抛弃不放弃的心态去debug解决各种灵异事件也是极好的。（心态已崩...））
