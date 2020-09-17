---
layout: post
title:  "[Webpack] Asset Management"
date:   2020-09-17 14:07:00 +0900
categories: unclassified
---

# Asset Management
https://webpack.js.org/guides/asset-management/

처음부터 가이드를 따랐다면 이제 'Hello webpack'을 표시하는 작은 프로젝트를 갖게됩니다. 이제 이미지와 같
은 다른 자산을 통합하여 처리 방법을 살펴 보겠습니다.

웹팩 이전에, 프런트 엔드 개발자는 grunt 및 gulp와 같은 도구를 사용하여 이러한 자산을 처리하고 `/src` 폴더에서 `/dist` 또는`/build` 디렉토리로 이동했습니다. 자바 스크립트 모듈에도 동일한 아이디어가 사용되었지만, 웹팩과 같은 도구는 모든 종속성을 ** dynamically bundle ** ([종속성 그래프](https://webpack.js.org/concepts/dependency-graph) 생성) 합니다. 이제 모든 모듈이 _ 그 종속성을 명시 적으로 표시하고 _ 사용하지 않는 모듈을 번들링하는 것을 피할 수 있기 때문에 이것은 훌륭합니다.

가장 멋진 웹팩 기능 중 하나는 로더가있는 JavaScript 외에 다른 유형의 파일도 포함 할 수 있다는 것입니다. 즉, JavaScript에 대해 위에 나열된 동일한 이점 (예 : 명시적 종속성)이 웹 사이트 또는 웹 앱을 빌드하는 데 사용되는 모든 것에 적용될 수 있습니다. 이미 해당 설정에 익숙할 수 있으므로 CSS부터 시작하겠습니다.

## Setup[](https://webpack.js.org/guides/asset-management/#setup)

시작하기 전에 프로젝트를 약간 변경해 보겠습니다:

**dist/index.html**

```diff
  <!doctype html>
  <html>
    <head>
-    <title>Getting Started</title>
+    <title>Asset Management</title>
    </head>
    <body>
-     <script src="main.js"></script>
+     <script src="bundle.js"></script>
    </body>
  </html>
```

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
-     filename: 'main.js',
+     filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
  };
```

## Loading CSS[](https://webpack.js.org/guides/asset-management/#loading-css)

In order to  `import`  a CSS file from within a JavaScript module, you need to install and add the  [style-loader](https://webpack.js.org/loaders/style-loader)  and  [css-loader](https://webpack.js.org/loaders/css-loader)  to your  [`module`  configuration](https://webpack.js.org/configuration/module):
자바 스크립트 모듈 내에서 CSS 파일을 'import'하려면 [style-loader](https://webpack.js.org/loaders/style-loader) 및 [css-loader](https://webpack.js.org/loaders/css-loader)를 설치하고 추가해야합니다.를 [`module`구성](https://webpack.js.org/configuration/module)에 추가합니다.

```bash
npm install --save-dev style-loader css-loader
```

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
+   module: {
+     rules: [
+       {
+         test: /\.css$/,
+         use: [
+           'style-loader',
+           'css-loader',
+         ],
+       },
+     ],
+   },
  };
```
모듈 로더는 체인으로 연결할 수 있습니다. 체인의 각 로더는 처리 된 리소스에 변환을 적용합니다. 체인은 역순으로 실행됩니다. 첫 번째 로더는 결과 (변환이 적용된 리소스)를 다음 로더로 전달합니다. 마지막으로 웹팩은 체인의 마지막 로더가 JavaScript를 반환 할 것으로 예상합니다.

위의 로더 순서를 유지해야합니다. `style-loader`가 먼저 나오고 그다음에 `css-loader`가 나옵니다. 이 규칙을 따르지 않으면 webpack에서 오류가 발생할 수 있습니다.

> webpack은 정규 표현식을 사용하여 특정 로더를 찾고 제공해야하는 파일을 결정합니다. 이 경우`.css`로 끝나는 모든 파일은`style-loader` 및`css-loader`에 제공됩니다.

이렇게하면 해당 스타일에 따라 './style.css'를 파일로 가져올 수 있습니다. 이제 해당 모듈이 실행될 때 문자열 화 된 CSS가있는`<style>`태그가 html 파일의`<head>`에 삽입됩니다.

새`style.css` 파일을 프로젝트에 추가하고`index.js`로 가져 와서 사용해 보겠습니다:

**project**

```diff
  webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
+   |- style.css
    |- index.js
  |- /node_modules
```

**src/style.css**

```css
.hello {
  color: red;
}
```

**src/index.js**

```diff
  import _ from 'lodash';
+ import './style.css';

  function component() {
    const element = document.createElement('div');

    // Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
+   element.classList.add('hello');

    return element;
  }

  document.body.appendChild(component());
```

Now run your build command:

```bash
npm run build

...
    Asset      Size  Chunks             Chunk Names
bundle.js  76.4 KiB       0  [emitted]  main
Entrypoint main = bundle.js
...
```

브라우저에서`index.html`을 다시 열면`Hello webpack`이 이제 빨간색으로 표시됩니다. 웹팩이 무엇을했는지 확인하려면 페이지를 검사하고 (`<style>`태그는 자바 스크립트에 의해 동적으로 생성되므로 결과를 표시하지 않으므로 페이지 소스를 보지 마십시오) 페이지의 헤드 태그를 살펴보십시오. `index.js`에서 가져온 스타일 블록을 포함해야합니다.

프로덕션에서 더 나은 로드시간을 위해 [css 최소화](https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production) 할 수 있으며 대부분의 경우 그렇게해야합니다. 그 외에도 [postcss](https://webpack.js.org/loaders/postcss-loader), [sass](https : // webpack .js.org / loaders / sass-loader) 및 [less](https://webpack.js.org/loaders/less-loader)를 예로들 수 있습니다.



## Loading Images[](https://webpack.js.org/guides/asset-management/#loading-images)

이제 CSS를 가져 오지만 배경 및 아이콘과 같은 이미지는 어떻습니까? [file-loader](https://webpack.js.org/loaders/file-loader)를 사용하여 시스템에도 쉽게 통합 할 수 있습니다.

```bash
npm install --save-dev file-loader
```

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ],
        },
+       {
+         test: /\.(png|svg|jpg|gif)$/,
+         use: [
+           'file-loader',
+         ],
+       },
      ],
    },
  };
```

이제 `import MyImage form './my-image.png'` 에서 해당 이미지가 처리되어 `output` 디렉토리에 추가되며,`MyImage` 변수에는 처리 후 해당 이미지의 최종 URL이 포함됩니다. 위와 같이 [css-loader](https://webpack.js.org/loaders/css-loader)를 사용하면 `url ( './ my-image.png')`에 대해서도 유사한 프로세스가 발생합니다. CSS 내에서, 로더는 이것이 로컬 파일임을 인식하고 `'./my-image.png'` 경로를 `output` 디렉토리에있는 이미지의 최종 경로로 바꿉니다. [html-loader](https://webpack.js.org/loaders/html-loader)는 `<img src = "./ my-image.png"/>` 를 동일한 방식으로 처리합니다.

프로젝트에 이미지를 추가하고 이것이 어떻게 작동하는지 살펴 보겠습니다. 원하는 이미지를 사용할 수 있습니다:

**project**

```diff
  webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
+   |- icon.png
    |- style.css
    |- index.js
  |- /node_modules
```

**src/index.js**

```diff
  import _ from 'lodash';
  import './style.css';
+ import Icon from './icon.png';

  function component() {
    const element = document.createElement('div');

    // Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
    element.classList.add('hello');

+   // Add the image to our existing div.
+   const myIcon = new Image();
+   myIcon.src = Icon;
+
+   element.appendChild(myIcon);

    return element;
  }

  document.body.appendChild(component());
```

**src/style.css**

```diff
  .hello {
    color: red;
+   background: url('./icon.png');
  }
```

Let's create a new build and open up the index.html file again:

```bash
npm run build

...
                               Asset      Size  Chunks                    Chunk Names
da4574bb234ddc4bb47cbe1ca4b20303.png  3.01 MiB          [emitted]  [big]
                           bundle.js  76.7 KiB       0  [emitted]         main
Entrypoint main = bundle.js
...
```

모든 것이 순조롭게 진행 되었다면 이제 아이콘이 반복되는 배경으로 표시되고 `Hello webpack` 텍스트 옆에 `img` 요소가 표시됩니다. 이 요소를 살펴보면, 실제 파일 이름이 `5c999da72346a995e7e2718865d019c8.png`와 같이 변경된 것을 볼 수 있습니다. 이것은 webpack이`src` 폴더에서 파일을 찾아서 처리했음을 의미합니다!


> 여기에서 논리적으로 다음 단계는 이미지를 축소하고 최적화하는 것입니다. [image-webpack-loader](https://github.com/tcoopman/image-webpack-loader) 및 [url-loader](https://webpack.js.org/loaders/url-loader)를 확인하세요. 이미지 로딩 프로세스를 향상시킬 수있는 방법에 대해 자세히 알아보십시오.

## Loading Fonts[](https://webpack.js.org/guides/asset-management/#loading-fonts)

그렇다면 글꼴과 같은 다른 자산은 어떻습니까? 파일 및 URL 로더는 로드하는 모든 파일을 가져와 빌드 디렉토리로 출력합니다. 즉, 글꼴을 포함한 모든 종류의 파일에 사용할 수 있습니다. 글꼴 파일을 처리하도록`webpack.config.js`를 업데이트 해 보겠습니다:

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ],
        },
        {
          test: /\.(png|svg|jpg|gif)$/,
          use: [
            'file-loader',
          ],
        },
+       {
+         test: /\.(woff|woff2|eot|ttf|otf)$/,
+         use: [
+           'file-loader',
+         ],
+       },
      ],
    },
  };
```

Add some font files to your project:

**project**

```diff
  webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
+   |- my-font.woff
+   |- my-font.woff2
    |- icon.png
    |- style.css
    |- index.js
  |- /node_modules
```

로더가 구성되고 글꼴이 제자리에 있으면 `@font-face` 선언을 통해 통합 할 수 있습니다. 로컬 `url (...)`지시문은 이미지에서와 같이 webpack에 의해 선택됩니다.

**src/style.css**

```diff
+ @font-face {
+   font-family: 'MyFont';
+   src:  url('./my-font.woff2') format('woff2'),
+         url('./my-font.woff') format('woff');
+   font-weight: 600;
+   font-style: normal;
+ }

  .hello {
    color: red;
+   font-family: 'MyFont';
    background: url('./icon.png');
  }
```

Now run a new build and let's see if webpack handled our fonts:

```bash
npm run build

...
                                 Asset      Size  Chunks                    Chunk Names
5439466351d432b73fdb518c6ae9654a.woff2  19.5 KiB          [emitted]
 387c65cc923ad19790469cfb5b7cb583.woff  23.4 KiB          [emitted]
  da4574bb234ddc4bb47cbe1ca4b20303.png  3.01 MiB          [emitted]  [big]
                             bundle.js    77 KiB       0  [emitted]         main
Entrypoint main = bundle.js
...
```

`index.html`을 다시 열고`Hello webpack` 텍스트가 새 글꼴로 변경되었는지 확인합니다. 모든 것이 잘되면 변경 사항을 확인해야합니다.

## Loading Data[](https://webpack.js.org/guides/asset-management/#loading-data)

Another useful asset that can be loaded is data, like JSON files, CSVs, TSVs, and XML. Support for JSON is actually built-in, similar to NodeJS, meaning  `import Data from './data.json'`  will work by default. To import CSVs, TSVs, and XML you could use the  [csv-loader](https://github.com/theplatapi/csv-loader)  and  [xml-loader](https://github.com/gisikw/xml-loader). Let's handle loading all three:
로드할 수 있는 또 다른 유용한 자산은 JSON 파일, CSV, TSV 및 XML과 같은 데이터입니다. JSON에 대한 지원은 실제로 기본 제공되며 NodeJS와 유사합니다. 즉, `import Data from './data.json'` 가 기본적으로 작동합니다. CSV, TSV 및 XML을 가져 오려면 [csv-loader](https://github.com/theplatapi/csv-loader) 및 [xml-loader](https://github.com/gisikw/xml)를 사용할 수 있습니다. 세 가지 모두 로드를 처리하겠습니다:

```bash
npm install --save-dev csv-loader xml-loader
```

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ],
        },
        {
          test: /\.(png|svg|jpg|gif)$/,
          use: [
            'file-loader',
          ],
        },
        {
          test: /\.(woff|woff2|eot|ttf|otf)$/,
          use: [
            'file-loader',
          ],
        },
+       {
+         test: /\.(csv|tsv)$/,
+         use: [
+           'csv-loader',
+         ],
+       },
+       {
+         test: /\.xml$/,
+         use: [
+           'xml-loader',
+         ],
+       },
      ],
    },
  };
```

Add some data files to your project:

**project**

```diff
  webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
+   |- data.xml
    |- my-font.woff
    |- my-font.woff2
    |- icon.png
    |- style.css
    |- index.js
  |- /node_modules
```

**src/data.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Mary</to>
  <from>John</from>
  <heading>Reminder</heading>
  <body>Call Cindy on Tuesday</body>
</note>
```

이제 네 가지 데이터 유형 (JSON, CSV, TSV, XML) 중 하나를 `import` 할 수 있으며 가져 오는 `Data` 변수에는 쉽게 사용할 수 있도록 구문 분석 된 JSON이 포함됩니다:

**src/index.js**

```diff
  import _ from 'lodash';
  import './style.css';
  import Icon from './icon.png';
+ import Data from './data.xml';

  function component() {
    const element = document.createElement('div');

    // Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
    element.classList.add('hello');

    // Add the image to our existing div.
    const myIcon = new Image();
    myIcon.src = Icon;

    element.appendChild(myIcon);

+   console.log(Data);

    return element;
  }

  document.body.appendChild(component());
```

`npm run build` 명령을 다시 실행하고`index.html`을 엽니 다. 개발자 도구에서 콘솔을 보면 가져온 데이터가 콘솔에 기록되는 것을 볼 수 있습니다!

> 이는 [d3](https://github.com/d3)와 같은 도구를 사용하여 일종의 데이터 시각화를 구현할 때 특히 유용 할 수 있습니다. ajax 요청을 작성하고 런타임에 데이터를 구문 분석하는 대신 빌드 프로세스 중에 모듈에로드하여 모듈이 브라우저에 도달하자마자 구문 분석 된 데이터가 준비되도록 할 수 있습니다.

> JSON 모듈의  default export  만 경고없이 사용할 수 있습니다.

```javascript
// No warning
import data from './data.json';

// Warning shown, this is not allowed by the spec.
import { foo } from './data.json';
```

## Global Assets[](https://webpack.js.org/guides/asset-management/#global-assets)

위에서 언급 한 모든 것의 가장 멋진 부분은 이러한 방식으로 애셋을 로드하면 보다 직관적 인 방식으로 모듈과 애셋을 그룹화 할 수 있다는 것입니다. 모든 것을 포함하는 전역 `/assets` 디렉토리에 의존하는 대신 자산을 사용하는 코드로 자산을 그룹화 할 수 있습니다. 예를 들어 다음과 같은 구조가 유용 할 수 있습니다.

```diff
- |- /assets
+ |– /components
+ |  |– /my-component
+ |  |  |– index.jsx
+ |  |  |– index.css
+ |  |  |– icon.svg
+ |  |  |– img.png
```

이 설정은 밀접하게 연결된 모든 것이 이제 함께 살기 때문에 코드를 훨씬 더 이식 가능하게 만듭니다. 다른 프로젝트에서 `/my-component`를 사용하고 싶다고 가정 해 봅시다. 간단히 복사하거나 거기에있는 `/components` 디렉토리로 이동하십시오. _external dependency_를 설치하고 _configuration에 동일한 로더가 정의되어있는 한 _ 계속 진행할 수 있습니다.
그러나 예전 방식에 잠겨 있거나 여러 구성 요소 (보기, 템플릿, 모듈 등)간에 공유되는 일부 자산이 있다고 가정 해 보겠습니다. 이러한 애셋을 기본 디렉토리에 저장하고 [aliasing](https://webpack.js.org/configuration/resolve/#resolvealias)을 사용하여 `import`를 더 쉽게 할 수도 있습니다.

## Wrapping up[](https://webpack.js.org/guides/asset-management/#wrapping-up)

다음 가이드에서는이 가이드에서 사용한 다른 자산을 모두 사용하지 않을 것이므로 다음 가이드 [출력 관리](https://webpack.com js.org/guides/output-management/)을 준비 할 수 있도록 정리를 수행하겠습니다.:

**project**

```diff
  webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- bundle.js
    |- index.html
  |- /src
-   |- data.xml
-   |- my-font.woff
-   |- my-font.woff2
-   |- icon.png
-   |- style.css
    |- index.js
  |- /node_modules
```

**webpack.config.js**

```diff
  const path = require('path');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
-   module: {
-     rules: [
-       {
-         test: /\.css$/,
-         use: [
-           'style-loader',
-           'css-loader',
-         ],
-       },
-       {
-         test: /\.(png|svg|jpg|gif)$/,
-         use: [
-           'file-loader',
-         ],
-       },
-       {
-         test: /\.(woff|woff2|eot|ttf|otf)$/,
-         use: [
-           'file-loader',
-         ],
-       },
-       {
-         test: /\.(csv|tsv)$/,
-         use: [
-           'csv-loader',
-         ],
-       },
-       {
-         test: /\.xml$/,
-         use: [
-           'xml-loader',
-         ],
-       },
-     ],
-   },
  };
```

**src/index.js**

```diff
  import _ from 'lodash';
- import './style.css';
- import Icon from './icon.png';
- import Data from './data.xml';
-
  function component() {
    const element = document.createElement('div');
-
-   // Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
-   element.classList.add('hello');
-
-   // Add the image to our existing div.
-   const myIcon = new Image();
-   myIcon.src = Icon;
-
-   element.appendChild(myIcon);
-
-   console.log(Data);

    return element;
  }

  document.body.appendChild(component());
```

## Next guide[](https://webpack.js.org/guides/asset-management/#next-guide)

Let's move on to  [Output Management](https://webpack.js.org/guides/output-management/)
