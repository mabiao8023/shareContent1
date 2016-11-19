# css布局中的FC

---

### 问题？？

1、子元素浮动导致父元素高度坍陷

2、如何实现两栏自适应布局

以上问题各位是如何解决？？为什么呢？

### 一、概念
#### 1、BOX
  CSS 布局的对象和基本单位。**元素的类型和display属性**，决定了这个 Box 的类型。不同类型的Box内的元素会以不同的方式渲染。

- block-level box:display 属性为 block, list-item, table 的元素。
- inline-level box:display 属性为 inline, inline-block, inline-table 的元素。

#### 2、什么是FC(W3C CSS2.1 规范中的一个概念)

Formatting context ，页面中的一块渲染区域，并且有一套**渲染规则**，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

**CSS2.1 中只有 BFC 和 IFC, CSS3 中还增加了 GFC 和 FFC。**

#### 3、什么是BFC？
块级格式化上下文(Block formatting context),只有普通流中的Block-level box参与的一块渲染区域，它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

**注：而float、position值不为relative\static的元素是脱离普通文档流的，不参与BFC的布局。**
##### 3.1 六大规定
- 内部的Box会在垂直方向，一个接一个地放置。

---

- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

> 外边距重合的问题？

---

- 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

> 元素浮动，其他元素被重叠？

---

- BFC的区域不会与float box重叠。

> 清除浮动
> 两栏自适应布局，左侧浮动，右侧生成bfc

---
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

---
- 计算BFC的高度时，浮动元素也参与计算。
> 清除浮动
> 子元素浮动父元素高度坍陷？

---
##### 3.1 这么厉害？怎么样才能成为BFC呢

- 根元素
- float不为none
- position不为relative和static
- display:inline-block, table-cell, table-caption, flex, inline-flex
- overflow不为visible


##### 3.2举个栗子解读BFC

```
<style>
    body {
        width: 300px;
        position: relative;
    }
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
    .main {
        height: 200px;
        background: #fcc;
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```
![image](https://mabiao8023.github.io/web-flex/float1.png)

**根据BFC布局第三条**
> 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

**怎么实现左边固定右边自适应?**

**根据BFC布局规则第四条**
> BFC的区域不会与float box重叠。
让ian生成BFC？

```
.main {
    overflow: hidden;
}
```

![image](https://mabiao8023.github.io/web-flex/float2.png)

---

**自适应两栏布局有很多的其他方法**

#### 4、IFC
**内联格式化上下文**,IFC线框高度由其包含行内元素中最高的实际高度计算而来的

- 1.IFC中的line box一般左右都贴紧整个IFC，但是会因为float元素而扰乱。float元素会位于IFC与与line box之间，使得line box宽度缩短。 
- 2.水平居中：当一个块要在水平居中时，设置为inline-block则会在外层产生IFC，通过text-align可以水平居中。
- 3.垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他行内元素则可以在此父元素下垂直居中。

#### 4、FFC
(Flex Formatting Contexts)为"自适应格式化上下文"，
- display值为flex或者inline-flex的元素将生成ffc

- display: flex 或 inline-flex得到一个伸缩容器。Flexbox 定义了伸缩容器内伸缩项目该如何布局。

**不定宽高的垂直水平居中方法**
方法1：
```
.parent{
position:relative;
}
.content{
position:absoulte;
top:50%;
left:50%;
transform:translate(-59%,-50%);
}
```
方法2：
```
.parent{
justify-content:center;
align-items:center;
display:-webkit-flex;
}
```
[点我了解FFC牛逼的地方](https://mabiao8023.github.io/web-flex/)
