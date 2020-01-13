
# Table of Contents

1.  [前言](#orged1a6fb)
2.  [各個範例嘗試](#org33fa39f)
    1.  ['string' 和 String不同](#org6abfdf1)
        1.  [但是'string'會受到String.prototype影響&#x2026;](#orga24d854)
    2.  [Function](#orgda6cd54)
3.  [其他資料](#org35ad2da)



<a id="orged1a6fb"></a>

# 前言

稍微看了一下[TypeScript 開發實戰](https://download.microsoft.com/download/C/6/0/C60E2BD0-8A7C-479F-851E-8B5810C0D70F/20130504_MVP_Track3_Session6.pdf)、[使用 TypeScript 駕馭](https://download.microsoft.com/download/7/8/D/78D289B4-CC63-4EA8-BB40-0C957C64F013/20160510_InnovativeApplicationsDevelopmentConference_session7.pdf)。我以為我js算熟&#x2026;還是有東西和我想得不同&#x2026;簡單嘗試了一下裡頭的範例。

測試的node.js版本：

    node --version

    v12.13.0


<a id="org33fa39f"></a>

# 各個範例嘗試

    node --version


<a id="org6abfdf1"></a>

## 'string' 和 String不同

    var o1 = 'Hello World';
    var o2 = new String('Hello World');
    o1 == o2 // true
    o1 === o2 // false
    typeof o1 // 'string'
    typeof o2 // 'object

原來 `new String()` 和原生 `string` 出來的不是同個類別&#x2026;

    o2 instanceof String // true
    o1 instanceof String // false
    
    o1 instanceof string // Erro, not define string


<a id="orga24d854"></a>

### 但是'string'會受到String.prototype影響&#x2026;

`'string'` 不能增加屬性或方法

    var car = 'Hello World';
    car.name = "BMW";
    car.start = function() {
        return true;
    }
    car.start();
    
    /*
    Thrown:
    TypeError: car.start is not a function
    */
    
    car // 'Hello World'

但是 `String` 產生的物件可以&#x2026;

    var car = new String('Hello World');
    car.name = "BMW";
    car.start = function() {
    return true;
    }
    car.start(); // true
    car // [String: 'Hello Wrold'] { name: 'BMW', start: [Function] }

兩個都會受到 `String.prototype` 的影響Orz&#x2026;

    var car = 'Hello World';
    String.prototype.name = "BMW";
    String.prototype.start = function()
    {
    return true;
    }
    var txt = 'Will';
    
    txt.name // "BMW"

    var car = new String('Hello World');
    String.prototype.name = "BMW";
    String.prototype.start = function()
    {
      return true;
    }
    var txt = 'Will';
    txt.name // "BMW"

因為他們的 `prototype` 指向同一個物件&#x2026;

    o1 = new String("Hello, World")
    o2 = "Hello, World"
    
    o1 === o2 // false
    o1.prototype  === o2.prototype // ture


<a id="orgda6cd54"></a>

## Function

    var o1 = function() {}
    var o2 = new Function();
    o1 == o2 // false
    o1 === o2 // false
    typeof o1 // function
    typeof o2 // function


<a id="org35ad2da"></a>

# 其他資料

-   [Zero in JavaScript](http://zero.milosz.ca/)

