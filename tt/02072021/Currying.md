# Currying function

Currying là một kỹ thuật khi làm việc với function. Không chỉ có ở Javascript, một số ngôn ngữ khác vẫn có hổ trợ kỹ thuật này.

Currying( Cà ri???) là một kỹ thuật cho phép chúng ta tranform từ f(a,b,c) sang f(a)(b)(c). Currying không gọi là một function, nó chỉ đơn giản là chuyển đổi cách viết hàm theo kiểu khác. Bên cạnh kỹ thuật này, có một số kỹ thuật khác, có thể gom nhóm, như Pure function, High-order function.

### Example

Ví dụ convert function max(a,b) sang max(a)(b).

#### Không phải currying function.

```nodejs
function max(a,b){
    return a >= b? a : b;
}

console.log("Max 5 and 6 is: ", max(5,6))
```

Kết quả:

```bash
PS C:\Users\pnt\tt> node test.js
Max 5 and 6 is:  6
```

#### Currying function

```javascript
function max(a) {
  return (b) => {
    return a >= b ? a : b;
  };
}

console.log("Max 5 and 6 is: ", max(5)(6));
```

Thay vì chúng ta truyền vào 2 tham số vào trong 1 hàm để so sánh sổ như hàm trên, chúng ta chỉ cần truyền một tham số ở hàm max, hàm max sẽ trả về một function, tiếp tục truyền vào tham số thứ 2 vào hàm vừa nhận được. Kết quả chúng ta vẫn so sánh được 2 số để xem số nào lớn hơn.

Kết quả:

```bash
PS C:\Users\pnt\tt> node test.js
Max 5 and 6 is:  6
```

2 ví dụ trên đã thể hiện được cơ bản kỹ thuật currying. Nó chuyển một hàm số dạng f(a,b,c) sang f(a)(b)(c) mà vẫn giữ được tính logic của nó, mặc dùng việc dùng currying function khiến cho việc implement phức tạp hơn, bù vào đó lợi ích nó mạng lại ở. Chẳng hạn:

#### Viết hàm tính giá cuối cùng với discount (10%) so với giá hiện tại.

```javascript
function countBill(price, discount) {
  return (price * discount) / 100;
}
console.log("Price 100: ", countBill(100, 10));
console.log("Price 100: ", countBill(200, 10));
console.log("Price 100: ", countBill(400, 10));
```

Kết quả:

```bash
PS C:\Users\pnt\tt> node test.js
Price 100:  90
Price 200:  180
Price 400:  360

```

Đoạn code trên muốn tính giá tiền cuối cùng cần phải truyền discount vào cho mỗi lần tính, khiến chúng ta cần phải truyền tham số nhiều lần. Cách code hàm countBill bằng currying function:

```javascript
function discount(disc) {
  return (price) => {
    return price - (price * disc) / 100;
  };
}
const countBill = discount(10);
console.log("Price 100: ", countBill(100));
console.log("Price 200: ", countBill(200));
console.log("Price 400: ", countBill(400));
```

Kết quả:

```bash
PS C:\Users\pnt\tt> node test.js
Price 100:  90
Price 200:  180
Price 400:  360

```

Bằng cách dùng currying function, chỉ cần truyền discount 1 lần, chúng ta có thể tính kết quả nhiều price cuối cùng được tính từ price gốc với discount giống nhau mà chỉ cần truyền vào price mà không cần truyền discount lặp đi lặp lại.

Cái gì có lợi cũng có hại, mọi người thử viết function nào đó mà format f(a,b,c) đơn giản nhưng chuyển qua f(a)(b)(c) lại phức tạp.

# Pure function

Pure function là atomatic building block trong lập trình hàm. Pure function có kết quả chỉ ảnh hưởng từ các tham số đầu vào, kết quả không bị tác động bởi các hiệu ứng lề như mutating input, log, http request, write/read a disk,..

### Pure function = Consistent result

```javascript
cons sum = (x,y)=> x + y;
console.log("Sum 5 and 6 ís: ", sum(5,6));
```

Result

```bash
p@nt:~/tt$ node test.js
Sum 5 and 6 ís:  11
```

Hàm trên thể hiện tính pure của function, kết quả chỉ ảnh hưởng bới x và y.

### Impure function = Inconsistent result

```javascript
let x = 2;
const sum = (y) => (x += y);

console.log("Sum 2 and 2 ís: ", sum(2));
console.log("Sum 2 and 2 ís: ", sum(2));
```

Kết quả

```bash
p@nt:~/tt$ node test.js
Sum 2 and 2 ís:  4
Sum 2 and 2 ís:  6
```

Để tránh impure function:

1. Tham số là immutable
2. Kết quả trả về không bị ảnh hưởng bởi http request
3. Kết quả trả về không bị ảnh hưởng bởi read/write file

#### Tác dụng pure function

1. Thích hợp cho việc testing vì kết quả chỉ ảnh hưởng từ đầu vào.
2. Refactor code.

# Higher-order function

Higher-order function(Hàm bậc cao) là hàm nhận hàm khác như một đối số hoặc trả về một hàm số. Higher-order function trái ngược với firt order function, hàm số không nhận hàm số khác là đối số hoặc trả về một hàm.

Chúng ta từng tìm hiểu qua map, filter, ređuce. Chúng đều là higher-order function.

Currying function cũng là một dạng của higher-order function, function trả về kết quả là một function.

Callback cũng là một dạng của higher-order function với việc nhận đối số là một function.

#### Example higher-order function như callback

```javascript
const checkNumber = (isOdd, then) => {
  if (!isOdd) then();
};

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10].forEach((n) => {
  checkNumber(n % 2 !== 0, () => {
    console.log(`${n} is even number`);
  });
});
```

Kết quả:

```bash
p@nt:~/tt$ node test.js
2 is even number
4 is even number
6 is even number
8 is even number
10 is even number
```

### Author: phatkhongbug.

