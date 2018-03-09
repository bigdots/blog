# Promise

原理：
1. 三种状态，且不可逆
2. 队列机制，收集异步操作
3. setTimeOut 模拟队列末尾执行

```js
// promise 实现
function Promise(fn) {
    // 私有属性，只可供对外暴露的API访问和修改
    var state = 'pending',
        value = null,
        deferreds = []; //存放一个异步操作

    // 对外暴露的属性，特权属性
    this.handle = function(deferred) {
        if (state === 'pending') {
            deferreds.push(deferred);
            return;
        }

        var cb = state === 'fulfilled' ? deferred.onFulfilled : deferred.onRejected,
            ret;
        if (cb === null) {
            cb = state === 'fulfilled' ? deferred.resolve : deferred.reject;
            cb(value);
            return;
        }
        ret = cb(value);
        deferred.resolve(ret);
    }

    function resolve(newValue) {
        if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
            var then = newValue.then;
            if (typeof then === 'function') {
                then.call(newValue, resolve, reject);
                return;
            }
        }
        state = 'fulfilled';
        value = newValue;
        finale();
    }

    function reject(reason) {
        state = 'rejected';
        value = reason;
        finale();
    }

    function finale() {
        setTimeout(function () {
            deferreds.forEach(function (deferred) {
                handle(deferred);
            });
        }, 0);
    }

    fn(resolve, reject);
}

Promise.prototype.then = function (onFulfilled, onRejected) {
    return new Promise(function (resolve, reject) {
        this.handle({
            onFulfilled: onFulfilled || null,
            onRejected: onRejected || null,
            resolve: resolve,
            reject: reject,
        });
    });
};

Promise.prototype.catch = function(){

}
```