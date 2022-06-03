# CEOS 15기 프론트엔드 서브 스터디 - Webpack 1주차

![image](https://user-images.githubusercontent.com/6462456/170818388-b71e1cb0-07c4-4ab5-821a-f2e965567f3d.png)  
_1주차(5월 23일 ~ 5월 29일) : Webpack, Motivation, Node.js & NPM_

# Webpack?

> **Webpack** : _모듈 번들러(Module Bundler)_

![image](https://user-images.githubusercontent.com/6462456/170855036-0e4a064d-13fb-46c5-9a6b-9142557715c1.png)

그렇군요...!  
근데 사실 아직은 무슨 말인지 잘 모르겠어요..ㅠ  
그럼, 먼저 **모듈**과 **모듈 번들링**에 대해서 짚고 넘어가 볼까요?

## 모듈?

> **모듈(Module)** : 웹 애플리케이션을 구성하는 모든 자원

웹 애플리케이션을 제작하기 위해 필요한
HTML, CSS, Javascript, Images, Font 등
많은 파일들 하나하나가 모두 모듈입니다.  

꼭 **자바스크립트 모듈**에만 국한되는 것은 아니네요..ㅎㅎ..  

## 모듈 번들링?

> **모듈 번들링(Module Bundling)**  
: 웹 애플리케이션을 구성하는 몇십, 몇백 개의 자원들을  
**하나의 파일로 병합 및 압축**해주는 동작  

![](https://joshua1988.github.io/webpack-guide/assets/img/webpack-bundling.e79747a1.png)
_모듈 번들링을 통해 자원들을 하나의 파일로 합칠 수 있어요._

> (다시 한 번) **Webpack** : _모듈 번들러(Module Bundler)_

![image](https://user-images.githubusercontent.com/6462456/170855060-8c199be2-03db-4fbc-8184-b3b8d8aa06e1.png)

그렇군요.. 그러면 이제 살짝은  
**Webpack**이 뭔지에 대해서 이해가 갈 것도.. 같아요!!  

# Webpack이 필요한 이유

Webpack이 갑자기 어느 날 **>뚝딱<** 하고
하늘에서 떨어지진 않았을 텐데요,,  
물론 등장하게 된 배경이 있고, 크게 세 가지로 요약할 수 있답니다.

## 1. 파일 단위의 자바스크립트 모듈 관리

```javascript
var a = 10;
console.log(a); // 10

function logText() {
  console.log(a); // 10
}
```

**자바스크립트의 변수 유효 범위**는 기본적으로 **전역(Global)**입니다.  
최대한 넓은 변수 범위를 갖기 때문에
어디에서도 접근하기가 편리하다는 장점이 있어요.  

> **하지만...** 

```html
<!-- index.html -->
<html>
  <head>
    <!-- ... -->
  </head>
  <body>
    <!-- ... -->
    <script src="./app.js"></script>
    <script src="./main.js"></script>
    <script>
      getNum(); // 20
    </script>
  </body>
</html>
```

```javascript
// app.js
var num = 10;
function getNum() {
  console.log(num);
}
```

```javascript
// main.js
var num = 20;
function getNum() {
  console.log(num);
}
```

위와 같은 상황을 가정해 볼까요?  
`app.js`와 `main.js`가 있고, 이를 `index.html`에서
`script` 태그를 통해 불러와 사용하는 상황입니다.  
이런 경우 `index.html`에서 호출한 `getNum()`의 결과는
20이 출력되네요.  
그 이유는 `app.js`와 `main.js`가 `num`이라는 같은 변수 네임을
사용하기 때문이에요. (같은 변수 스코프 상에 존재)  

![image](https://user-images.githubusercontent.com/6462456/170855124-36bfdaeb-56dd-44fa-a2a8-656d401bee7d.png)

실제로 복잡한 애플리케이션을 개발하는 상황을 가정한다면,
이러한 특징이 갖는 문제와, 동시에 모듈화의 필요성에 대해서
어느 정도 와 닿을 수 있겠네요...  

> AMD, Common.js, 그리고...

**AMD**나 **Common.js**, 그리고 원래 스터디 내용에 포함하려 했었지만..  
그냥 이 블로그 자체가 잘 정리되어 있어서 한 번 다들 읽어 보시면
좋을 것 같아 링크를 함께 남깁니다!!  

(참고 링크 : https://ingg.dev/webpack/)

## 2. 웹 개발 작업 자동화 도구

프론트엔드에서 개발을 진행하면서 이뤄지는 과정에 대해서 상상해 본다면,
아래와 같은 작업들이 존재할 수 있어요.

1. Editor에서 코드를 수정하고 저장한 뒤 브라우저에서 새로 고침
2. 배포 시 HTML, CSS, JS 압축
3. 배포 시 이미지 압축
4. 배포 시 CSS 전처리기 변환
5. what else?

이러한 일들을 **자동화**해주는 도구들이 필요했습니다.  
그래서 Grunt와 Gulp 같은 도구들이 등장했습니다.

## 3. 웹 애플리케이션의 빠른 로딩 속도와 높은 성능

Awesome한 웹 사이트를 구현하는 것만큼 중요한 것은,
**유저에게 사이트가 최대한 빨리 보여지도록** 만드는 거에요.  

![image](https://user-images.githubusercontent.com/6462456/170855146-2f407c18-96cf-4d2c-9a9a-a9f43ad3ce2e.png)  
_(멋진 랜딩 페이지를 만들었으나 로딩에 약 10여 초가 걸려서 슬픈 프론트 개발자)_

사이트 로딩 속도가 유저의 인내심(대략 5초)를 넘는 순간,
멋진 사이트를 만들기 위해 했던 모든 노력은 물거품이 되고 말 거에요...  

웹 사이트의 로딩 속도를 높이기 위한 가장 대표적인 노력들 중 하나는,
**브라우저에서 서버로 요청하는 파일 숫자를 줄이는 것**입니다.  
앞에서 살펴본 웹 태스크 매니저를 이용해 파일들을 압축하고 병합함으로써
절대적인 파일의 숫자를 줄이는 것이죠.  

또한, 초기에 유저에게 보여지는 화면에 존재하는 자원이 아니라면
필요에 의해, 나중에 보여질 때 **딱!!** 요청되게 할 수도 있겠네요.  
이는 **레이지 로딩(Lazy Loading)** 과 연결되는 개념입니다.  

기억하세요. Webpack의 철학은  
**<기본적으로 필요한 자원은 미리 로딩하는 게 아니라
그 때 그 때 요청하자>** 입니다!!  

# Webpack을 통해 해결하고자 하는 문제

1. 자바스크립트 변수 유효 범위
2. 브라우저별 HTTP 요청 숫자의 제약
3. 사용하지 않는 코드의 관리
4. Dynamic Loading & Lazy Loading 미지원

앞에서 살펴 본 내용들을 통해 Webpack으로 해결하고자 하는 문제를  
위의 4가지 정도로 정리해봤습니다~!

## 1. 자바스크립트 변수 유효 범위 문제

> ES6의 **Modules** 문법과 Webpack의 **모듈 번들링**으로 해결해요.  
> (참고 링크 : https://babeljs.io/docs/en/learn#modules)

```javascript
// math.js
export function sum(x, y) {
    return x + y;
}
export var pi = 3.141593;
```

```javascript
// app.js
import * as math from "math";
console.log("2π = " + math.sum(math.pi, math.pi));
```

ES6의 Modules 문법을 이용하는 경우는, `import` 키워드와
`export` 키워드를 이용해 독립적인 변수 스코프를 구성할 수 있어요.  
위와 같은 상황에서는 `math.js`에서 import한
`sum()`과 `pi`는 다른 파일의 동일한 이름의 변수와 구별될 수 있죠 ~_~  

## 2. 브라우저별 HTTP 요청 숫자의 제약

![image](https://user-images.githubusercontent.com/6462456/170854448-d601bb27-1b77-438f-b002-b0189ca27543.png)  
_최신 브라우저 별 최대 HTTP 요청 횟수_

최신 브라우저들은 위와 같이 한 번에 서버로 보낼 수 있는
HTTP 요청 숫자를 제한하고 있어요.
이 경우 **한 번에 서버에 보낸다**는 개념은 하나의 똑같은 도메인에
대한 동시 요청을 정의한다고 이해할 수 있어요.  

Webpack을 이용한다면 여러 개의 파일을 하나로 합칠 수 있으므로
위와 같이 브라우저 별 HTTP 요청 숫자 제약이 주어지더라도
이를 피할 수 있겠네요...! 그렇죠?  

> 좀 더 파고 들어가면...  

위의 브라우저 별 최대 요청 횟수는 HTTP 1에 적용되는 사항이며,
이를 피하기 위해 여러 개의 도메인을 두어 다량의 요청을
처리할 수 있도록 하는 **도메인 샤딩**이라는 방법 또한 존재한다고 하네요.  
HTTP 2의 달라진 구조에서는 동시 요청의 제한이 없으며
도메인 샤딩 또한 지양하는 것을 추천한다고 합니다.    

## 3. Dynamic Loading & Lazy Loading 미지원

Webpack 이전에는 `Require.js`와 같은 라이브러리를 쓰지 않는다면,
동적으로 원하는 순간에 모듈을 로딩하는 것이 불가능했어요.
그러나 Webpack의 `Code Splitting` 기능을 이용한다면
원하는 모듈을 원하는 타이밍에 로딩할 수 있습니다!!
_WOW!_

```javascript
/* Before */
import { add } from './math';

console.log(add(16, 26));
```

```javascript
/* After */
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

공식 React Documentation에서 제안하는 앱에 코드 분할을 도입하는
가장 좋은 방법은 동적 `import()` 문법을 사용하는 방법입니다!
Webpack이 이 구문을 만나게 되면 앱의 코드를 분할합니다.
`Create React App`이나 `Next.js` 를 사용한 경우
이미 Webpack이 구성이 되어 있기 때문에 즉시 사용할 수 있다네요~~ 데단헤..  
_자세한 내용은 아래 참고 링크 ㄱ ㄱ_  

(참고 링크 : https://webpack.js.org/guides/code-splitting/)

# Node.js와 NPM?

Webpack을 사용하기 위해서는 **Node.js**와 **NPM**에 대한 내용 또한
어느 정도 익힐 필요가 있는데요,  
프론트엔드 개발자라면 해당 도구들을 많이 사용하면서도
내부적인 원리에 대해서는 나중에 찾아봐야지.. 하고
일단 가져다 쓴 경우가 많을 것 같아요~~ _(저만 그런가요??)_  

![image](https://user-images.githubusercontent.com/6462456/170855181-fec33b02-bc6a-48eb-9559-c6b186bdb91a.png)

마침 이 기회에 각각 조금은 더 자세하게 알아보고 가면 좋겠네요~

## Node.js?

> **Node.js** : 브라우저에 종속적이지 않은 자바스크립트 런타임 환경

이전에는 자바스크립트 코드를 실행하는 환경은 브라우저의 역할이었습니다.
따라서 `.js` 파일로 작성한 후 `script` 태그 등으로 감싸 브라우저를 통해
실행됨으로써 DOM을 조작하는 등의 기능을 수행할 수 있었어요.  

하지만, Node.js부터는 웹 브라우저에 종속적인 환경에서 벗어나,
자바스크립트 애플리케이션을 여러 OS에서 실행할 수 있게 됐어요!!

> Node.js®는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다. Node.js는 이벤트 기반, Non 블로킹 I/O 모델을 사용해 가볍고 효율적입니다... (중략) ([공식 사이트](https://nodejs.org/en/)에 의한 설명)

**Node.js**는 Chrome V8 자바스크립트 엔진으로 빌드되었으며,
위에서도 설명했지만 이를 통해 자바스크립트 기반으로 구성된 서버 사이드 서비스를
자바스크립트 자체만으로 구현할 수 있습니다!!  
뿐만 아니라, 자바스크립트 애플리케이션 개발에 사용하기 위해 필요한 모듈,
파일 시스템, http 라이브러리 등을 built-in 으로 제공하기도 해요~!  

- 정적 파일 서버
- 웹 응용프로그램
- 메시징 미들웨어
- HTML5 멀티 플레이어 게임용 서버
- Everything you want!!

위는 Node.js로 할 수 있는 대표적인 작업들을 나열해 본 내용입니다!
[공식 홈페이지](https://nodejs.org/en/about/)를 참고하면
Node.js의 최대 장점은 **Non-blocking I/O**로 설명할 수 있어요.  
Non-blocking I/O는 입출력 처리를 완료하지 않은 상태에서
다른 처리 작업을 진행할 수 있도록 도중에 멈추지 않고
입출력 처리를 기다리는 방식이에요...!  

데이터베이스로부터 대량의 데이터를 조회하여 웹페이지에 표시하는 예시를 살펴보면,
**대기시간(blocking)**의 발생 때문에 순차적인 작업들이 밀리게 된다면
결과적으로 웹페이지 표시가 **지연되는 현상(lock)**이 발생할 수 있어요.  
하지만 Node.js의 경우 이러한 문제에 대해 모든 API를 비동기 방식으로
동작하도록 하여 Non-blocking I/O 가 가능하도록 합니다.
이러한 장점 때문에 Node.js가 가장 많이 권장되는 곳은 실시간 처리가 빈번한
SPA(Single Page Application) 개발이에요.

![image](https://user-images.githubusercontent.com/6462456/171799618-317df9d5-22bb-4430-b48b-724340af0aab.png)

Node.js가 뭔지, 그리고 웹 개발에 있어 얼마나 중요한지,
대략 느낄 수 있겠죠??  

## NPM?

> **NPM** : Node Package Manager

Node.js를 사용하면서 자주 쓰이고 재사용되는 자바스크립트 코드들을
**패키지**로 만들어 관리할 수 있는데요,
**NPM**은 그런 패키지들을 모아 놓은 오픈 소스 패키지 저장소입니다!  

실제로 개발하면서 필요한 기능들을 직접 개발할 수도 있지만,
이미 다른 개발자가 구현해 놓고 수많은 사람들이 사용하는 패키지가 있다면
NPM을 통해 필요한 패키지를 설치해 사용하는 것 또한
좋은 방법이 될 수 있습니다!!  

## Node.js 및 NPM 맛보기 👅

> Node.js 설치 : https://nodejs.org/en/download/

Node.js는 위의 공식 홈페이지를 통해 설치할 수 있습니다.
보통 LTS를 다운받는 경우가 무난하지만, Angular나
React 같이 특정 프레임워크를 사용하고자 설치하는 경우
설치하려는 Node.js 버전을 꼭 체크해 봐야 합니다!  
_(2022.06.01 기준 LTS : v16.15.0)_  

추가로, NPM은 Node.js 설치 시 함께 설치됩니다!!  

```bash
node -v # Node.js 버젼 설치
npm -v
```

![image](https://user-images.githubusercontent.com/6462456/171208438-9c16e743-c270-4a9b-a9c1-0f5e8d88d53f.png)

위의 명령어를 통해 설치된 Node.js와 NPM 버전을 각각 확인할 수 있습니다!  

### Node.js 맛보기 👅

```javascript
// hello.js
console.log('hello node.js!');
```

아주 간단한 내용의 `hello.js`를 작성해 볼까요?  

```bash
node hello.js
```

그 다음으로, 터미널에다가 `node` 명령어를 입력해
작성한 `hello.js`를 실행해 봅시다...  

![image](https://user-images.githubusercontent.com/6462456/171214590-94a01ccc-5101-4e83-93db-706bfa0acdec.png)

와우!!!!!!!!!!!!!!!!!!!!!  
Node.js가 반갑게 인사하네요~~~~  

![image](https://user-images.githubusercontent.com/6462456/171799765-2b02924d-2864-495c-9dda-2706011157fa.png)

### NPM 맛보기 👅

```bash
npm init -y
```

디렉토리를 하나 만들고 그 안에서 위 명령어를 입력해 볼까요?  
`npm init -y` 명령어를 통해 해당 Node.js 프로젝트에 대한
명세인 `package.json` 파일을 생성할 수 있어요.  

`package.json`에는 해당 프로젝트의 이름, 버전, 사용되는 모듈 등의
스펙이 정해져 있으며, 이 파일을 통해 모듈 의존성 모듈 관리도 진행할 수 있습니다!!  
만약 어떤 오픈 소스를 다운 받을 때 이 `package.json`만 있다면
해당 오픈 소스가 의존하고 있는 모듈이 어떤 것인지 알 수 있음과 동시에,
그 모듈들을 `npm install` 명령어로 한 번에 설치할 수 있기도 해요~~  

![i11308422346](https://user-images.githubusercontent.com/6462456/171799960-eb580751-a8a3-4e96-8c34-62947747cfc1.gif)

_NPM 짱!!_  

```json
// package.json
{
  "name": "week1",
  "version": "1.0.0",
  "description": "![image](https://user-images.githubusercontent.com/6462456/170818388-b71e1cb0-07c4-4ab5-821a-f2e965567f3d.png)   _1주차(5월 23일 ~ 5월 29일) : Webpack, Motivation, Node.js & NPM_",
  "main": "hello.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

생성된 `package.json` 파일은 위와 같이 생겼어요~!  
`package.json` 파일을 생성할 때 가장 주의해야 할 점은,
자바스크립트의 **객체 리터럴** 형식이 아닌,
올바른 `json` 파일의 형식을 따라 작성해 주어야 한다는 점이에요.  

![](https://mblogthumb-phinf.pstatic.net/MjAxOTAzMTNfMjgz/MDAxNTUyNDU5Njk4MTEx.R8OnbrGGUid8zGeW-Ucd0OropnCT22woVhZaGdQZNRwg.LE8GHOix_Qxsv1vO_KINM7wpMuutcdwbBWchdZkkYNYg.GIF.ebichu1030/에비츄.gif?type=w2)

그럼, 기본적으로 생성된 파일의 각 속성에 대해서 조금 더 자세히 알아볼까요...?  

---

> name : **해당 자바스크립트 패키지의 고유 이름**

- 반드시 214글자보다 짧은 이름으로 정할 것
- `.`이나 `_`로 시작하지 말 것
- 대문자를 포함하지 말 것
- URL-safe한 특성을 가질 것

`name` 항목에 작성되는 내용은 위의 특성을 지켜야 한다고 해요.  
또한 자바스크립트 파일 내에서 패키지가 사용되는 경우
`name`의 내용이 `require()` 함수의 파라미터로 사용되므로,
이를 고려하여 짧으면서도 간결한 네이밍으로 짓는 것을 권장한다고 합니다!  

> version : **해당 자바스크립트 패키지의 버전**

`name`과 `version`, 이 두 항목이
자바스크립트 패키지의 고유성을 판별하는 기준이기도 해요.  

> description : **해당 자바스크립트 패키지에 대한 설명**

`description`으로 작성한 패키지의 설명은 나중에
`npm search`를 통해 검색된 내용에서 다른 개발자들이
패키지를 찾아 내고 이해하는 데 도움을 주는 내용이에요.  

> main : **해당 자바스크립트 패키지의 시작점이 되는 모듈의 ID**

만약 `main` 항목에 대한 내용을 `poodle`라는 ID로 지정한 경우를
예시로 들어보면, `require('패키지명')`을 실행했을 때
`poodle` 모듈의 exports 객체가 반환된다고 해요.
참고로 모듈의 ID는 패키지의 root에 상대적인 경로로 지정한다고 합니다.  

> scripts : **해당 자바스크립트 패키지의 생명주기 중  
실행될 수 있는 script 명령들을 포함한 사전**

`scripts` 항목에 작성한 내용들 중, 키는 **이벤트**를,
그리고 값은 이벤트에 따라 **실행될 커맨드**를 지정해요.  

scripts 항목에 대해서는 조금 이따가
**NPM Custom Commmand**와 연결지어 좀 더 자세히 살펴보겠습니다!!  

> keyword : **해당 자바스크립트 패키지를 설명하는 키워드 배열**

`keyword` 항목에 작성한 내용들은 다른 개발자들이
`npm search`를 통해 해당 패키지를 검색할 때
`description`과 더불어 패키지에 대한 설명 및 이해를 도와 주게 됩니다!  

> author : **해당 자바스크립트 패키지를 작성한 사람**

기본 커맨드에 의해 생성된 내용으로는 `author` 필드가 포함되었지만,
보통 한 사람만을 나타낼 때는 `author`를,
여러 사람들을 나타내고자 할 때는 `contributors` 필드를 사용한다고 합니다~  

세부 항목으로는 `name`을 필수적으로 가지며
`email`과 `url`을 선택적으로 작성할 수 있다고 해요.  

> license : **해당 자바스크립트 패키지에 대한 라이센스 명시**

`license` 항목은 해당 패키지를 사용하기 위해 어떠한 권한들을 필요로 하는지,
어떠한 금기 사항이 있는지 등을 설명하기 위해 존재해요.  

```json
"license": "BSD-3-Clause"       // 특정 라이센스 명시
"license" : "(ISC OR GPL-3.0)"  // 여러 라이센스 하에 있는 경우
"license": "UNLICENSED"         // 비공개 혹은 퍼블리싱하지 않는 경우
```

지정할 수 있는 라이센스 종류에 대해서는
[이 링크](https://spdx.org/licenses/)를 참고할 수 있답니다~!  

---

이상으로 `package.json`을 구성하는 항목들에 대해서 약간은 깊게
다루고 넘어왔어요~  

- **scripts**
- **dependencies**
- **devDependencies**

이 중에서, 특히 패키지의 의존성 등과 관련이 있어 좀 더 자세히
살펴보고자 하는 속성은 위의 3가지입니다!
이 내용들에 대해서는 아래에서 좀 더 자세히 다루어 보기로 해요~!  

추가로, 위에서는 기본적으로 생성된 `package.json`에 존재하는 항목들에
대해서만 짚고 넘어가 봤지만, 실제로 패키지에 대해서 작성할 수 있는
훨씬 많은 항목들이 존재해요.  
만약 궁금하다면,
[이 블로그](https://programmingsummaries.tistory.com/385)를
참고하면 도움이 될 것 같습니다...!  

> npm init **-y** 옵션의 의미...?

위에서 `package.json` 파일을 생성하기 위해 사용한 `npm init`명령어에서
`-y` 옵션을 줬었는데요, 만약 `-y` 옵션을 사용하지 않는다면 어떻게 될까요?

![image](https://user-images.githubusercontent.com/6462456/171528283-11a64a2f-12ec-4f33-a84e-6242f65ac4a0.png)

옵션을 주지 않고 `npm init` 명렁어만을 사용한다면,
위처럼 대화형 환경을 통해 순차적으로 각 항목에 대한 내용을 입력함으로써
`package.json`을 생성하게 되는 것을 알 수 있습니다.  

이렇게 비교해 보니, `-y` 옵션은 대화형 환경을 생략하고
미리 정해진 default 값을 통해 `package.json`을 생성하도록
하는 옵션임을 확인할 수 있겠네요..!  
참고로 이 때 `-y`의 의미는 **yes**라고 합니다!  

![yes](https://mblogthumb-phinf.pstatic.net/MjAxOTAzMTNfMTI3/MDAxNTUyNDYyMDg3NjUy.gr3DWpe4PqPu3MbeLaRTYIscQvdloudFQQ7LHY9bsTcg.MK38OhFmfup35U49v7OsUkGC061_EqnDWAlBk8Qbc9Yg.GIF.ebichu1030/4412.gif?type=w2)

# NPM Module Install

> `npm install <패키지 이름>` : 자바스크립트 패키지 설치 명령어

NPM을 통해 프로젝트에서 사용하고자 하는 자바스크립트 패키지를 설치하려면
위와 같은 명령어를 통해 수행할 수 있어요!  
예시로, `jquery` 패키지를 설치해 보도록 할게요!!  

## NPM을 통한 패키지 지역 설치

```bash
npm install jquery --save-prod
npm install jquery      # --save-prod는 생략해도 기본 지정
npm i jquery            # install과 같은 옵션
```

위의 명령어를 실행함으로써 `jquery` 패키지를 지역적으로 설치할 수 있어요!!  
`--save-prod` 옵션 같은 경우는 `package.json` 내의
`dependencies` 항목에 패키지가 추가되도록 하는 옵션인데,
이는 default 값이기 때문에 생략하더라도 지정되는 값입니다!!  
마찬가지로, `install` 또한 `i`로 줄여서 사용하더라도 같은 의미에요!  

![image](https://user-images.githubusercontent.com/6462456/171554996-758da5f2-88c8-4177-84a7-69296a8605ae.png)

기본적으로 위의 명령어를 사용한 자바스크립트 패키지 설치는
해당 프로젝트에 지역적인 설치를 수행해요.  
프로젝트 디렉토리를 열어 보면 `node_modules` 디렉토리 내에
조금 전 설치한 `jquery` 패키지가 생성되어 있는 것을 확인할 수 있는데요,
이를 통해 `node_modules` 디렉토리가 패키지들을 포함하는 장소라는
것을 확인할 수 있었네요~!  

## NPM을 통한 패키지 전역 설치

```bash
npm install gulp --global
npm install gulp -g     # --global과 같은 옵션
```

`npm install ...` 명령어를 통해 지역적으로 패키지를 설치할 수도 있지만,
보다 범용적으로 쓰이는 패키지라면 모든 프로젝트마다 설치하기보다
전역적인 설치를 수행하는 게 효율적인 방법일 수 있어요.  
이 때는 `--global` 또는 `-g` 옵션을 추가함으로써 전역 설치를 수행합시다!!  

```text
# window
%USERPROFILE%\AppData\Roaming\npm\node_modules

# mac
/usr/local/lib/node_modules
```

`--global` 옵션을 통해 전역 설치를 수행한 패키지가 위치하는 장소는
위처럼 OS마다 다르답니다!!  

![image](https://user-images.githubusercontent.com/6462456/171555710-1b174258-e58f-4104-b1c2-9972f67edc8d.png)

실제로 `--global` 옵션을 통해 전역 설치를 수행한 후
Mac OS 기준 디렉토리를 찾아가 보니 설치한 `gulp` 패키지가
위치하는 것을 확인할 수 있었네요...  

추가로, 위처럼 전역 설치를 수행하는 경우는 OS에 따라
**permission issue**가 발생할 수 있으니
`sudo`를 통해 설치하시기를 권장드립니다!  

## NPM 지역 설치 패키지의 의존성 관리

위에서 알아본 내용을 통해, NPM으로 지역 설치한 패키지들은
프로젝트 내 `node_modules` 디렉토리에 위치하게 돼요.  

게다가 추가로 `package.json` 파일 내에
설치한 패키지에 대한 내용이 기록된다는 걸 알 수 있었습니다.  
그 이유는, 프로젝트를 빌드하는 등의 상황에 있어서
설치한 패키지들에 대한 의존성을 관리해 주기 위해서입니다.  
(공식 문서에서는 이러한 과정을 **save**한다고 표현했어요.)  

1. npm install jquery **--save-prod**
2. npm install jquery **--save-dev**
3. npm install jquery --save-optional
4. npm install jquery --no-save

`npm install ...` 명령어를 통해 패키지를 설치하는 경우
의존성 관리에 대해 명시해줄 수 있는 옵션은 총 **4가지**가 있어요.  

> --save-prod

```bash
npm install jquery --save-prod
npm install jquery -P   # 위와 같은 명령어
npm install jquery      # 위와 같은 명령어
```

아까 사용한 지역 설치 명령어 예시를 살펴보면,
`--save-prod` 옵션을 사용해서 설치했으며 이 옵션은 default 값으로
지정되어 있기 때문에 생략해도 같은 동작을 수행한다고 배웠어요.  

```json
// package.json
{
  "dependencies": {
    "jquery": "^3.6.0"
  }
}
```

위 명령어에 의해 `package.json`에 변화된 내용을 살펴보면,
`dependencies` 항목 아래에 추가한 `jquery` 패키지에 대한
의존성이 기록되어 있는 것을 알 수 있어요.  

> --save-dev

```bash
npm install jquery --save-dev
npm install jquery -D   # 위와 같은 명령어
```

`--save-dev` 옵션을 주고 설치한 경우는
`--save-prod`의 경우와 어떠한 내용이 다를까요??  

```json
// package.json
{
  "devDependencies": {
    "jquery": "^3.6.0"
  }
}
```

`package.json`을 살펴보면, 아까와는 다르게
`devDependencies`라는 항목에 패키지가 들어가 있네요...!!  

> `dependencies` vs `devDependencies`?

그냥 `dependencies`와 `devDependencies` 항목이 가지는 차이점은
**개발용 라이브러리**와 **배포용 라이브러리**를 구분하는 데 있어요.  

만약, `jquery`처럼 화면 로직과 직접적으로 관련된 라이브러리는
배포용 라이브러리로 구분하고 `--save-prod` 옵션으로 설치해야 합니다.  
이 경우 패키지는 `npm run build`로 빌드 시
최종 애플리케이션 코드 안에 포함됩니다.  

반대로, 개발할 때만 사용하고 배포할 때는 빠져도 좋은 패키지의 경우는
`--save-dev` 옵션과 함께 설치함으로써
배포 시 애플리케이션 코드에서 제외하도록 해 코드 최적화를 수행할 수 있어요.  
이 경우 애플리케이션에 포함되어야 하는 패키지를 개발용 라이브러리로
잘못 구분하지 않도록 주의를 기울이는 것이 중요하겠죠....!

- `webpack` : 빌드 도구
- `eslint` : 코드 문법 검사 도구
- `imagemin` : 이미지 압축 도구

개발할 때만 사용하고 배포할 때는 빠져도 좋은 라이브러리의 예시는 위와 같습니다!  

> --save-optional, 그리고 --no-save

```bash
npm install jquery --save-optional
npm install jquery -O   # 위와 같은 명령어
```

```json
// package.json
{
  "optionalDependencies": {
    "jquery": "^3.6.0"
  }
}
```

`--save-optional` 항목과 함께 설치한 경우는
`package.json` 내의 `optionalDependencies` 항목에
패키지의 의존성이 추가됩니다.  

`optionalDependencies` 항목에 의존성을 추가하는 경우는
`npm install ...` 명령어를 통해 꼭 설치되지 않아도
상관없는 경우에 대해서 관리하는 경우에요.  
이는 추후 의존성에 명시된 패키지들을 설치할 때
`--no-optional` 옵션을 함께 사용함으로써 적용할 수 있답니다!!  

```bash
npm install jquery --no-save
```

`--no-save` 옵션과 함께 설치하면 패키지는
`node_modules` 디렉토리 내에 설치되지만,
`package.json` 내에 따로 의존성이 추가되지는 않아요.  

만약 해당 프로젝트에 대해서 특정 패키지를 적용해보거나
테스트를 진행해보고 싶긴 한데, 패키지의 의존성을 추가하기는 싫은 경우에
`--no-save` 옵션을 통해 진행할 수 있겠네요...!!  

> 다른 옵션들?

사실 `package.json` 내에 추가할 수 있는 의존성의 종류에는
`peerDependencies`나 `bundledDependencies` 등
더 많은 방법들이 존재해요.
하지만 첫 만남에 너무 깊게 파고들면 다음부터 흥미가 떨어질 수 있으니까요...!
오늘은 가장 중요한 `dependencies`와 `devDependencies`에
대해서만 짚고 넘어가 보자구요!!  

만약 이러한 방법들에 대해서 더 자세히 알고 싶다면,
[여기](https://betterprogramming.pub/what-are-npms-optional-dependencies-and-when-should-we-use-them-796a6a964e73)에 자세하게 설명되어 있으니 참고하시면 좋겠습니다~~~  

> 패키지 버전과 Semver 버전 규칙

![sementic-versioning](https://user-images.githubusercontent.com/6462456/171792883-3ccd2e45-def4-4d1f-9122-f740ec5df874.png)

기본적으로 자바스크립트 패키지의 `version` 항목의 값은
**[major, minor, patch]** 의 규칙을 따릅니다.  
또한 `package.json`에 버전을 명시할 때에는 Semantic Versioning,
즉 **유의적 버전**이라고 부르는 **Semver**이라는 규칙을 사용해
명시할 수 있는데요, 그 규칙들은 아래와 같습니다.

- **^** : 가장 왼쪽의 major 버전을 변경하지 않은 minor 이하의
업데이트는 허용
- **~** : major과 minor 버전을 변경하지 않은 patch 이하의
업데이트는 허용
- **>**, **>=** : 현재 명시된 버전보다 상위 버전 허용
- **<**, **<=** : 현재 명시된 버전보다 상위 버전 허용
- **=** : 정확하게 해당 버전만을 사용하도록 명시

위와 같이 Semver 연산자에 의해 정해진 규칙대로
`npm update` 명령어에 따른 패키지 버전 업데이트가 이루어진다고
보시면 될 것 같아요~~~  

보다 자세한 규칙들이 궁금하시다면
[여기](https://nodejs.dev/learn/semantic-versioning-using-npm)를 참고하시면 도움이 될 듯 합니다!!  

> package.json, 그리고 package-lock.json...?

프로젝트 디렉토리를 한 번 살펴보면 위에서 설명한 `package.json` 파일 외에
`package-lock.json` 파일이 생성되어 있는 것을 볼 수 있는데요,
사실은 그 동안 사이드 프로젝트를 하면서 많이 봐 왔던 파일일 것입니다..!  
내가 만든 기억이 없는데,,, 이 친구는 도대체 어디서 튀어 나온 걸까요..?  

```json
// package.json
{
...
  "dependencies": {
    "jquery": "^3.6.0"
  }
...
}
```

```json
// package-lock.json
{
...
      "dependencies": {
        "jquery": "^3.6.0"
      }
    },
    "node_modules/jquery": {
      "version": "3.6.0",
      "resolved": "https://registry.npmjs.org/jquery/-/jquery-3.6.0.tgz",
      "integrity": "sha512-JVzAR/AjBvVt2BmYhxRCSYysDsPcssdmTFnzyLEts9qNwmjmu4JTAMYubEfwVOSwpQ1I1sKKFcxhZCI2buerfw=="
    }
  },
  "dependencies": {
    "jquery": {
      "version": "3.6.0",
      "resolved": "https://registry.npmjs.org/jquery/-/jquery-3.6.0.tgz",
      "integrity": "sha512-JVzAR/AjBvVt2BmYhxRCSYysDsPcssdmTFnzyLEts9qNwmjmu4JTAMYubEfwVOSwpQ1I1sKKFcxhZCI2buerfw=="
    }
  }
}
...
```

위는 `npm install jquery`를 수행하고 난 후의
`package.json`과 `package-lock.json`을 비교해 본 내용이에요!!  
비슷하게 패키지 의존성에 대한 내용을 다루지만 `package-lock.json`이
훨씬 구체적인 정보들을 함께 보관하고 있음을 확인할 수 있네요...!  

`package-lock.json`을 사용하는 이유는,
`package.json`의 정보가 부족한 상황이 존재하기 때문입니다.  
`package.json`에서 버전을 지정할 때 사용하는
version range(~0.0.1)의 형태가 협업 상태의 여러 사람 간의
로컬 개발환경 구성에서 문제를 일으킬 수 있기 때문에,
이를 `node_modules` 구조나 `package.json`이 수정되고 생성될 때
당시의 의존성에 대한 정확하고 구체적인 정보를 품고
자동으로 생성되도록 함으로써 해결하고자 한 것이죠.  

```bash
npm install     # package.json을 이용한 패키지 환경 구성
```

`npm install` 명령어를 통해 패키지 설치 환경을 구성하는 경우 또한
`package-lock.json`이 존재하는 경우 `package.json`에
우선적으로 고려되어 이를 이용한다고 합니다...!  

> NVM?

[nvm 과 npm 구별하기](https://lynmp.com/ko/article/tb585d114096490055)

위에서 다룬 **NPM**과 비슷한 이름으로 헷갈릴 수 있는 내용이라서
짧게 짚고 넘어가면 **NVM**은 **Node Version Manager**로,
한 마디로 정리해 **Node.js 의 여러 버젼을
마음대로 골라 설치할 수 있게 해 주는 프로그램** 입니다.  

개발을 하다 보면 Node.js 의 각 버젼을 유지하면서 시스템을 구성해야 하는
경우가 존재하는데, 이 때 같은 시스템 안에서 여러 Node.js 를 사용하기 위해
버젼별로 Node.js 환경을 격리시키는 역할을 수행한다고 할 수 있습니다!!  
마치 파이썬의 **Anaconda**처럼 생각하면 될까요...?  

- **nvm ➝ Node.js ➝ npm (권장)**
- apt or yum or brew ➝ Node.js ➝ npm (가능은 하나, 비추천)
- Node.js ➝ nvm (ㄴㄴ)

npm과의 관계를 고려해서 가장 이상적인 개발 환경을 설정하는 경우를 생각한다면,
위처럼 nvm을 통해 Node.js를 설치하고, 그 안에서
npm으로 패키지를 관리하는 경우를 생각해볼 수 있겠네요...!  

# NPM Custom Command

NPM Custom Command란, 사용자가 임의로 명령어의 이름과 동작을 정의해서
사용할 수 있는 기능으로, `package.json` 내에 명시된
`scripts` 부분의 내용을 의미합니다!!  

```json
{
...
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
  },
...
}
```

위에서 생성한 `package.json`의 `scripts` 내용입니다.  
하위 항목으로 `test` 라는 명령어와 이에 따른 커맨드라인 동작을 가지고 있네요.  

```json
{
...
    "scripts": {
        "hello": "echo Hello NPM Custom Command!!"
  },
...
}
```

이 내용을 위의 내용으로 바꿔 볼까요??  
그리고 쉘에서 `npm run hello`를 실행해 봅시다.  

![image](https://user-images.githubusercontent.com/6462456/171797210-8fbeb8dd-1f71-4d69-a082-6277cd135f15.png)

![](https://mblogthumb-phinf.pstatic.net/MjAxOTAzMTNfMjc5/MDAxNTUyNDYyNTIzODI3.ta6z8HEOo6Dto2trCFhs1Mca8OryH_gi8etuBZrjqN0g.0wtnEmzCnILGnQw-HSDHAiNaiSFVAKAA9RHEz0qKyIQg.GIF.ebichu1030/1624bd88996488156.gif?type=w2)

와우 너무 신기해!!!!!!!!!!!  
`scripts` 항목에 채운 내용들을 기반으로
`npm run 명령어 이름` 형식으로 Custom Command를
실행할 수 있다는 것을 확인했네요~~~~~~~~  

```json
"scripts": {
  "dev": "node server.js",
  "build": "webpack --mode=none",
}
```

Custom Command를 실제로 사용한 사례를 확인해 볼까요?  
위 코드는 서버를 실행하는 `dev` 커스텀 명령어와
Webpack으로 빌드하는 `build` 커스텀 명령어를 정의한 코드입니다.
사용자는 매번 `node server.js`와 `webpack --mode=none`를
칠 필요 없이 `npm run dev`와 `npm run build`를 입력하는 것만으로
원하는 동작을 빠르게 수행할 수 있게 됐어요~!  

```json
"scripts": {
  "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
}
```

만약 위처럼 실행하려는 명령어가 길수록
더 편리하게 사용할 수 있는 기능이겠네요~~  

```json
"scripts": {
  "build": "webpack",
  "deploy": "npm run build -- --mode production"
}
```

NPM Custom Command의 더욱 강력한 기능은
위처럼 명령어의 수행 부분에 다른 Custom Command를 가져와
조합해서 구성할 수 있다는 점입니다!!  

이 경우 `npm run deploy`를 통해 지정한 명령을 수행한다면,
먼저 `build`에 정의한 `webpack` 명령어가 실행되면서
명령어 뒤쪽에 붙은 실행 옵션들이 수행됩니다.  
이후 `webpack`이라는 도구의 mode에 production이라는 값을
넘겨주는 동작을 연결되어 수행하게 되는 것입니다...!  

![](https://mblogthumb-phinf.pstatic.net/MjAxOTAzMTNfMTQz/MDAxNTUyNDYzNzY1OTEw.tBzt8x1ENOvnhnM8YeQ-zt7eNz6E0u1fYevfBYS7WKYg.fsZavB-44AJbsjW4wyneKgiYZuJeekNCSpjsTdLV7XMg.JPEG.ebichu1030/おるちゅばんエビちゅ_第19話_%28DVD_x264_1024x768%29-かみひこうき.avi_000358848.jpg?type=w2)

_WOW..._  

# Webpack 1주차 내용을 정리하며...

이상으로 Webpack이 뭔지, Webpack을 통해 어떠한 문제를 해결하고자 하는지,
그리고 Node.js와 NPM, package.json을 구성하는 내용들에 대해서
간단하면서 자세하게 살펴봤는데요,
사실 너무 Node.js 이후의 내용들을 지나치게 무겁게 다룬 것 같은
느낌을 떨쳐버릴 수 없습니다.....
그래도 자세하게 알고 넘어간다는 게 나쁠 건 없으니까요 뭐~~  

![](https://user-images.githubusercontent.com/6462456/171801019-23b5b3e7-293b-4faf-8529-4af92f67269b.gif)

다음 2주차 내용 때 만나요~~~

# 위에서 언급했던 제가 방문했던 링크들..!

- https://babeljs.io/docs/en/learn#modules
- https://medium.com/@syalot005006/브라우저-http-최대-연결수-알아보기-3f7aa1453bc2  
- https://webpack.js.org/guides/code-splitting/
- https://ingg.dev/webpack/
- https://programmingsummaries.tistory.com/385
- https://betterprogramming.pub/what-are-npms-optional-dependencies-and-when-should-we-use-them-796a6a964e73
- https://nodejs.dev/learn/semantic-versioning-using-npm

# (추가) 자주 사용하는 NPM 명령어 정리

## 1. package.json 생성
```bash
npm init
npm init -y   # default 설정을 따라서 대화형 환경 없이 생성
```

## 2. 패키지 설치
```bash
# 로컬 설치
npm install <package-name>
# 전역 설치
npm install -g <package-name>
# 개발 설치
npm install --save-dev <package-name>
# package.json의 모든 패키지 설치
npm install
```

## 3. 패키지 제거
```bash
# 로컬/개발 패키지 제거
npm uninstall <package-name>
# 전역 패키지 제거
npm uninstall -g <package-name>
```

## 4. 패키지 업데이트
```bash
npm update <package-name>
```

## 5. 전역 설치된 패키지 목록 확인
```bash
npm ls -g --depth=0
```

## 6. Custom Command 실행
```bash
npm start
# start를 제외한 scripts 구성 항목의 실행 명령
npm run <script-name>
```

## 7. 전역 패키지 설치 위치 확인
```bash
npm root -g
```

## 8. 패키지 정보 참조
```bash
npm view <package-name>
```

## 9. NPM 버전 확인
```bash
npm -v
npm --version
```

## 10. NPM 명령어 설명 참조
```bash
npm help <command>
```