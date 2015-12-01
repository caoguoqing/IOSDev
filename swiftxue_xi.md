# Swift学习

搞定swift与Objc相互调用，并实现晓得demo

关于swift随时笔记

修改tabbaritem按钮使用图片的本身的颜色值：
自定义一个tabVC，在viewdid周期方法中指定tabbaritem的选中和未选中的状态图片（渲染方式）。
```objc
/**
 *  @author shuguang, 15-12-01 14:12:42
 *
 *  先使用ViewController设置为tabVC的根视图,关联UItabBarItem，然后，editor -> embed in -> navigation Controller即可。
 *
 *  @since 1.0
 */
@property (weak, nonatomic) IBOutlet UITabBarItem *ibTabbar;
```