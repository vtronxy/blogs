# Generator 函数理解

## 写法

```javascript
    function* fibonacci() {
        let [prev, curr] = [0, 1];
        for (;;) {
            [prev, curr] = [curr, prev + curr];
            yield curr;
        }
    }

    for (let n of fibonacci()) {
        if (n > 1000) break;
        console.log(n);
    }
```

函数的返回值是 Iterator对象

## 迭代器

具备 myObject[Symbol.iterator] 是一个具备next方法的对象
{next:function(){}},
next方法返回 {value,done}

## for-of / Array.from / spread operator

调用对象的 myObject[Symbol.iterator].next(); 直到返回{value:undefined,done:true}

## yield表达式

- 作为函数的参数

- 在赋值表达式的右边

- 在表达式当中 
> 此时yield表达式 必须使用() 括号包起来

## yield* 表达式

在一个generator函数中 执行另一个generator
