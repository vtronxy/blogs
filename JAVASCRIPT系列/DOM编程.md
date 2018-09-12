# DOM编程基础

## DOM大小及位置 client,offset,scroll

## addEventListener添加事件handle
> 通过 addEventListener() 来添加的匿名函数无法移除

## removeChild与绑定的事件Handler


## 统计页面中节点数目
```javascript
function countNodes(node) {
    //  计算自身
    var count = 1;
    //  判断是否存在子节点
    if(node.hasChildNodes()) {
        //  获取子节点
        var cnodes = node.childNodes;
        //  对子节点进行递归统计
        for(var i=0; i<cnodes.length; i++) {
            count = count + countNodes(cnodes.item(i))
        }
    }
    return count;
}

//使用方法
countNodes(document.body);
```
## 参考链接
* https://www.w3cplus.com/javascript/offset-scroll-client.html DOM位置及大小
* https://segmentfault.com/q/1010000008962585 removeChild 与绑定的event handler
* https://www.zhihu.com/question/35335233 addEventListener添加匿名函数
