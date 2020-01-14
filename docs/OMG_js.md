

# 前言

稍微看了一下[TypeScript 開發實戰](https://download.microsoft.com/download/C/6/0/C60E2BD0-8A7C-479F-851E-8B5810C0D70F/20130504_MVP_Track3_Session6.pdf)、[使用 TypeScript 駕馭](https://download.microsoft.com/download/7/8/D/78D289B4-CC63-4EA8-BB40-0C957C64F013/20160510_InnovativeApplicationsDevelopmentConference_session7.pdf)。我以為我js算熟&#x2026;還是有東西和我想得不同&#x2026;簡單嘗試了一下裡頭的範例。

測試的node.js版本：

    node --version

    v12.13.0


# 各個範例嘗試


## 'string' 和 String不同

    ```js

    var o1 = 'Hello World';
    var o2 = new String('Hello World');
    console.log("o1 == o2: ", o1 == o2) // true
    console.log("o1 === o2: ", o1 === o2) // false
    console.log("typeof o1: ", typeof o1) // 'string'
    console.log("typeof o2: ", typeof o2) // 'object

    ```

    o1 == o2:  true
    o1 === o2:  false
    typeof o1:  string
    typeof o2:  object

原來 `new String()` 和原生 `string` 出來的不是同個類別&#x2026;

    ```js

    var o1 = 'Hello World';
    var o2 = new String('Hello World');
    console.log("o2 instanceof String: ", o2 instanceof String) // true
    console.log("o1 instanceof String: ", o1 instanceof String) // false
    
    try{
      o1 instanceof string // Erro, not define string
    }catch (e){
      console.debug(e)
    }

    ```

    o2 instanceof String:  true
    o1 instanceof String:  false
    ReferenceError: string is not defined
        at Object.<anonymous> (/tmp/babel-18852wT2/js-script-18852pye:7:17)
        at Module._compile (internal/modules/cjs/loader.js:956:30)
        at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
        at Module.load (internal/modules/cjs/loader.js:812:32)
        at Function.Module._load (internal/modules/cjs/loader.js:724:14)
        at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)
        at internal/main/run_main_module.js:17:11


### 但是'string'會受到String.prototype影響&#x2026;

`'string'` 不能增加屬性或方法

    ```js

    var car = 'Hello World';
    car.name = "BMW";
    car.start = function() {
        return true;
    }
    
    try{
      car.start();
    }catch(e){
      console.debug(e);
    }
    /*
    Thrown:
    TypeError: car.start is not a function
    */
    
    console.log("car: ", car) // 'Hello World'

    ```

    TypeError: car.start is not a function
        at Object.<anonymous> (/tmp/babel-18852wT2/js-script-18852CbA:8:7)
        at Module._compile (internal/modules/cjs/loader.js:956:30)
        at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
        at Module.load (internal/modules/cjs/loader.js:812:32)
        at Function.Module._load (internal/modules/cjs/loader.js:724:14)
        at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)
        at internal/main/run_main_module.js:17:11
    car:  Hello World

但是 `String` 產生的物件可以&#x2026;

    ```js

    var car = new String('Hello World');
    car.name = "BMW";
    car.start = function() {
    return true;
    }
    console.log("car.start(): ", car.start()); // true
    console.log("car: ", car) // [String: 'Hello Wrold'] { name: 'BMW', start: [Function] }

    ```

    car.start():  true
    car:  [String: 'Hello World'] { name: 'BMW', start: [Function] }

兩個都會受到 `String.prototype` 的影響Orz&#x2026;

    ```js

    var car = 'Hello World';
    String.prototype.name = "BMW";
    String.prototype.start = function()
    {
      return true;
    }
    var txt = 'Will';
    
    console.log("txt.name: ", txt.name) // "BMW"

    txt.name:  BMW

---

    ```js

    var car = new String('Hello World');
    String.prototype.name = "BMW";
    String.prototype.start = function()
    {
      return true;
    }
    var txt = 'Will';
    console.log("txt.name: ", txt.name) // "BMW"

    ```

    txt.name:  BMW

因為他們的 `prototype` 指向同一個物件&#x2026;

    ```js

    o1 = new String("Hello, World")
    o2 = "Hello, World"
    
    console.log("o1 === o2: ", o1 === o2) // false
    console.log("o1.prototype  === o2.prototype: ", o1.prototype  === o2.prototype) // ture

    ```

    o1 === o2:  false
    o1.prototype  === o2.prototype:  true


## Function

    ```js

    var o1 = function() {}
    var o2 = new Function();
    console.log("o1 == o2: ", o1 == o2) // false
    console.log("o1 === o2: ", o1 === o2) // false
    console.log("typeof o1: ", typeof o1) // function
    console.log("typeof o2: ", typeof o2) // function

    ```

    o1 == o2:  false
    o1 === o2:  false
    typeof o1:  function
    typeof o2:  function


# 其他資料

-   [Zero in JavaScript](http://zero.milosz.ca/)

