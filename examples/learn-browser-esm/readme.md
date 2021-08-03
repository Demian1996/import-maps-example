# 在浏览器中使用 esm

## 一、简介

该示例主要是测试 esm 在浏览器中的使用方式。

## 二、运行

```shell
npm start
```

## 三、详细说明

现有一个公共模块 count，拥有 increase、decrease、outputCount 方法，以 esm 规范导出功能。假设现在要调用两次 increase，一次 decrease，然后输出 count 值。下面将分别测试如何在浏览器中使用该模块完成相应功能的调用。

### 方式一、新建 main 调用模块，然后在浏览器中引入

如题所示，新建 main.js 文件，在 main.js 中完成对 count 模块的功能调用。然后在 html 中直接引入 main.js 文件。

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

### 方式二、直接在浏览器中调用模块

直接新建 `type="module"` 的 script 标签，然后在标签中引入 count 模块，完成功能调用

```html
<script type="module">
  import { increase, decrease, outputCount } from 'http://localhost:3000/count.js';
  // import { increase, decrease, outputCount } from './count.js';

  increase();
  increase();
  decrease();
  outputCount();
</script>
```

需要注意，esm 支持使用 url 的方式引入模块，既可以写绝对路径，也可写绝对路径。

## 四、总结

上面介绍了 esm 使用的两种方式，可以看到 esm 支持使用 url 的方式导入其他在线的模块。但是 url 过长的话修改和复用起来就很麻烦。对于该问题，有一个提案 [import-maps](https://github.com/WICG/import-maps) 允许通过维护一个 `import map` 映射，通过将模块映射到 url 后，可以直接使用`from '缩写名'`的形式。提案中将该 idea 定义为`allows "bare import specifiers"`定义为详细见同级目录 learn-import-maps。
