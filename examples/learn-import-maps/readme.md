# import-maps

## 一、简介

该示例主要是测试 import-maps 在浏览器中的使用方式。

## 二、运行

```shell
npm start
open http://localhost:3001
```

## 三、esm

假设要在项目中使用 lodash，如果是用 esm 的形式，那么代码可能如下所示：

```html
<script type="module">
  import { map } from 'https://cdn.jsdelivr.net/npm/lodash-es@4.17.21/lodash.min.js';

  const arr = [{ a: '1' }, { a: '2' }];
  console.log(map(arr, 'a'));
</script>
```

可以看到通过 url 导入了在线的 lodash 模块，然后在代码中进行了使用。但是 url 一方面过长，另一方面如果有多个 js 文件，每个文件引入 lodash 都需要写很长的 url 链接。于是有了 import-maps 提案，该提案运行通过维护一份映射表的形式，将 lodash 映射到详细 url 上，然后通过`import 'lodash'`的方式导入 lodash。

## 四、使用 import-maps

### 1、简介

如果要使用 import-maps，需要新建一个 script 标签，将 type 设置为`importmap`，然后在其中填写相关的映射。如下所示，将 lodash 映射到 `v4.17.21` 版本的线上地址：

```html
<script type="importmap">
  {
    "imports": {
      "lodash": "https://cdn.jsdelivr.net/npm/lodash-es@4.17.21/lodash.min.js"
    }
  }
</script>
```

在添加了上面的标签后，原来的代码就可以直接改为

```js
import { map } from 'lodash';
```

import-maps 还提供了诸如 scopes 的功能，这里不过多介绍，详细见官方提案的仓库。

### 2、缺点

import-maps 是个很好的提案，大大方便了在浏览器端引入 esm 规范的模块。但是作为一个较新的提案，浏览器兼容性很差，详细见[import-maps 兼容性](https://caniuse.com/?search=import-maps)。ie 和 firefox 不支持，chrome 的较新的版本才支持。
由于兼容性的一些问题，import-maps 还不是一种可以直接拿起来就用的方案。此时 systemjs 就可以作为其 polyfill，成为我们最好的替代选择。

## 五、总结

import-maps 作为较新的提案，大大地方便在浏览器端引入线上模块，但是浏览器兼容性较差，所以需要使用 `systemjs` 作为替代选择。
