# JS的防抖与节流
[![Design pattern]](../index.md)

[Design pattern]:https://img.shields.io/badge/防抖与节流-debounce_throttle-4FC08D?style=social&logo=javascript&logoColor=339966

在进行窗口的resize、scroll，输入框内容校验等操作时，如果事件处理函数调用的频率无限制，会加重浏览器的负担，导致用户体验非常糟糕。此时我们可以采用debounce（防抖）和throttle（节流）的方式来减少调用频率，同时又不影响实际效果。

## 函数防抖（debounce）
将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。

```javascript
var debounce=function(fn, wait){
	var timer = null;
    return function(){
        if(timer!== null)   clearTimeout(timeout);        
        timer= setTimeout(fn, wait);    
    }
};
function yourfunc(){
	console.log(Math.random())
};
window.addEventListener('scroll',debounce(yourfunc,1000));
```

## 函数节流（throttle）
使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。

```javascript
var throttle = function(func, delay) {            
　　var timer = null;            
    return function() {                
        var context = this;               
        var args = arguments;                
        if (!timer) {
            timer = setTimeout(function() {                        
                func.apply(context, args);                        
                timer = null;                    
            }, delay);          
        }            
    }      
}  
function yourfunc() {
　　console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(yourfunc, 1000));
```

## 区别
函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。