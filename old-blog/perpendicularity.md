# perpendicularity

```
  记录垂直居中的各种方式。
```

1. 文本

*   1\)单行垂直居中

    单行文本居中最简单，只需要在父级css设置

```
   .center {
            height: 100px;
            width: 100px;
            background:slateblue;
            text-align: center;
            line-height: 100px;
        }
    <div class="center">
        what?
    </div>
```

![](http://148.70.128.231:8080/staic/blog/center/1.png)

* 2\)多行垂直居中

```
    .center {
            display: table-cell;
            text-align: center;
            vertical-align: middle;
            background: cornflowerblue;
            height: 100px;
            width: 100px;
        }
    <div class="center">
        what?what?what?what?what?
    </div>
```

![](http://148.70.128.231:8080/staic/blog/center/2.png) 2. 块

* 1\)绝对定位居中

```
    .father 
    {
        background:deepskyblue;
        height: 150px;
        width: 150px;
        position: relative;
    }
    .son 
    {
        background: darkorchid;
        height: 100px;
        width: 100px;
        position: absolute;
        top: 0;
        bottom: 0;
        right: 0;
        left: 0;
        margin: auto;
    }
    <div class="father">
        <div class="son"></div>
    </div>
```

![](http://148.70.128.231:8080/staic/blog/center/3.png)

* 2\)相对定位

```
   .father 
    {
        background:deepskyblue;
        height: 150px;
        width: 150px;
        position: relative;
    }
    .son 
    {
        background: darkorchid;
        height: 100px;
        width: 100px;
        position: relative;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    <div class="father">
        <div class="son"></div>
    </div>
```

![](http://148.70.128.231:8080/staic/blog/center/3.png)\
3\. 万能的flex，我最爱用。(文本和块都可以!)

```
    .father 
    {
        display:flex;
        justify-align:center;
        align-items:center;
    }
    .son
    {
        background: darkorchid;
        height: 100px;
        width: 100px;
    }
    <div class="father">
        <div class="son"></div>
    </div>
```
