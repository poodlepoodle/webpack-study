# CEOS 15기 프론트엔드 서브 스터디 - Webpack 1주차
  
![image](https://user-images.githubusercontent.com/6462456/172036327-e8172ac0-33df-4eed-939e-11c8818f9120.png)
_2주차(5월 30일 ~ 6월 5일) : Getting Started, Concepts, Tutorials_

# Webpack 맛보기 튜토리얼

![에비츄-먹방2](https://user-images.githubusercontent.com/6462456/172036633-676cf582-9ac6-4fec-a8fa-f4186961080d.gif)

## 개발 환경 구성

-  Node.js LTS Version(버전 10 이상)
-  NPM (버전 6 이상)

> [Node.js 설치 링크](https://nodejs.org/en/)

지난 1주차 스터디 내용에서 Node.js와 NPM에 대해서 다뤘기 때문에
다들 설치되어 있으실 텐데요~~~
혹시 모르니 한 번 더 설치 링크를 안내해 드립니다!!  

## 실습 절차

> 웹 페이지 자원 구성

```bash
npm init -y
```

빈 디렉토리를 구성하고 위 명령어로 package.json 파일을 생성합니다.  

```bash
npm i webpack webpack-cli -D
npm i lodash
```

다음은 해당 디렉토리에 Webpack 관련 라이브러리와
`lodash` 라이브러리를 설치합니다.  
`webpack-cli`는  **devDependencies** 의존성으로 설치합니다!!  

```html
<!-- index.html -->
<html>
  <head>
    <title>Webpack Demo</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="src/index.js"></script>
  </body>
</html>
```

```javascript
/* src/index.js */
function component() {
  var element = document.createElement('div');

  /* lodash is required for the next line to work */
  element.innerHTML = _.join(['Hello','webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

위의 내용을 가진 `index.html` 파일과 
`src/index.js` 파일 두 개를 생성해 줍니다.  

> 웹팩 빌드를 위한 구성 및 빌드

```javascript
import _ from 'lodash';     // 추가

function component() {
    ...
}

document.body.appendChild(component());
```

`index.js` 맨 위에 한 줄을 추가해 줍니다.  

```html
<!-- index.html -->
<html>
  <head>
    <title>Webpack Demo</title>
    <!-- <script src="https://unpkg.com/lodash@4.16.6"></script> -->
  </head>
  <body>
    <!-- <script src="src/index.js"></script> -->
    <script src="dist/main.js"></script>
  </body>
</html>
```

`index.html`의 내용 또한 수정해 줍니다.  

```json
// package.json
{
  ...
  "scripts": {
    "build": "webpack --mode=none"
  },
  ...
}
```

Webpack 빌드 명령어를 실행하기 위해 `package.json` 파일에
위의 Custom Command를 추가해 줍니다.  

```bash
npm run build
```

![image](https://user-images.githubusercontent.com/6462456/172037812-45e17ea5-597f-40b9-82b0-a0a447078f44.png)

위 명령어를 실행해 Webpack 빌드를 진행합니다.  

![image](https://user-images.githubusercontent.com/6462456/172037879-60e01119-9c6a-4ab6-815c-f01b569d6356.png)

그 다음으로, VScode 확장 탭에서 **Live Server**를 검색한 후
설치해 줍니다. `index.html`을 라이브 서버로 실행하기 위함이에요!  

![image](https://user-images.githubusercontent.com/6462456/172038270-8421af85-77bd-401e-830f-49e7418f5f3c.png)

![image](https://user-images.githubusercontent.com/6462456/172038316-c3866167-f24e-404d-a7b1-75317bfca88b.png)

그 다음, VScode 오른쪽 아래를 보면 **Go Live** 버튼이 있는데요,
`index.html`을 편집하는 상황에서 이 버튼을 클릭하면
바로 라이브 서버로 실행되어 창이 열리는 것을 확인할 수 있습니다!!  

```javascript
// webpack.config.js
// `webpack` command will pick up this config setup by default
var path = require('path');

module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

다음으로 루트 폴더에 `webpack.config.js` 파일을 생성한 후
위 내용을 작성해 줍니다.  

