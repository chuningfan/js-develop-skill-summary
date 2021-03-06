# mimiprogram - Native 小程序原生

## 目录
- [setData](#setData)
- [navigateTo与redirectTo](#navigateTo与redirectTo)


<br><br><br><br><br><br>

## setData

setData 是一个将 data 中定义的数据进行修改并刷新到视图的函数。但需要注意的是，它是一个异步函数，如果刷新的数据内容较多，需要注意后续的东西要在 setData 的回调中进行处理，否则会有数据不准确的问题

```js
this.setData({
    a: this.data.a,
    b: this.data.b
},()=>{
    //do your stuff
});
```

**性能优化**

由于 setData 会修改数据并刷新视图，所以在执行 setData 应尽可能只更新必要更新的数据，以免浪费渲染性能，造成界面卡顿的感觉；在更新 Object 或 Array 若能明确需要更新的位置，应更新指定位置的内容

```js
this.setData({
    'list[1].name': 'zhangsan'
});
```

路径变量

```js
let path = 'list[' + i+ '].name'; 
this.setData({
    [path]: 'zhangsan'
});
```

<br><br>

## navigateTo与redirectTo

`wx.navigateTo` 与 `wx.redirectTo` 以功能来说，同样是跳转到新页面的功能函数，它们之间有几点区别

- `wx.navigateTo` 保留当前页面，而 `wx.redirectTo` 会关闭当前页面
- 同是在目标页面的 url 中增加与 Web Url类似的链接参数，并可在目标页面的 `onLoad(options)` 中获取
- `wx.navigateBack` 可以返回通过 `wx.navigateTo` 进行跳转的界面，通过 `wx.redirectTo` 进行跳转的不能返回
- 微信小程序规定 `wx.navigateTo` 只允许有五层页面路径，当已经有五层页面路径时，再进行跳转，将不会执行跳转页面功能。
- `wx.navigateBack` 可通过 `getCurrentPages()` 函数获得页面栈，并决定需要返回的层级

为了防止小程序页面跳转不了的情况常，页面切换功能应按需使用，部分操作完成后不需要保留的页面，应尽可能使用 `wx.redirectTo` 进行跳转

<br><br>
