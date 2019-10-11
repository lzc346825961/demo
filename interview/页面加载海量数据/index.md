# 页面加载海量数据

[![randerData]](../index.md)

[randerData]:https://img.shields.io/badge/页面加载海量数据-randerData-4FC08D?style=social&labelColor=339966&logo=Vue.js&logoColor=339966

## 题目

> 10w 条记录的数组，一次性渲染到页面上，如何处理可以不冻结UI？

## 具体化

> 页面上有个空的无序列表节点 ul ，其 id 为 list-with-big-data ，现需要往列表插入 10w 个 li ，每个列表项的文本内容可自行定义，且要求当每个 li 被单击时，通过 alert 显示列表项内的文本内容。

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>页面加载海量数据</title>
</head>

<body>
  <ul id="list-with-big-data">100000 数据</ul>
  <script>
    // 此处添加你的代码逻辑
  </script>
</body>

</html>
```

## 分析
***
可能在看到这个问题的第一眼，我们可能会想到这样的解决办法：获取 ul 元素，然后新建 li 元素，并设置好 li 的文本内容和监听器绑定，然后在循环里对 ul 进行 append 操作，即可能想到的是以下代码实现。

```javascript
(function() {
  const ulContainer = document.getElementById("list-with-big-data");

  // 防御性编程
  if (!ulContainer) {
    return;
  }

  for (let i = 0; i < 100000; i++) {
    const liItem = document.createElement("li");

    liItem.innerText = i + 1;
    // EventListener 回调函数的 this 默认指向当前节点，若使用箭头函数，得谨慎
    liItem.addEventListener("click", function() {
      alert(this.innerText);
    });
    ulContainer.appendChild(liItem);
  }
})();
```

实践上述代码，我们发现界面体验很不友好，卡顿感严重。出现卡顿感的主要原因是，在每次循环中，都会修改 DOM 结构，并且由于数据量大，导致循环执行时间过长，浏览器的渲染帧率过低。
事实上，包含 100000 个 `li` 的长列表，用户不会立即看到全部，只会看到少部分。因此，对于大部分的 `li` 的渲染工作，我们可以延时完成。
我们可以从 **减少 DOM 操作次数** 和 **缩短循环时间** 两个方面减少主线程阻塞的时间。

### DocumentFragment

> The DocumentFragment interface represents a minimal document object that has no parent. It is used as a lightweight version of [Document](https://developer.mozilla.org/en-US/docs/Web/API/Document) that stores a segment of a document structure comprised of nodes just like a standard document. The key difference is that because the document fragment isn't part of the active document tree structure, changes made to the fragment don't affect the document, cause [reflow](https://developer.mozilla.org/en-US/docs/Glossary/reflow), or incur any performance impact that can occur when changes are made.

在 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment) 的介绍中，我们知道可以通过 `DocumentFragment` 的使用，减少 DOM 操作次数，降低回流对性能的影响。

### requestAniminationFrame

> The **window.requestAnimationFrame()** method tells the browser that you wish to perform an animation and requests that the browser call a specified function to update an animation before the next repaint. The method takes a callback as an argument to be invoked before the repaint.

在缩短循环时间方面，我们可以通过 **分治** 的思想，将 100000 个 `li` 分批插入到页面中，并且我们通过 `requestAniminationFrame` 在页面重绘前插入新节点。
### 事件绑定
如果我们想监听海量元素，推荐方法是使用 JavaScript 的事件机制，实现事件委托，这样可以显著减少 DOM 事件注册 的数量。
## 解决方案
***
经过上面的讨论，我们有了如下的解决方案。

```javascript
(function() {
  const ulContainer = document.getElementById("list-with-big-data");

  // 防御性编程
  if (!ulContainer) {
    return;
  }

  const total = 100000; // 插入数据的总数
  const batchSize = 4; // 每次批量插入的节点个数，个数越多，界面越卡顿
  const batchCount = total / batchSize; // 批处理的次数
  let batchDone = 0; // 已完成的批处理个数

  function appendItems() {
    // 使用 DocumentFragment 减少 DOM 操作次数，对已有元素不进行回流
    const fragment = document.createDocumentFragment();

    for (let i = 0; i < batchSize; i++) {
      const liItem = document.createElement("li");
      liItem.innerText = batchDone * batchSize + i + 1;
      fragment.appendChild(liItem);
    }

    // 每次批处理只修改 1 次 DOM
    ulContainer.appendChild(fragment);
    batchDone++;
    doAppendBatch();
  }

  function doAppendBatch() {
    if (batchDone < batchCount) {
      // 在重绘之前，分批插入新节点
      window.requestAnimationFrame(appendItems);
    }
  }

  // kickoff
  doAppendBatch();

  // 使用 事件委托 ，利用 JavaScript 的事件机制，实现对海量元素的监听，有效减少事件注册的数量
  ulContainer.addEventListener("click", function(e) {
    const target = e.target;

    if (target.tagName === "LI") {
      alert(target.innerText);
    }
  });
})();
```