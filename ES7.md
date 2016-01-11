### 官方文档

#### [Decorators修饰符:](https://github.com/wycats/javascript-decorators)

https://github.com/wycats/javascript-decorators

#### [async-await:](https://github.com/tc39/ecmascript-asyncawait)

http://tc39.github.io/ecmascript-asyncawait/
```js
  async function chainAnimationsAsync(elem, animations) {
        let ret = null;
        try {
            for(const anim of animations) {
                ret = await anim(elem);
            }
        } catch(e) { /* ignore and keep going */ }
        return ret;
    }
```

### 计划废除
* Object Observe
