# Scrapy

*2020-08-16*

## Table of Contents

- [Scrapy](#scrapy)
  - [Table of Contents](#table-of-contents)
  - [Scrapy folder](#scrapy-folder)
  - [Clock](#clock)
  - [Toggle](#toggle)
  - [localStorage](#localstorage)
  - [foreach](#foreach)
  - [filter](#filter)
  - [Json](#json)

## Scrapy folder
  
  - Spider

    > __pycache__, __init__.py 존재, spider 을 통해 정보를 모아온다. 핵심임.
    scrapy genspider <name> <domain>
    'https://blog.scrapinghub.com/robots.txt' (robots.txt) 홈페이지가 허가한 범위를 알려준다.

    ```js
    // bad
    function Var(){
        for(var i = 0; i < 10; i++){
            console.log('i',i)
        }
    }
    Var()
    console.log('after',i) // i is not defined

    // good
    for(var j = 0; j < 10; j++){
        console.log('j',j)
    }
    console.log('after loop j is', j)
    ```

## Clock

  - Using Date, setinterval, 삼항 연산자

    > 삼항연산자 : var Bool = math.PI > 4 ? "Yes" : "No";
    중첩 : var A = true, B = false;
    var hello = A ? (B ? "true true": "true false") :
    (B ? "false true" : "false false");

    ```js
    function getTime(){

        const date = new Date(); //함수를 호출할 때 new를 붙이면 새로운 객체를 만든 뒤 이를 리턴함.

        const minutes = date.getMinutes();

        const hours = date.getHours();

        const seconds = date.getSeconds();

        clockTitle.innerText = `${hours < 10 ? `0${hours}`:hours}:${minutes < 10 ? `0${minutes}`:minutes}:${seconds  < 10 ? `0${seconds}` : seconds}`;
    }

    function init(){
        setInterval(getTime,1000);
    }
    /*
    //bad
    setInterval(getTime(1,2),1000);
    getTime(1,2)을 하게 된다면 실제 함수가 즉시 실행되게 된다.
    //good
    setInterval(()=>{getTime(1,2);}, 1000);
    */

    init();

    ```
    
## Toggle
  - 하나의 설정 값에서 다른 값으로 변경 (두 가지 상태만이 존재한다.)

    > ex) 
    .mystyle {
    width: 100%;
    padding: 25px;
    background-color: coral;
    color: white;
    font-size: 25px;
    }
    function myFunction() {
    var element = document.getElementById("myDIV");
    element.classList.toggle("mystyle");
    }

    ```js
    const CLICKED_CLASS = "Clicked";
    function handleClick(){
        const currentClass = title.className;
        if (currentClass !== CLICKED_CLASS){
            title.className = CLICKED_CLASS;
        } else {
            title.className = ""; //초기 설정 다 ""로 되어있음
        }
    }

    //? title에 class = "btn"을 추가한다면?...
    //! => class = "clicked" 이전 클래스 btn을 고려하지 않는다.

    // better
    //모든 과정을 축약
    function handleClick(){
        title.classList.toggle(CLICKED_LIST)
    }

    ```
## localStorage
  - localStorage 데이터는 만료 기간이 없다.
  - sessionStorage 브라우저가 종료되면 지워진다.

    > localStorage.setItem('name', 'jess2');

    ```js
    // bad
    function saveToDos() {
        localStorage.setItem(TODOS_LS, toDos)
    }

    // good
    function saveToDos() {
        localStorage.setItem(TODOS_LS, JSON.stringify(toDos))
    }

    function loadToDos() {
    const loadedToDos = localStorage.getItem(TODOS_LS)
    if (loadedToDos !== null) {
        const parsedToDos = JSON.parse(loadedToDos)
        parsedToDos.forEach(function(toDo) {
            paintToDo(toDo.text)
        }) //foreach 문은 array object에서만 가능하다.
        }
    }
    ```
## foreach
  - 주어진 함수를 배열 요소 각각에 대해 실행한다.

    > ex) arr.forEach(callback(currentvalue[, index[, array]])[, thisArg])

    ```js
    function loadToDos() {
        const loadedToDos = localStorage.getItem("toDos");
        if (loadedToDos !== null) {
            const parsedToDos = JSON.parse(loadedToDos);//(local storage string으로 줘야함 아님 걍 object 라고 하더라.) string 객체를 json 객체로 변환시켜주자.
            parsedToDos.forEach(function(toDo) {
            addToDo(toDo.value);
            });//toDos에 있는 항목들 각각에 함수를 적용할 것이다.
        }
        return;
    }
    ```

## filter

  - 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환한다.

    >ex) const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

    const result = words.filter(word => word.length > 6);

    console.log(result);
    // expected output: Array ["exuberant", "destruction", "present"]

    ```js
    function handleDelete(event) {
        const target = event.target;
        const li = target.parentElement;
        const ul = li.parentElement;
        const toDoId = li.id;
        ul.removeChild(li);
        toDos = toDos.filter(function(toDo) {
            return toDo.id !== parseInt(toDoId);//string 을 parse 로 변환시켜줌
        });//toDos 를 순회하며 li 에 없는 id 인 toDos를 체크 true 면 return
        persistToDos();
    }
    ```

## Json

  - Json(javaScript Object Notation) 속성-값 쌍 또는 키-값 쌍으로 이루어진 데이터를 전달하기 위해 인간이 읽을 수 있는 텍스트를 제공

    >json 객체
    {
    "name": "식빵",
    "family": "웰시코기",
    "age": 1,
    "weight": 2.14
    }
    json 배열
    "dog": [
    {"name": "식빵", "family": "웰시코기", "age": 1, "weight": 2.14},
    {"name": "콩콩", "family": "포메라니안", "age": 3, "weight": 2.5},
    {"name": "젤리", "family": "푸들", "age": 7, "weight": 3.1}
    ]

    ```js
    toDos = toDos.filter(function(toDo) {
        return toDo.id !== parseInt(toDoId);//string 을 parse 로 변환시켜줌
    });//toDos 를 순회하며 li 에 없는 id 인 toDos를 체크 true 면 return
    persistToDos();
    ```