# 手机卫士 #

## 一.Animation界面 ##

### 1.获取版本号,版本名 ###

### 2.动画效果的实现 -- 四种补间动画的使用###

### 3.动画的监听 -- 通过sp中保存的值来判断用户是否开启版本更新功能 ###

### 4.开启则通过网络请求--获取数据--比对版本 ###

### 5.有新版本则进行升级--断点续传下载apk(使用XUtils工具) ###

### 注意:按逻辑适时进入主界面,有可能网络请求执行的时间小于动画执行时间,需要保证在动画执行完后进入主界面 ##


## 二:Home界面 ##

### 1.一个简单的布局:textView的跑马灯效果,属性动画的使用,GridView的展示及其item的点击事件 ###


## 三:Home界面的右上角的Setting界面 ##

### 1.自定义控件(MyRelativeLayout布局)--布局没有text属性-->需要自定义text的属性

### 2.仿系统来自定义属性(myandroid:myText),以及mybackground属性(可以不用定义,属于布局自带属性),在values-->attrs文件夹中自定义

### 3.图片状态的回显(一般在onCreate方法中完成即可,服务相关的则需要根据系统是否开启了该服务来判定并回显) ###

### 功能1:开启自动更新:开启后会在Animation界面完成版本的比对,若版本更新,则需要通过断点续传的方式下载新的apk

### 功能2:黑名单拦截:开启后会根据系统是否开启了"SmsService"来开启/关闭该服务,开启该服务后会在服务中动态注册一个收到短信的广播和电话状态的监听器,会分别比对来电号码是否是黑名单(比对拦截模式),是黑名单则分别执行短信或电话拦截

### 短信拦截:通过获得的短信来获取号码-->判断为黑名单时-->判断短信内容某些关键字/判断拦截模式为短信拦截时-->终止短信接收的广播(即拦截了)

### 电话拦截:通过电话状态的监听器,在响铃时判断-->为黑名单&&为电话拦截模式-->挂断电话并删除通话记录-->

### -->挂断电话:需要通过反射拿到getService(),并且需要通过AIDL实现和系统间的进程通信拿到ITelephony-->最后调用endCall()

### -->删除通话记录:需要通过内容观察者来观察通话记录的数据库的变化,当发生变化时删除通话记录(这样能准确的去完成任务)

### 功能3:号码归属地显示:在第一次进入Animation界面时完成了号码归属地的数据库的下载,开启服务(ShowLocationService),通过查询数据库,显示号码归属地-->

### 服务中显示自定义吐司:涉及到窗体管理器(区分在Activity和在Service中显示View的区别),算法实现Toast在界面内可以自由移动

### 号码归属地的显示风格:自定义控件(类:SetRelativeBg,布局:R.layout.view_rl_bg),给布局自定义属性(gaga:mybig_title...),Dialog的单选模式setSingleChoiceItems()和回显



## 四:Home界面的position(0)--手机防盗

### 1.自定义对话框的展示:设置密码和密码验证(对话框的使用) ###

### 2.成功则进入GuardSetupActivity1界面:功能罗列的界面展示 ###

### 3.GuardSetupActivity2界面:获取sim卡串号,进行sim卡的绑定与解绑,图片的回显 ###

### 4.GuardSetupActivity3界面:选则安全号码(手动输入,或者从联系人界面获取startActivityAndForResult()),数据是通过内容解析者获取系统提供的联系人信息(ContactUtils),获取联系人的正常格式...edittext的数据回显时光标靠右;

### 5.GuardSetupActivity4界面:开启防盗保护(CheckBox和textView的状态的回显),激活和取消高级管理员权限. ###

### 6.FunctionActivity1:设置完成界面:回显和功能列表(GPS追踪,播放报警音乐,远程删除数据,远程锁屏)-->通过广播接收者来完成

### 注意:设置界面的基类抽取(baseSetupActivity界面): 基本抽取和手势的使用


## 五:完成设置界面后(开启了防盗保护)-->广播接收者##

### 1.静态注册了手机重启广播(BootCompletedReceiver),重启后检查保护状态是否开启保护,比对sim卡串号,若变更--给安全号码发送短信  ##

### 2.静态注册了短信接收的广播,通过管理员权限获取到相应的短信内容,执行相应的功能 ###


## 六:完成设置界面后(开启了防盗保护)-->绑定服务 ##

### 1.绑定了获取位置信息的服务(LocationService):对手机的位置进行定位,接收到*#location#*短信后,就会激活该服务(具体实现见LocationService,涉及到了坐标的获取和转换) ###


## 七:Home界面的position(1)--通讯卫士(黑名单列表)

### 1.操作数据库:对数据库进行了基本的增删改查的操作,以及分段查询,查询联系人数目

### 注意:当查询数据过多时(例如:2000,游标会报空(未解决...)),用分页查询来可以避免(每次查询一定数目),当滑动到底部时再继续查询(刷新数据)


## 八:Home界面的position(2)--软件管家

### 1.通过Environment类获取手机内存和SD卡内存

### 2.通过包管理器获取所有的应用程序的名称(设备的静态信息--已有的应用),图标,并判断为用户/系统软件

### 3.在代码中动态的给listView添加控件,完成listView的吸顶效果

### 4.listView的展示:当listView的item存在不同类型的View时,需要判断contentView的值和属性(是哪种View,可不可以被复用)

### 5.popupWindow的使用:可设置具体显示位置,设置弹出动画,注意:popupWindow需要手动关闭(在退出界面时)

### 6.启动:通过包名和隐式意图即可启动

### 7.分享:启动隐式意图,可携带内容,关注该广播的应用都可以被启动

### 8.详情:启动隐式意图,通过包名可以跳转到每个应用的详情

### 9.卸载:通过开启隐式意图到卸载界面,通过广播监听,当应用真正被卸载时,将数据从集合中移除,并刷新


## 九:Home界面的position(3)--进程管理

### 1.通过ActivityManager获取正在运行进程个数(设备的动态信息),可用的剩余内存,总内存

### 2.通过PackageManager和ActivityManager获取所有的进程信息,图标,区分用户进程/系统进程

### 注意:有些进程会信息不全,这时获取packageInfo就会有异常,所以需要在catch中设置一些默认值

### 3.listView的展示和吸顶效果

### 4.全选:遍历用户和系统集合,设置CheckBox为true(注意:自己不能被选中)

### 5.反选:遍历,设置CheckBox的状态取反(跳出自己那次循环)

### 6.清理:遍历,通过killBackgroundProcesses()杀死进程(添加权限),并移出集合(注意在遍历的时候不能改变集合大小,所有先添加,最后移除)

### 7.设置(1):是否显示系统进程:在Adapter中的getCount()中设置,记住在onStart()或onResume()刷新Adapter

### 8.设置(2):是否显开启锁屏清理:创建一个锁屏清理的服务,在服务中动态注册接收锁屏信号的广播,当锁屏时清理所有进程

### 注意:一般状态的回显是根据判断Sp中的保存值来判断(在onCreate()中完成),服务状态的回显是根据此服务是否在系统中运行(在onStart()/onResume()中完成)


## 十:


## LAST:高级工具:

### 1.程序锁:

### 思路一:用Activity+listView来完成:在Activity中添加两个listView(加锁和未加锁),在点击按钮的事件中分别设置可见和不可见,最后区分加锁(系统/用户)应用和未加锁(系统/用户)应用

### 思路二:用Activity+Fragment+RecyclerView来完成,在Activity中添加两个Fragment,在两个Fragment钟分别添加一个RecyclerView,显示item...

### 思路二的所有内容放在app_lock_unlock包下,由于没有实现recyclerView中的item中控件类型不统一时怎么复用,所以思路二boom boom...

### 2.归属地查询:输入框的抖动动画,归属地数据库的查询和比对

### 3.常用号码查询:在Animation界面加载了常用号码数据库,查询数据并用可折叠列表(ExpandableListView)实现了常用号码的展示-->

### -->多表关联的查询,优化数据库的关闭(在Activity中获取和关闭)

### -->BaseExpandableListAdapter:区分好外层和内层

### 4.短信的备份:ProgressDialog的横向展示,通过接口回调实现进度显示,对短信进行xml格式的存储-->

## -->重点:接口的回调:分层管理,实现低耦合

### "类A"需要通过"类B"中的工具方法来获取数据-->创建一个接口C-->"类B"的工具方法中把"接口"C作为参数(形参)

### 在"类B"的内部通过形参"接口C"来调用"接口C"中的方法-->给其设置值(数据)

### "类A"在调用类B的工具方法获取数据时就必须要传入参数"接口C"(实参),所以就需要实现"接口C"中的方法-->从而也就获取到了值(数据)

## 类A--V层(通过实现接口拿到数据-->设置给任意的View)
## 类B--P层(通过给接口设置值,把数据传递出去,不去强行规范V层传递过来的参数,也不管V层把参数设置给了哪种View)


## 项目中涉及到的权限在AndroidManifest.xml文件中有说明