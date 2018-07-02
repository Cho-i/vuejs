**뷰 템플릿 (Template)**

- ( html, css ) + 뷰 인스턴스의 데이터와 로직 => 브라우저의 html
- 가상 돔 기반의 render() 함수로 변환 - 화면을 그리는 역할
  - 변환 과정에서 뷰의 반응성(Reactivity) 추가

```html
<!-- template 속성을 사용하지 않은 경우 -->
<div id=“app”>
    <h3>{{ message }}</h3>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        }
    });
</script>

<!-- template 속성을 사용한 경우 -->
<div id=“app”>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        template: '<h3>{{ message }}</h3>'
    });
</script>

<!-- ES6 : 싱글 파일 컴포넌트 체계 -->
<template>
    <p>Hello {{ message }}</p>
</template>
```





**뷰의 속성과 문법**

```
- 데이터 바인딩
- 자바스크립트 표현식
- 디렉티브
- 이벤트 처리
- 고급 템플릿 기법
```





**데이터 바인딩 (Data Binding)**

- html 화면 요소 + 뷰 인스턴스의 데이터
- 문법 1 : {{  }} - 콧수염 괄호 (중괄호)
- 문법 2 : v-bind





**{{  }} - 콧수염 괄호 (중괄호)**

```html
<!-- {{  }} 를 이용한 데이터 바인딩 -->
<div id="app">
    {{ message }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        }
    });
</script>

<!-- v-once 속성을 이용한 1회 바인딩 -->
<div id="app">
    {{ message }}
</div>
```



**v-bind**

- id, class, style 등의 html 속성 값에 데이터 바인딩 할 때 사용

```html
<!-- v-bind 예제 -->
<div id="app">
    <p v-bind:id="idA">아이디 바인딩</p>
    <p v-bind:class="classA">클래스 바인딩</p>
    <p v-bind:style="styleA">스타일 바인딩 바인딩</p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            idA: 10,
            classA: 'container',
            styleA: 'color: blue'
        }
    });
</script>
```

- v-bind: 는 : 로 간소화 할 수 있으나, 혼용해서 사용하지 않는 것이 좋으며, 전반적인 통일성을 위해 v-bind: 로 사용하기를 지양



**자바스크립트 표현식**

- {{  }} 안에 자바스크립트 표현식 입력 가능

```html
<!-- 자바스크립트 표현식 예제 -->
<div id="app">
    <p>{{ message }}</p>
    <p>{{ message + "!!!" }}</p>
    <p>{{ message.split('').reverse().join('') }}</p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        }
    });
</script>
```



- 주의할 점
  - 선언문과 분기 구문 사용 불가
  - 복잡한 연산은 인스턴스에서 처리하고 화면에는 간단한 연산 결과만 표시
    - 반복적인 연산에 대해 캐싱 효과를 얻을 수 있음

```html
<!-- 자바스크립트 표현식 사용시 주의할 점 -->
<div id="app">
    {{ var a = 10; }} <!-- X, 선언문은 사용 불가능 -->
    {{ if (true) {return 100;} }} <!-- X, 분기문은 사용 불가능 -->
    {{ true ? 100 : 0 }} <!-- O, 삼항 연산자로 표현 가능 -->

    {{ message.split('').reverse().join('') }} <!-- X, 복잡한 연산은 인스턴스 안에서 수행 -->
    {{ reversedMessage }} <!-- O, 스크립트에서 computed 속성으로 계산한 후 최종 값만 표현 -->
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        computed: { // 데이터 속성을 자동으로 계산해 주는 속성
            reversedMessage: function() { // {{ reversedMessage }}에 표현되기 전에 연산을 수행하는 함수
                return this.message.split('').reverse().join('');
            }
        }
    });
</script>
```





**디렉티브**

- v- 접두사를 가지는 모든 속성을 의미

```html
<!-- 디렉티브 형식 -->
<div id="app">
    <a v-if="flag">두잇 Vue.js</a>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            flag: true
            // flag: false
        }
    });
</script>
```



| 디렉티브 이름 | 역할                                                         |
| ------------- | ------------------------------------------------------------ |
| v-if          | 지정한 뷰 데이터 값의 참, 거짓 여부에 따라 해당 HTML 태그를 화면에 표시하거나 표시하지 않습니다.<br /><br /><u>우리가 흔히 알고 있는 if와 동일</u> |
| v-for         | 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력합니다.<br /><br /><u>우리가 흔히 알고 있는 for과 동일</u> |
| v-show        | v-if와 유사하게 데이터의 진위 여부에 따라 해당 HTML 태그를 화면에 표시하거나 표시하지 않습니다.<br />다만, v-if는 해당 태그를 완전히 삭제하지만 v-show는 css 효과만 display:none;으로 주어<br />실제 태그는 남아 있고 화면 상으로만 보이지 않습니다.<br /><br /><u>v-if와 유사하지만 **v-if는 element 삭제, v-show는 css display:none**</u> |
| v-bind        | HTML 태그의 기본 속성과 뷰 데이터 속성을 연결합니다.<br />**<u>간소화 문법 : [ : ]</u>** |
| v-on          | 화면 요소의 이벤트를 감지하여 처리할 때 사용합니다.<br />예를 들어, v-on:click은 해당 태그의 클릭 이벤트를 감지하여 특정 메서드를 실행할 수 있습니다.<br /><br /><u>우리가 흔히 알고 있는 jquery의 on과 동일</u><br />**<u>간소화 문법 : [ @ ]</u>** |
| v-model       | 폼(form)에서 주로 사용되는 속성입니다. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화합니다.<br />화면에 입력된 값을 저장하여 서버에 보내거나 watch와 같은 고급 속성을 이용하여 추가 로직을 수행할 수 있습니다.<br /><input\>, <select\>, <textarea\> 태그에만 사용할 수 있습니다. |





```html
<!-- 많이 사용되는 디렉티브 다루기 -->
<html>
    <head>
        <title>Vue Template - Directives</title>
    </head>
    <body>
        <div id="app">
            <a v-if="flag">두잇 Vue.js</a> <!-- 1️⃣ -->
            <ul>
                <li v-for="system in systems">{{ system }}</li> <!-- 2️⃣ -->
            </ul>
            <p v-show="flag">두잇 Vue.js</p> <!-- 3️⃣ -->
            <h5 v-bind:id="uid">뷰 입문서</h5> <!-- 4️⃣ -->
            <button v-on:click="popupAlert">경고 창 버튼</button> <!-- 5️⃣ -->
        </div>
    </body>

    <script>
        new Vue({
            el: '#app',
            data: {
                flag: true, // 1️⃣, 3️⃣
                systems: ['android', 'ios', 'window'], // 2️⃣
                uid: 10 // 4️⃣
            },
            methods: {
                popupAlert: function() { // 5️⃣
                    return alert('경고 창 표시');
                }
            }
        });
    </script>
</html>
```

1️⃣ v-if : 분기 처리의 조건 값인 flag 값이 true이므로 '두잇 Vue.js' 텍스트를 화면에 표시합니다.

2️⃣ v-for : 뷰 데이터 systems는 android, ios, window의 총 3개의 값을 가지는 배열입니다. 이 배열의 요소 개수만큼 <li\> 태그가 반복되어 {{ system }}으로 각 요소의 값을 화면에 표시합니다.

3️⃣ v-show : v-if와 마찬가지로 flag 값이 true이므로 '두잇 Vue.js'를 화면에 표시합니다.

4️⃣ v-bind : HTML 태그의 id 속성을 뷰 데이터에 선언한 uid 값과 연결하여 화면에 표시합니다.

5️⃣ v-on : [경고창 버튼]을 클릭했을 때 해당 이벤트를 감지하여 methods 속성에 선언한 popupAlert() 메서드를 수행합니다. 결과적으로 브라우저 기본 경고창을 엽니다.





**이벤트 처리**

- 이벤트 처리를 위해 v-on 디렉티브와 methods 속성을 사용

```html
<!-- v-on 디렉티브를 이용해 이벤트 처리하기 -->
<button v-on:click="clickBtn">클릭</button>

<script>
    methods: {
        clickBtn: function() {
            alert('clicked');
        }
    }
</script>

<!-- v-on 디렉티브로 메소드 호출할 때 인자 값 넘기기 -->
<button v-on:click="clickBtn(10)">클릭</button>

<script>
    methods: {
        clickBtn: function(num) {
            alert('clicked ' + num + ' times');
        }
    }
</script>

<!-- event 인자를 이용해 돔 이벤트에 접근하기 -->
<button v-on:click="clickBtn">클릭</button>

<script>
    methods: {
        clickBtn: function(event) {
            console.log(event);
        }
    }
</script>
```





**고급 템플릿 기법**

```
- computed 속성
- computed 속성과 methods 속성의 차이점
- watch 속성
```



**computed 속성**

- 장점
  - data 속성 값의 변화에 따라 자동으로 다시 연산
  - 캐싱



**computed 속성과 methods 속성의 차이점**

- methods 속성은 호출할 때에만 수행 - 수동적
- computed는 데이터 값이 바뀌면 자동으로 수행 - 능동적
  - 데이터가 변경되지 않는 한 이전의 계산 값을 가지고 있다가(캐싱하고 있다가) 필요할 때 바로 반환

```html
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMsg">문자열 역순</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        methods: {
            reversedMsg: function() {
                this.message = this.message.split('').reverse().join('');
                return this.message;
            }
        }
    });
</script>
```

- 복잡한 연산을 반복 수행해서 화면에 나타내야 한다면 methods 속성보다 computed 속성을 사용하는 것이 성능면에서 효율적



**watch 속성**

- 데이터 변화를 감지하여 자동으로 특정 로직 수행
- computed 속성과 유사하지만 computed 속성은 간단한 연산에 적합
- watch 속성은 데이터 호출과 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합

```html
<div id="app">
    <input v-model="message">
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        watch: {
            message: function(data) {
                console.log("message의 값이 바뀝니다 : ", data);
            }
        }
    });
</script>
```
