# url参数解析

```js
// 参数解析
function parseQuery(url) {
    let u = url.split('?');
    if (u.length === 2) {
        let paramString = u[1].split('&');
        return paramString.reduce((result, current) => {
            let param = current.split('=');
            param.length === 2 && (result[param[0]] = param[1]);
            return result;
        }, {});
    } else {
        throw '非法参数';
    }
}
```