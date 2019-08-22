# lotteryScratch.js

> lotteryScratch.js는 HTML5 canvas를 사용하여 제작되었다. 마치 복권을 긁는 듯한 이벤트를 처리해주는 자바스크립트 라이브러리 이며, 브라우저에서 각종 이벤트 페이지 제작에 사용될 수 있다.

[Demo   : https://ryujun.github.io/demos/JavaScript/lotteryScratch/](https://ryujun.github.io/demos/JavaScript/lotteryScratch/)<br>
[Github : https://github.com/RyuJun/lotteryScratch](https://github.com/RyuJun/lotteryScratch)

## 지원
> chrome, firefox, while 등등의 모던브라우저 및 ie9 버전 이상에서 지원함

## 설치 및 사용방법

### 일반 웹 페이지

웹 페이지에서 사용하려면 html파일 상단에 lotteryScratch.js 파일을 `<script>`태그를 이용하여 삽입.
```html
<script src="lotteryScratch.js" type="text/javascript"></script>

```
자바스크립트 코드에서 생성자 new lotteryScratch( ... )를 사용하여 접근할 수 있다.
```js
var lotteryScratchGo1 = new lotteryScratch(
  document.querySelector('#lotteryScratch1'), // Element
  492, // width
  259, // height
  20, // 복권 긁는 circle의 size
  './images/img_cont02.jpg', // 당첨내용을 덮을 (복권이 긁힐) 이미지 주소
);
```

### Example 1
`'''.html` 파일 상단에 lotteryScratch.js 가 아래와 같이 추가 되었다면,
element 하단에 js코드를 작성해준다.
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>lotteryScratch</title>
  <script src="./js/lotteryScratch.js"></script> <!-- 추가 -->

</head>

<body>
  <span>lotteryScratch1</span>
  <div id="lotteryScratch1"></div>
  <script>
   // 코드 작성 
  </script>
</body>

</html>
```
하단에 작성하기 싫다면 `window.load` 이벤트를 EventListener에 등록하여 사용하여도 무관하다.
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>lotteryScratch</title>
  <script src="./js/lotteryScratch.js"></script>
  <script>
    window.addEventListener('load', function () {
      // 코드작성
    });
  </script>
</head>

<body>
  <span>lotteryScratch1</span>
  <div id="lotteryScratch1"></div>
</body>

</html>
```
아래와 같이 js 파일을 작성하여 동작시켜본다. 각 함수의 자세한 명세는
다음 카테고리인 명세항목 에서 확인 할 수 있다.
```js
var lotteryScratchGo1 = new lotteryScratch(
  document.querySelector('#lotteryScratch1'), 492, 259, 20,
  './images/img_cont02.jpg',
);

lotteryScratchGo1
  .LotteryScratchInit()
  .LotteryScratchCallback(function () { 
    // callback 함수 LotteryScratch가 실행 된 후 실행할 코드 작성
    // 이곳에서 당첨이미지?를 아래 background로 깔아준다.
    document.querySelector('#lotteryScratch1').style.backgroundImage = "url('./images/20190719_112814.png')";
  }).LotteryScratch();
```

### Example 2
예시 1과 마찬가지로 `lotteryScratch.js`를 삽입 시켜준 뒤, <br>
아래와 같이 js 파일을 작성하여 동작시켜본다. 각 함수의 자세한 명세는
다음 카테고리인 명세항목 에서 확인 할 수 있다.
```js
var lotteryScratchGo2 = new lotteryScratch(
  document.querySelector('#lotteryScratch2'), 492, 259, 25,
  './images/img_cont02.jpg',
);

lotteryScratchGo2.LotteryScratchInit();

document.querySelector('#lotteryScratch2').onclick = function () {
  // 동작 버튼을 클릭했을 시에만 동작하게 하기위한 방어코드
  lotteryScratchGo2.LotteryScratchReadyAlert('lotteryScratch2 start or reset 버튼을 클릭한 뒤 진행해주세요!');
}

document.querySelector('#resetButton').onclick = function () {
  // 버튼을 클릭해야만 lotteryScratch가 실행된다.
  lotteryScratchGo2
    .LotteryScratchClear() // 초기화
    .LotteryScratchCallback(function () { // register callback Function
      document.querySelector('#lotteryScratch2').style.backgroundImage = "url('./images/20190719_114105.png')"; // background Img
    }).LotteryScratch(); // lotteryScratch Start
}
```

## 명세 ( lotteryScratch.propertys )
생성자를 통하여 변수에 lotteryScratch를 담았다면 아래와 같은 property Function에 접근이 가능하다.
함수 이름 맨 앞에 _가 붙는다면 내부에서 사용되는 함수이다.

### lotteryScratch._checkSupportUserAgent()
`_checkSupportUserAgent()` : user가 접속한 기기가 pc인지 mobile인지 판단해준다. pc면 false, mobile이면 true

```js
_checkSupportUserAgent: function () {
  var filter = 'win16|win32|win64|mac|macintel';
  if (navigator.platform) return filter.indexOf(navigator.platform.toLowerCase()) < 0 ? true : false;
},
```

### lotteryScratch._checkSupportLotteryScratch()
`_checkSupportLotteryScratch()` : Element에는 id가 필수로 존재하여야 한다. id Attribute를 찾고 없다면 false값을 리턴 시킨다. 
```js
_checkSupportLotteryScratch: function () {
  return (function (element) {
    return element.getAttribute('id') ? true : false;
  })(this.lotteryScratchElement);
},
```

### lotteryScratch._cutCircle()
`_cutCircle()` : canvas에 마우스 드래그 evnet발생시 실행되는 함수 circle형태로 이미지를 지워나간다. 
* @param context : cavans.getContext(2d); 값. 
* @param pointX : 현재 마우스의 X 좌표 값. 
* @param pointY : 현재 마우스의 Y 좌표 값. 
* @param radius : 원의 크기 값 (클수록 크게 지워짐). 

```js
_cutCircle: function (context, pointX, pointY, radius) {
  context.globalCompositeOperation = 'destination-out';
  context.beginPath();
  context.arc(pointX, pointY, radius, 0, Math.PI * 2, true);
  context.fill();
},
```

### lotteryScratch.LotteryScratchReadyAlert()
`LotteryScratchReadyAlert()` : 바로 canvas를 드래그하는 경우를 막을 때 사용 <br>ex) 긁기 start버튼을 클릭하지 않고 바로 드래그 시 alert뿜음 
* @param message : alert창에 입력될 message 

```js
LotteryScratchReadyAlert: function (message) {
  if (!this.lotteryScratchStatus) alert(message);
},
```

### lotteryScratch.LotteryScratchClear()
`LotteryScratchClear()` : LotteryScratch의 리셋을 위한 함수<br>
ex) 버튼을 누를때마다 해당 함수를 호출하면 현재 진행중인 canvas를 리셋 시킴 

```js
LotteryScratchClear: function () {
  this.lotteryScratchElement.parentNode.style = 'none';
  this.lotteryScratchElement = this.lotteryScratchElement.parentNode;
  this.lotteryScratchElement.removeChild(this.lotteryScratchElement.childNodes[0]);
  this.LotteryScratchInit();
  return this;
},
```

### lotteryScratch.LotteryScratchInit()
`LotteryScratchInit()` : 기본 크기 및 canvas Element를 생성함 Canvas를 imgCnavas로 변경 

```js
LotteryScratchInit: function () {
  if (this._checkSupportLotteryScratch) {
    var canvas = document.createElement('canvas'); // canvas init 
    var imageContext = canvas.getContext('2d'); // canvas.getContext('2d') for image 
    var topImage = new Image(); // new image 
    this.lotteryScratchElement.style.width = this.lotteryScratchWidth + "px";
    this.lotteryScratchElement.style.height = this.lotteryScratchHeight + "px";
    this.lotteryScratchElement.onmouseover = function () { canvas.style.cursor = 'crosshair' };
    canvas.width = this.lotteryScratchWidth; // canvas image width 
    canvas.height = this.lotteryScratchHeight; // canvas image height 
    this.lotteryScratchElement.appendChild(canvas); // canvas append 
    this.lotteryScratchElement = canvas;
    topImage.onload = function () {
      imageContext.drawImage(topImage, 0, 0);
    };
    topImage.src = this.lotteryScratchTopImageSrc;
  }
  return this;
},
```

### lotteryScratch.LotteryScratch()
`LotteryScratchClear()` : 실질적으로 lotteryScratch가 동작하는 코드 해당 Element의 id가 존재할 때 넘어온 param값에 따라 cavnas image 만들기 및 값을 세팅해준다. 
* 마우스 down, up, move 이벤트를 통해 드래그와 같은 동작 event가 발생하며 
* _cutCircle 함수를 호출하여 이미지를 삭제시킨다.
* _checkSupportUserAgent() 함수를 통해 모바일 인지를 체크하며,
* mobile에서는 `mousedown`, `mousemove`, `mouseup` event가 <br>
`touchstart`, `touchmove`, `touchend` event로 변경된다.
* 또한 `e.pageX||Y` 를 통하여 얻어오던 좌표값은<br>
`e.changedTouches[0].clientX||Y`를 통해 얻어온다.
* `e.changedTouches[0]`가 배열인 이유는 멀티터치를 지원하기위한것으로 확인된다.

```js
LotteryScratch: function () {
  if (this._checkSupportLotteryScratch) {
    if (!this._checkSupportUserAgent()) { // pc일 경우
      this.lotteryScratchElement.onmousedown = (function (e) {
        var canvas = this.lotteryScratchElement;
        var radius = this.circleRadius;
        var cutCircle = this._cutCircle;
        this.callback !== null ? this.callback() : null;
        this.callback = null;
        var deleteMoveAction = function (e) {
          var rect = canvas.getBoundingClientRect(),
            context = canvas.getContext("2d"),
            scrollPosition = window.scrollY || document.documentElement.scrollTop,
            pointX = Math.round(e.pageX - rect.left),
            pointY = Math.round(e.pageY - rect.top - scrollPosition);
          cutCircle(context, pointX, pointY, radius);
        }
        document.addEventListener('mousemove', deleteMoveAction);
        document.onmouseup = function () {
          document.removeEventListener('mousemove', deleteMoveAction);
        };
      }).bind(this);
    } else { // mobile일 경우
      this.lotteryScratchElement.ontouchstart = (function (e) {
        e.preventDefault();
        var canvas = this.lotteryScratchElement;
        var radius = this.circleRadius;
        var cutCircle = this._cutCircle;
        this.callback !== null ? this.callback() : null;
        this.callback = null;
        var deleteMoveAction = function (e) {
          var rect = canvas.getBoundingClientRect(),
            context = canvas.getContext("2d"),
            scrollPosition = window.scrollY || document.documentElement.scrollTop,
            pointX = Math.round(e.changedTouches[0].clientX - rect.left),
            pointY = Math.round(e.changedTouches[0].clientY - rect.top - scrollPosition);
          cutCircle(context, pointX, pointY, radius);
        }
        document.addEventListener('touchmove', deleteMoveAction);
        document.ontouchend = function () {
          document.removeEventListener('touchmove', deleteMoveAction);
        };
      }).bind(this);
    }
    this.lotteryScratchStatus = true;
  }
  return this;
},
```

### lotteryScratch.LotteryScratchCallback()
`LotteryScratchCallback()` : callback으로 들어온 함수를 this.callback에 저장 한 뒤,<br> LotteryScratch 함수에서 실행 

```js
LotteryScratchCallback: function (callbackFunction) {
  this.callback = callbackFunction;
  return this;
}
```