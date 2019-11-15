---
layout:     post
title:    面试整理
subtitle:  
date:       2019-11-15
author:     
header-img: 
catalog: true
tags:
    - < 面试题整理 >
typora-root-url: ..

---



# grid布局

```css
 display: grid 
```

## 1.colums，rows

```css
grid-template-columns//属性定义每一列的列宽，
grid-template-rows//属性定义每一行的行高。
```

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
/*也可以写成repeat*/
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
/*repeat()接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。

repeat()重复某种模式也是可以的。*/
grid-template-columns: repeat(2, 100px 20px 80px);
```

## 2.auto-fill 关键字

 单元格的大小是固定的，但是容器的大小不确定 . `auto-fill`关键字表示自动填充。 

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

## 3. fr 关键字 

 如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。 

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
/*也可以写成绝对宽度*/
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

## 4.minmax()

 `minmax()`函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。 

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
/*minmax(100px, 1fr)表示列宽不小于100px，不大于1fr。*/
```

## 5.auto关键字

浏览器自己决定长度---自适应

```css
grid-template-columns: 100px auto 100px;
/*等于该列单元格的最大宽度*/
```

## 6.网格线名称

 `grid-template-columns`属性和`grid-template-rows`属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。 

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

## 7.两栏布局/十二网格布局

```css
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}
grid-template-columns: repeat(12, 1fr);
```

## 8.grid-gap 属性

 设置行与行的间隔（行间距） 

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

## 9.grid-template-areas 属性

 用于定义区域。 

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```

## 10.grid-auto-flow 属性

 这个顺序由`grid-auto-flow`属性决定，默认值是`row`，即"先行后列"。也可以将它设成`column`，变成"先列后行"。 

```
grid-auto-flow: column;
```

## 11.justify-items 属性， align-items 属性， place-items 属性

```CSS

.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
/*start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）。*/
/*整个内容区域在容器里面的位置*/
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

