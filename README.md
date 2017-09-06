# browser-traps

> 在浏览器上开发的一些坑总结

1.  `console` 的支持情况，在 `ie8/9` 下如果不打开 `debug` 调试器，则会报错，参看 [`console` 支持情况](http://caniuse.com/#search=console)
2. `input` 事件问题，`input`事件是在 [`ie9+`](http://caniuse.com/#search=input) 支持的，但是在遇到 `ie10/11` 情况下，定义了 `placeholder` 属性时，输入框聚焦或者离开焦点时都会派发 `input` 事件，以及代码去赋值 `placeholder` 也会如此，所以这种情况下，还是使用 `keydown` 事件来代替处理更好些
3. `ie`  下 `ajax` 的 `get` 请求会被缓存返回304，打开调试器就不会，处理方式：1. 请求地址追加时间戳 2. 服务端设置 `Response Body` 消息头 `Cache-Control:max-age=0`
4. `ie` 下容器里包含一个 `disabled` 元素，这个元素还是能触发 `click` 事件，示例查看 http://output.jsbin.com/hukozi/13
5. `ie9/10` 下有时元素的 `clientHeight` 为 `0` ，原因 http://www.satzansatz.de/cssd/onhavinglayout.html
6. `ie8/9` 跨域 `Ajax` 问题限制，因为在 `ie8/9` 使用 `XDomainRequest` 而不是 `XMLHttpRequest`，导致一些限制 [Detail](https://github.com/MoonScript/jQuery-ajaxTransport-XDomainRequest)
    - 只能是 `GET/POST`
    - `POST` 请求时，传输数据 `Content-type` 必须是 `text/plain`
    - 只能是 `HTTP/HTTPS`
    - 协议调用必须相同
    - 只能是异步
7. 当元素高度为 `0` 时，它父元素的 `scrollWidth` 在不同浏览器值不一样，示例查看 https://stackoverflow.com/questions/45809092/element-scrollwidth-value-different-in-ie-chrome-firefox