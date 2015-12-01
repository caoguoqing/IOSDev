# Swift学习

搞定swift与Objc相互调用，并实现晓得demo

关于swift随时笔记

修改tabbaritem按钮使用图片的本身的颜色值：
自定义一个tabVC，在viewdid周期方法中指定tabbaritem的选中和未选中的状态图片，并指定图片的渲染方式imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal。
```objc
UIImage *tabbarImage = [UIImage imageNamed:imageName];
UIImage *tabbarImageSel = [UIImage imageNamed:imageSelName];
item.image   = [tabbarImage imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
item.selectedImage =  [tabbarImageSel imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
```
无意中的关联，只为记录过程，经实践，修改关联的控件属性，并不起作用
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

tabVC，当tabBarItem个数超出四个之后，会自动出现More按钮，点击后跳转到剩余的item列表中，在选择跳转到不同的VC中。