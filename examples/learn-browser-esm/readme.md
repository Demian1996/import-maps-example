# 测试在浏览器中使用 esm

## 简介

该示例只要是测试 esm 在浏览器中的使用方式。

现假设有一个公共模块 count，拥有 increase、decrease、outputCount 方法，以 esm 规范导出功能。假设现在要调用两次 increase，一次 decrease，然后输出 count 值。下面将分别测试如何在浏览器中使用该模块完成相应功能的调用。

## 一、新建 main 调用模块，然后在浏览器中引入

如题所示，新建 main.js 文件，在 main.js 中完成对 count 模块的功能调用。然后在 html 中直接引入 main.js 文件

main.js:

```js
import { increase, decrease, outputCount } from './count.js';

increase();
increase();
decrease();
outputCount();
```

index.html:

```html
<script type="module" src="main.js"></script>
```

需要注意两点：
1、在使用 webpack 等打包工具时，我们可以省略文件后缀，使用`from './count'`的形式，但是在浏览器中需要补充文件后缀，不然会出现如下错误：

```txt
GET http://localhost:3000/count net::ERR_ABORTED 404 (Not Found)
```

2、引入使用 esm 规范的文件时，需要在 script 中添加`type="module"`，不然会报以下错误

```txt
Uncaught SyntaxError: Cannot use import statement outside a module
```

## 二、直接在浏览器中调用模块

直接新建 `type="module"` 的 script 标签，然后在标签中引入 count 模块，完成功能调用

```html
<script type="module">
  import { increase, decrease, outputCount } from 'http://localhost:3000/count.js';
  // import { increase, decrease, outputCount } from './count.js';

  increase();
  decrease();
  increase();
  outputCount();
</script>
```

需要注意，esm 支持使用 url 的方式引入模块，既可以写绝对路径，也可写绝对路径。
