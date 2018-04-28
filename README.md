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
8.  <details>
      <summary>`JavaScript` 处理数字问题</summary>
    问题场景

    前端页面通过 `ajax` 请求获取序列值，服务端返回的是 `Long` 型数值 `20171024005229743`，这时前端获取转化成 `Number` 时就变成 `20171024005229744`

    示例
    ```js
    +'20171024005229743' //output 20171024005229744
    ```

    `JavaScript` 里的 `Number` 是采用双精度浮点型 (IEEE-754 double-precision floating-point format numbers)

    它有一个安全整数范围 `-(2^53 - 1) ~ (2^53 - 1)` 即 `± 9007199254740991`，当超过这个范围后就不安全了

    示例

    ```js
    9007199254740993 === 9007199254740992 // output true
    ```

    在 `ECMAScript 2015` 提供了 `Number.MAX_SAFE_INTEGER/MIN_SAFE_INTEGER/isSafeInteger()` 来进行安全整形判断

    在 `Java` 里, `Integer` 整形范围 `-(2^31 - 1) ~ 2^31 - 1`, `Long` 长整型范围 `-(2^63 - 1) ~ 2^63 - 1`

    **因此服务端对应如果要做 `JSON` 对象传递前台时，长整型需要转成字符串返回**
    </details>

9.  <details>
      <summary>`IE8` 下事件对象不会传递到 `setTimeout` 函数里</summary>

      示例

      ```js
      $('button').on('click', function(e) {
        setTimeout(function() {
          // 这里 e.originalEvent 获取是空对象，访问里面属性报错 Member not found.
          console.log(e.originalEvent);
        }, 100);
      })
      ```

      这个时候在 `IE8` 下要做对象深拷贝处理

      ```diff
      $('button').on('click', function(e) {
        + var copyE = $.extend(true, {}, e);

        setTimeout(function() {
          console.log(copyE.originalEvent);
        }, 100);
      })
      ```
    </details>