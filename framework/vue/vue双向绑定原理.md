# [vue双向绑定原理](./index.md)
***
## vue双向绑定原理

vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。我们先来看Object.defineProperty()这个方法：

```javascript
var obj  = {};
Object.defineProperty(obj, 'name', {
        get: function() {
            console.log('我被获取了')
            return val;
        },
        set: function (newVal) {
            console.log('我被设置了')
        }
})
obj.name = 'fei';//在给obj设置name属性的时候，触发了set这个方法
var val = obj.name;//在得到obj的name属性，会触发get方法
```

已经了解到vue是通过数据劫持的方式来做数据绑定的，其中最核心的方法便是通过Object.defineProperty()来实现对属性的劫持，那么在设置或者获取的时候我们就可以在get或者set方法里假如其他的触发函数，达到监听数据变动的目的，无疑这个方法是本文中最重要、最基础的内容之一。

## 实现最简单的双向绑定

我们知道通过Object.defineProperty()可以实现数据劫持，是的属性在赋值的时候触发set方法，

```javascript
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div id="demo"></div>
    <input type="text" id="inp">
    <script>
        var obj  = {};
        var demo = document.querySelector('#demo')
        var inp = document.querySelector('#inp')
        Object.defineProperty(obj, 'name', {
            get: function() {
                return val;
            },
            set: function (newVal) {//当该属性被赋值的时候触发
                inp.value = newVal;
                demo.innerHTML = newVal;
            }
        })
        inp.addEventListener('input', function(e) {
            // 给obj的name属性赋值，进而触发该属性的set方法
            obj.name = e.target.value;
        });
        obj.name = 'fei';//在给obj设置name属性的时候，触发了set这个方法
    </script>
</body>
</html>
```
当然要是这么粗暴，肯定不行，性能会出很多的问题。

## 讲解vue如何实现