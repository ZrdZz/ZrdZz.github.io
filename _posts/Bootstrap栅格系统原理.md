---
layout:     post
title:      栅格系统
date:       2017-11-2
author:     zrd
catalog:    true
tags:
    - CSS
---

## 栅格系统的组成元素

![](http://j4n.co/content/4-blog/10-Creating-your-own-css-grid-system/grid-elements.png)

row必须放在container中，column放在row中，内容放在column中。

### container的作用

1. 在不同的宽度区间内（响应式断点）提供宽度限制。当宽度变化时，column采用不同的宽度。
2. 提供一个padding，阻止内部内容触碰到浏览器边界。

![](http://images0.cnblogs.com/blog2015/412020/201507/052156204047619.png)

row是column的容器，每个row中的column之和必须为一个固定数值，也可以通过嵌套的方式扩展。row的左右margin都为-15px(为了方便嵌套)，用来抵消container中的padding。

![](http://images0.cnblogs.com/blog2015/412020/201507/052202216395692.png)

每个column左右padding都为15px，row的负margin抵消了container的padding，所以为每个column设置padding就是为了防止内容直接触碰边界，同时不同的column之间拥有30px的卡槽（Gutter）。

![](http://images0.cnblogs.com/blog2015/412020/201507/052206281235724.png)

row的负margin设计主要为了嵌套，如果要在column中嵌套column首先要把被嵌套的column放到row中，把row放到作为容器的column中，而不需要在放置一个container。

![](http://images0.cnblogs.com/blog2015/412020/201507/052232328106603.png)

现在将被嵌套的column放入row中，上层column便是起到了container的作用。

![](http://images0.cnblogs.com/blog2015/412020/201507/052234011394651.png)
