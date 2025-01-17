[![lib]](../index.md)

[lib]:https://img.shields.io/badge/js-lib-4FC08D?style=social&labelColor=339966&logo=javascript&logoColor=339966

# 生成随机UID

```javascript
console.group('生成随机UID:')
const genUid = (length = 20) => {
    var soupLength = genUid.soup_.length
    var id = []
    for (var i = 0; i < length; i++) {
        id[i] = genUid.soup_.charAt(Math.random() * soupLength)
    }
    return id.join('')
}
genUid.soup_ = '!#$%()*+,-./:;=?@[]^_`{|}~ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
console.log(genUid())
console.groupEnd()
```

# 无loop生成指定长度的数组
```javascript
        console.group('无loop生成指定长度的数组')
        const List = len => [...new Array(len).keys()]
        const list = List(10) // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
        console.log(list)
        console.groupEnd()
```

# 一行代码去重数组
```javascript
        console.group('一行代码去重数组')
        var list = [1, 1, 2, 3, 6, 45, 8, 5, 4, 6, 5]
        var uniqueList = [...new Set(list)] // [1, 2, 3, 6, 45, 8, 5, 4]
        console.log(uniqueList)
        console.groupEnd()
```

# RGB色值生成16进制色值
```javascript
        console.group('RGB色值生成16进制色值')
        var rgb2Hex = rgb => {
            let rgbList = rgb.toString().match(/\d+/g)
            let hex = '#'
            for (let i = 0, len = rgbList.length; i < len; ++i) {
                hex += ('0' + Number(rgbList[i]).toString(16)).slice(-2)
            }
            return hex
        };
        var res = rgb2Hex('100, 50, 0') // '#643200'
        console.log(res)
        console.groupEnd()
```

# 颜色混合
```javascript
        console.group('颜色混合')
        const colourBlend = (c1, c2, ratio) => {
            ratio = Math.max(Math.min(Number(ratio), 1), 0)
            let r1 = parseInt(c1.substring(1, 3), 16)
            let g1 = parseInt(c1.substring(3, 5), 16)
            let b1 = parseInt(c1.substring(5, 7), 16)
            let r2 = parseInt(c2.substring(1, 3), 16)
            let g2 = parseInt(c2.substring(3, 5), 16)
            let b2 = parseInt(c2.substring(5, 7), 16)
            let r = Math.round(r1 * (1 - ratio) + r2 * ratio)
            let g = Math.round(g1 * (1 - ratio) + g2 * ratio)
            let b = Math.round(b1 * (1 - ratio) + b2 * ratio)
            r = ('0' + (r || 0).toString(16)).slice(-2)
            g = ('0' + (g || 0).toString(16)).slice(-2)
            b = ('0' + (b || 0).toString(16)).slice(-2)
            return '#' + r + g + b
        }
        res = colourBlend('#ff0000', '#3333ff', 0.5) // "#991a80"
        console.log(res)
        console.groupEnd()
```

# 判断是否为质数
```javascript
        console.group('判断是否为质数')
        const mathIsPrime = n => {
            if (n === 2 || n === 3) {
                return true
            }
            if (isNaN(n) || n <= 1 || n % 1 != 0 || n % 2 == 0 || n % 3 == 0) {
                return false;
            }
            for (let x = 6; x <= Math.sqrt(n) + 1; x += 6) {
                if (n % (x - 1) == 0 || n % (x + 1) == 0) {
                    return false
                }
            }
            return true
        }
        console.log(mathIsPrime(7)) // true
        console.groupEnd()
```

# 判断对象类型
```javascript
        console.group('判断对象类型')
        var type = data => Object.prototype.toString.call(data).replace(/^\[object (.+)\]$/, '$1').toLowerCase()
        res = type({}) // object
        console.log(res)
        console.groupEnd()
```

# 遍历类数组对象
```javascript
        console.group('遍历类数组对象')
        const elements = document.querySelectorAll('.selector');
        Array.prototype.forEach.call(elements, (el, idx, list) => {
            console.log(el) // 元素节点
            console.log(idx) // 元素节点
            console.log(list) // 元素节点
        })
        console.groupEnd()
```

# 时间格式化
```javascript
        console.group('时间格式化')
        const dateFormatter = (formatter, date) => {
            date = (date ? new Date(date) : new Date)
            const Y = date.getFullYear() + '',
                M = date.getMonth() + 1,
                D = date.getDate(),
                H = date.getHours(),
                m = date.getMinutes(),
                s = date.getSeconds()
            return formatter.replace(/YYYY|yyyy/g, Y)
                .replace(/YY|yy/g, Y.substr(2, 2))
                .replace(/MM/g, (M < 10 ? '0' : '') + M)
                .replace(/DD|dd/g, (D < 10 ? '0' : '') + D)
                .replace(/HH|hh/g, (H < 10 ? '0' : '') + H)
                .replace(/mm/g, (m < 10 ? '0' : '') + m)
                .replace(/ss/g, (s < 10 ? '0' : '') + s)
        }

        res = dateFormatter('YYYY-MM-DD HH:mm:ss', '1995/02/15 13:55') // 1995-02-15 13:55
        console.log(res)
        console.groupEnd()
```