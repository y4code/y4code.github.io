+++
date = 2019-12-29T16:00:00Z
title = "7 Practical Tips for Cheating at Design 简陋翻译"
image =  "images/featured-post/post-1.jpg"
type = "featured"
+++

原文链接

[https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886](https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886 "https://medium.com/refactoring-ui/7-practical-tips-for-cheating-at-design-40c736799886")

**使用颜色和 Weight 来创造等级而不是大小**

* 主内容使用暗色（而不是黑色）（一篇文章的标题一样）
* 二级内容使用灰色（已发布文章的日期）
* 附加内容使用更轻（lighter）的灰色（maybe footer 的版权声明）

* 大多数文本 使用 400/500 weight
* 加重强调的文本 使用 600/700

* 不要使用400 以下 weight

**不要在有颜色的背景中使用灰色字**

* 让文本颜色接近于背景色，而不是 浅灰 就完事了
* 两条在colorful 背景中减少对比度的方法
  1. 减少白色字体的透明度
  2. based on 背景色选取一个颜色，色彩不变，调节它的饱和度明度（maybe 明度增加）

**抵消阴影**

* 单 使用向下的阴影 和 阴影扩散而不是 各个角度阴影，因为这像我们现实中看事物的方式

**更少得使用 border**

* 过多的 border 使内容杂乱，下次你想用的时候可以尝试
  1. 使用 box shadow来作为 border 区分内容
  2. 使用两种不同的（细微变化的）背景色 在毗连元素上
  3. 增加额外的空白

**不要放大你的原本意在小的 icon**

* 另外，尝试给无边小图标 加背景色和一个形状 以闭合它

**使用单边border 为清淡的设计增加颜色**

* 导航栏，为已 active 的的 item 增加一个高亮
* 或者是整个页面的顶部有个条
* Dribble Color Search 配色指南 [https://dribbble.com/colors/d4f49c](https://dribbble.com/colors/d4f49c "https://dribbble.com/colors/d4f49c")

**不是每个 button 都需要一个背景色**

* 主要的action应该是明显的 固定的？ 高差异的背景色在这里很好用
* 二级action应该是清晰的但不是重要的，轮廓样式或者 低差异化的背景色是个很好的选择
* 三级action 应该是可以发现的，但不能是招摇的，使用Link样式对 这些action works fine 

* 如果破坏性button不是主action，那应该是二级action的待遇