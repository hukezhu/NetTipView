# NetTipView


	由于项目中需要添加无网或者断网条件下的提示界面,所以就简单的写了一个提示的界面.记得之前在一份源码中见过类似的场景,但是忘了是哪一个了,也没有找到.
	
	因为项目中多数界面都使用到这个无网提示界面,包含控制器界面和自定义的view还有webView等,所以写了一个使用了UIView的分类,使用起来比较方便.    
	
	demo中使用了一个新闻接口,目前上拉加载获取不到数据,就只能这样了,使用了AFN,MJRefresh,MJExtension,代码较乱,请勿喷,谢谢
	
	使用了cocopods,所以打开项目时双击Demo.xcworkspace打开.
###分析
项目中使用了原生和webView相结合的方式,所以需要考虑两种情况.

* 原生界面
* UIWebView界面


对于断网无网提示,也要考虑两种情况

* 应用启动时就是无网状态
* 应用使用过程中断网


![断网无网提示分析.png](http://upload-images.jianshu.io/upload_images/693090-af21c47838f5dbee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.原生界面
 
 首先,应用启动时就是无网状态,我们可以在视图即将显示或者视图加载完毕时判断当前网络状态(项目多数都使用了AFN,可以通过AFN直接判断网络状态),如果是无网状态,就显示自定义无网界面;如果是有网状态,就隐藏自定义界面(这一步有无均可)
 
 其次,应用使用过程中断网,因为项目中多数使用了缓存,我们不能简单的直接显示断网提示界面,既然有缓存,用户仍然可以浏览界面,我们只需要提示用户即可.(这个可以在网络请求处进行判断)
 
 
2.UIWebView界面

首先,应用启动时就是无网状态,也是在页面将要显示时判断网络状态如果是无网状态,就显示自定义无网界面;如果是有网状态,就隐藏自定义界面(这一步有无均可).在UIWebView的代理方法中,当开始接在的时候就隐藏自定义界面,加载失败的话,就判断显示(这个地方需要判断,只有当无网时才显示,因为加载失败有好多的原因,不只是无网)

其次,应用使用过程中断网,刷新时判断网络状态,无网就取消刷新,弹出提示即可.


写了好多,觉着自己都被说晕了,文笔不好,思路也不清晰.


最终实现的效果:

![静态图片](https://box.worktile.com/view/c84ac928fb3d4a34a2c62989e8860b66?pid=39f1a5e31a40410cbd72fafeae831bf6&token=be630b8bbcd24ad19f67eee5d74c915b&dt=)




借鉴一下简述的界面:
![简书的无网界面](https://box.worktile.com/view/3089f1cd9d5744ed888cdf0e746fd8bd?pid=39f1a5e31a40410cbd72fafeae831bf6&token=be630b8bbcd24ad19f67eee5d74c915b&dt=)




##使用方法

直接将NetTipView.h 和 NetTipView.m 拖入到项目中,如果想要更新提示文字和按钮文字,直接在NetTipView.h文件中修改.

有一点需要注意:这个自定义view中使用了tag来标记view,需要检查自己的项目中是不是有跟这个view中相同的tag值,如果有相同的,一定要修改,不然会有问题,在源文件中也有标注.

```
/**
 *
 *  直接使用这个tag值可能会有问题.因为这里是根据tag值去查找当前view中是否包含本view,如果项目中之前使用了相同的tag值,则会产生错误,
 *  所以使用之前建议在项目中全局搜索这个tag值,如果没有,直接跳过;如果有相同的可以修改这个tag值(与本项目中的不相同即可)
 */
#define kNetTipViewTag        0xeef


#define descText  @"当前网络不可用,请检查您的网络"
#define buttontitle  @"点击刷新"





```










实现的效果:
![实现的效果](https://box.worktile.com/view/8e4fa01299c0479fa13ba836ae71397a?pid=39f1a5e31a40410cbd72fafeae831bf6&token=be630b8bbcd24ad19f67eee5d74c915b&dt=)

