# Hoisting in Javascript

Hoisting (cẩu, kéo, móc,...) là cơ chế của ngôn ngữ Javascript, do ngôn ngữ thiết kế ,biến và hàm khai báo thì nơi khai báo của chúng sẽ được move lên trên top của scope trước khi chúng được thực thi.

Điều này có nghĩa nơi khai báo của biến và hàm ở đâu không phải là vấn đề, bởi vì chúng được khai báo ở trên top của phạm vi chúng hoạt động, có thể là global hoặc local.

Chú ý rằng thật sự hoisting chỉ move phần khai báo của biến hoặc hàm lên trên top của scope, việc thực thi(assign) thì vẫn ở vị trí ban đâu.

### undefined vs referrenceError.
Trong Javascript, biến chưa khai báo sẽ được gán value là undefined lúc thực thi và kiểu nó cũng là undefined.

```javascript
console.log(typeof variable);
``` 

Kết quả

```bash
p@nt:~/tt$ node test.js
undefined
```
referrenceError xảy ra khi cố gắng truy cập một biến chưa được khai báo từ trước.

```javascript
console.log(typeof variable);
console.log(variable);
```

Kết quả:
```bash
p@nt:~/tt$ node test.js
undefined
/home/p/tt/test.js:2
console.log(variable);
            ^

ReferenceError: variable is not defined
    at Object.<anonymous> (/home/p/tt/test.js:2:13)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
```

### Hoisting variable
Chu trình của một biến trong Javascript: Declaration -> Initialisation/Assignment -> Usage. Tuy nhiên, vì Javascript cho phép chúng ta vừa khai báo và init, nên thông thường hay khai báo một biến.

```javascript
var a = 100;
```
Nhưng bản chất bên trong Javascript, biến được khai báo trước (chuyển lên top scope) sau đó mới khởi tạo giá trị cho biến.

Tuy nhiên, những biến chưa được khai báo (undeclared variable - biến không có keyword) không tồn tại cho đến khi dòng code assign value của chúng được thực thi. Việc thực thi assign value cho biến không được khai báo ngầm định tạo nó như một biến global. Đều đó có nghĩa, tất cả các biến không được khai báo là global.

```javascript
function hoist() {
  a = 10;
  var b = 20;
}

hoist();

console.log("a is: ", a);
console.log("b is: ", b);
```
Kết quả:

```bash
p@nt:~/tt$ node test.js
a is:  10
/home/p/tt/test.js:9
console.log("b is: ", b);
                      ^

ReferenceError: b is not defined
    at Object.<anonymous> (/home/p/tt/test.js:9:23)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
```

Đoạn code trên cho thấy dễ dàng gặp rủi ro khi sử dụng biến mà không khai báo, đôi khi biến đó ở global scope mà không biết. Vì vậy, cần khai báo biến ở mọi phạm vi, dù global hay local.

### Var
Phạm vi hoạt động của khai báo biến **_var_** là môi trường thực thi hiện tại của nó.

#### Global variable

```jạvascript
console.log(hoist);
var hoist = 'The hoist is not defined';

```
Kêt quả bên trên có phải là ReferrenceError: hoist is not defined?

Kêt quả:

```bash
p@nt:~/tt$ node test.js
undefined
```

Giải thích:

```javascript
var hoist;
console.log(hoist);
hoist = 'The hoist is not defined';
```
**_console.log(hoist)_** thực thi khi hoist chưa được assign value => hoist = undefined.

#### Function scope variable

```javascript

hoist();
console.log("A: ",a)
function hoist(){
  console.log("Message: ",message)
  var message = 'init';
  console.log("Message: ",message);
  a = 3;
}

```
Kết quả:

```bash
p@nt:~/tt$ node test.js
Message:  undefined
Message:  init
A:  3
```

Giải thích:

```javascript
function hoist(){    //Hosting move declare function on the top
  console.log("Message: ",message)
  var message = 'init';
  console.log("Message: ",message);
  a = 3;   //Declare before usage
}
hoist();
console.log("A: ",a) // Global, is initilied

```
