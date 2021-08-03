# systemjs

## 一、简介

该示例主要是测试 systemjs 在浏览器中的使用方式。

## 二、运行

```shell
npm start
open http://localhost:3002
```

## 三、systemjs

相比于 import-maps，systemjs 的兼容性更好，同时 systemjs 的使用方式和 esm+import-maps 很像，如下所示：

```html
<script type="systemjs-importmap">
  {
    "imports": {
      "lodash": "https://unpkg.com/lodash@4.17.10/lodash.js"
    }
  }
</script>
<script>
  System.import('lodash').then(({ map }) => {
    const arr = [{ a: '1' }, { a: '2' }];
    console.log(map(arr, 'a'));
  });
</script>
```
