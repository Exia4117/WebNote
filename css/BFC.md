### BFC
[什么是BFC](https://www.cnblogs.com/libin-1/p/7098468.html)

BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

BFC作用：
1. 利用BFC避免外边距折叠
2. 清除内部浮动 （撑开高度）
   - 原理: 触发父div的BFC属性，使下面的子div都处在父div的同一个BFC区域之内
3. 避免文字环绕
4. 分属于不同的BFC时，可以阻止margin重叠
5. 多列布局中使用BFC

如何生成BFC：（脱离文档流，满足下列的任意一个或多个条件即可）
1. 根元素，即HTML元素（最大的一个BFC）
2. float的值不为none
3. position的值为absolute或fixed
4. overflow的值不为visible（默认值。内容不会被修剪，会呈现在元素框之外）
5. display的值为inline-block、table-cell、table-caption

BFC布局规则：
1. 内部的Box会在垂直方向，一个接一个地放置。
2. 属于同一个BFC的两个相邻的Box的margin会发生重叠
3. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此, 文字环绕效果，设置float
4. BFC的区域不会与float box重叠。
5. 计算BFC的高度，浮动元素也参与计算