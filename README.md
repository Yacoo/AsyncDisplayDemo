# AsyncDisplayDemo
##开始
`AsyncDisplayKit`的基本单元是`node`。`ASDisplayNode`是`UIView`的一种抽象,进而也是`CALayer`的一种抽象。它不像视图那种只能在主线程使用，`nodes`是线程安全的,你可以在后台线程实例化以及配置他们整个的层级关系。

为了保证界面的平滑与响应灵敏，你的应用应该每秒钟渲染60帧,这是ios中的黄金准则。这意味着主线程每60分之一秒处理一帧.需要16毫秒去执行所有的布局与渲染代码！由于系统开销，你的代码通常只有少于十毫秒的时间来执行，否则就会掉帧。

`AsyncDispplayKit`让你把图片解码，文本大小以及渲染、其他一些代价高昂的UI操作从中线程中移除。 It has other tricks up its sleeve too... but we'll get to that later. 

##`nodes`
如果你已经熟悉了和`view`打交道，你就已经知道怎么用`nodes`了。`view`的大多数方法都有一个对应的`nodes`的方法，并且大多数`UIView`和`CALayer`的属性也都是可用的。如果存在一些命名差异的话（例如clipsToBounds和masksToBounds），`nodes`默认使用`UIView`的名称。唯一的例外是`nodes`使用`position`代替`center`。

另外，以可以直接通过`node.view`或者`node.layer`来获取`node`包装下的视图。

`AsyncDisplayKit`包含以下几个核心的`nodes`:
* `ASDisplayNode` `UIView`的对应实现-子类可以实现自定义`nodes`
* `ASCellNode`，`UICollectionViewCell`或者`UITableViewCell`的对应实现，子类可以实现自定义`cell`,也可以通过`view controller`实例化。
* `ASControllerNode`,类似`UIControl`，子类化以创建`button`。
* `ASImageNode`，就像`UIImageview`-异步的解码图片。
* `ASTextNode`，就像`UITextView`-基于`TextKit`创建并且可以支持所有的富文本特性。

##`nodes`容器

当你想要在应用中使用`AsyncDisplayKit`时，一个常见的错误就是直接在现有的视图层级中添加`nodes`。这么做实际上会让你的`nodes`再被渲染的时候一闪而过（Doing this will virtually guarantee that your nodes will flash as they are rendered）。

正确的做法是，你应该在某个容器类中添加子类`nodes`。这些类负责告诉他们所含的`nodes`他们目前所处的状态，这样就能够足够搞笑地加载数据并渲染`nodes`。你可以把这些类认为是`UIKit`和`ASDK`的整合。

四个`node`容器是：
* `ASViewController`。一个`UIViewController`的子类，你可以提供`nodes`给它管理。
* `ASCollectionNode`，类似于`UICollectionView`-管理一组`ASCellNodes`。
* `ASTableNode`，类似于`UITableView`-同样管理`ASCellNode`但是使用了修正的宽度（also uses ASCellNode but has with a fixed width.）
* `ASPagerNode`一种特殊的`UICollectionNode`，用法和`UIPageViewController`一致。

###布局系统
`AsyncDisplayKit`的布局系统是它最强大也是最特别的特性。它基于`CSS FlexBox`模型，它提供了一种可以指定一个自定义`node`的size以及子类`nodes`的布局的声明方式。由于所有的`node`默认是同步渲染的，通过为每个`node`提供一个`ASLayoutSpec`可以让计算与布局异步执行。

几个布局相关
* `ASLayoutSpec`，为它相关的`node`提供大小以及位置。
* `ASStackLayoutSpec`让你用一种和`UIStackView`类似的方式布局`nodes`。
* `ASBackgroundLayoutSpec`，为node设置背景
* `ASStaticLayoutSpec`，当你想要手动在一块固定区域设置一组`nodes`时会非常有用。






