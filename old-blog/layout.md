# layout

```
记录一些页面常用布局
```

1. 3栏两边固定，中间宽度自适应。

*   flexble个人最喜欢

    ```
    .fa{
        height:100%:
        width:100%;
        display:flex;
    }
    .left{
        width:100px;
        background:red;
    }
    .right{
        width:100px;
        background:red;
    }
    .center{
        flex:1;
    }
    <div class='fa'>
        <div class='left'></div>
        <div class='center'></div>
        <div class='right'></div>
    <div>
    ```
* margin or padding

```
  .fa{
       height:100%:
       width:100%;
   }
   .left{
       width:100px;
       background:red;
   }
   .right{
       width:100px;
       background:red;
   }
   .center{
       width:100%;
       margin:0px 100px;
   }
   <div class='fa'>
       <div class='left'></div>
       <div class='center'></div>
       <div class='right'></div>
   <div>
```
