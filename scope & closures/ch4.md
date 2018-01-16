# You Don't Know JS: Scope & Closures
# Chapter 4: Hoisting

Đọc tới chương này là coi như bạn đã thoải mái với phạm vi, và cách biến được gắn với các phạm vi ở tầng khác nhau, cách nó được khai báo. Cả phạm vi hàm (function scope) và phạm vi khối (block scope) đều có chung nguyên tắc: biến nào khai báo ở phạm vi nào thì thuộc về phạm vi đó.

Có một chi tiết tinh tế về cách thức đính kèm scope hoạt ra sao với khai báo xuất hiện ở các vị trí khác trong scope.

## Gà hay trứng?

Có thể bạn nghĩ rằng JavaScript được thông dịch theo từng dòng, từ trên xuống dưới khi thực thi chương trình. Đúng là vậy, nhưng lại có một phần giả định đó sẽ dẫn đến suy nghĩ sai.

Xem đoạn code sau:

```js
a = 2;

var a;

console.log( a );
```

Bạn nghĩ nó sẽ log ra cái gì trong lệnh `console.log(..)`?

Nhiều lập trình viên sẽ nghĩ là `undefined` vì `var a` xuất hiện sau `a = 2`, và đương nhiên biến đó sẽ được định nghĩa lại theo tự nhiên, và sẽ là `undefined`. Nhưng kết quả là `2`.

Xem đoạn code dưới đây:

```js
console.log( a );

var a = 2;
```

Dựa theo đoạn code ở trên kia thì có thể bạn sẽ nghĩ rằng `2` sẽ được in ra, kiểu như hành vi tìm kiếm từ trên xuống được tối thiểu hóa. Một số người thì nghĩ rằng `a` được gọi trước khi khai báo, nên kết quả chắc chắn là `ReferenceError`.

Không may, cả hai đều sai, kết quả là `undefined`.

**Vậy chuyện gì đã xảy ra?** Câu chuyện gà và trứng xuất hiện. Cái nào có trước? Trứng hay gà?

## Phản biện của trình biên dịch

Để trả lời câu hỏi này, chúng ta cần ngược lại Chương 1, và câu chuyện trình biên dịch của chúng ta. Gọi lại *Engine* sẽ biên dịch code trước khi diễn giải nó. Một phần của giai đoạn biên dịch là tìm và kết hợp với tất cả các khai báo trong phạm vi phù hợp. Chương 2 đã chỉ cho ta thấy đó chính là trái tim của lexical scope (phạm vi từ vựng).

Vì vậy, cách tốt nhất để nghĩ về hoisting là: các khai báo biến và hàm đó được xử lý trước khi thực thi.

Khi bạn thấy `var a = 2;`, bạn có thể nghĩ nó là một câu lệnh. Nhưng thực tế thì JavaScript hiểu nó là hai lệnh: `var a;` và `a = 2;`. Câu lệnh đầu tiên, khai báo sẽ được xử lý trong giai đoạn dịch. Câu lệnh thứ hai, phép gán
, được đặt đúng chỗ cho giai đoạn thực thi.

Đoạn code đầu của chúng ta có thể được viết như thế này:

```js
var a;
```
```js
a = 2;

console.log( a );
```

...phần đầu là biên dịch và phần sau là thực thi.

Tương tự, đoạn code thứ 2 có thể ghi như sau:

```js
var a;
```
```js
console.log( a );

a = 2;
```

Do đó, một cách suy nghĩ về tiến trình này theo cách ẩn dụ rằng khai báo biến và hàm được "chuyển" đến nơi chúng xuất hiện trong luồng code đến đầu code. Điều này dẫn đến cái tên "hoisting"

Nói cách khác, **trứng có trước gà**.

**Ghi chú:** Chỉ có khai báo tự nó được hoist, trong khi các phép gán hay logic thực thi khác được đặt vào chỗ. Nếu hoisting tự sắp xếp logic thực thi cho code, nó sẽ dẫn đến sự tàn phá.

```js
foo();

function foo() {
	console.log( a ); // undefined

	var a = 2;
}
```

Khai báo hàm `foo` được hosting (trường hợp này bao gồm giá trị ngụ ý của nó như một hàm thực tế), sao cho việc gọi hàm ở dòng đầu tiên có thể thực hiện được.

Đồng thời cũng rất quan trọng để chú ý rằng hoisting theo từng scope. Đoạn code trước của ta đơn giản là nó một scope toàn cục, hàm `foo(..)` mà chúng ta thực hiện ở đây chính nó cho thấy `var a` được chuyển lên trên cùng của `foo(..)`. Vì vậy chương trình có lẽ giải thích chính xác như sau:

```js
function foo() {
	var a;

	console.log( a ); // undefined

	a = 2;
}

foo();
```

Khai báo hàm được đưa lên trên nhưng biểu thức hàm thì không.

```js
foo(); // not ReferenceError, but TypeError!

var foo = function bar() {
	// ...
};
```

Định danh biến `foo` được đưa lên và gắn với scope của toàn bộ chương trình (toàn cục), vì vậy `foo()` không thất bại với lỗi `ReferenceError`. Nhưng `foo` lại chưa có giá trị nào (mà nó phải có vì nó là khai báo hàm chứ không phải biểu thức). Vì vậy, khi `foo()` có gắng tìm giá trị `undefined`, thì lại bị `TypeError`.

Và cũng nhắc lại rằng cho dù là một biểu thức hàm được đặt tên, tên định danh của nó cũng không có trong toàn bộ scope:

```js
foo(); // TypeError
bar(); // ReferenceError

var foo = function bar() {
	// ...
};
```

Đoạn này cần diễn đạt chính xác (có hoisting) hơn như sau:

```js
var foo;

foo(); // TypeError
bar(); // ReferenceError

foo = function() {
	var bar = ...self...
	// ...
}
```

## Hàm trước

Cả khai báo và biến đều được hoist. Nhưng có một chi tiết tinh vi (có thể xuất hiện trong mã với nhiều khai báo "trùng lặp") rằng hàm được đưa lên trước, sau mới tới biến.

Xem:

```js
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```

`1` được in ra thay vì `2`! Đoạn code này được diễn dịch bởi *Engine* như sau:

```js
function foo() {
	console.log( 1 );
}

foo(); // 1

foo = function() {
	console.log( 2 );
};
```

Để ý rằng khai báo `var foo` đã trùng lặp (và bị bỏ qua), mặc dù nó đến trước khai báo `function foo()...`, bởi khia báo hàm được đưa lên trước các biến thông thường.

Trong khi khai báo `var` trùng lặp nhiều lần được bỏ qua thì khai báo hàm tiếp theo lại ghi đè lên hàm trước.

```js
foo(); // 3

function foo() {
	console.log( 1 );
}

var foo = function() {
	console.log( 2 );
};

function foo() {
	console.log( 3 );
}
```

Mặc dù có vẻ nó không có gì thú vị hơn về mặt học thuật, nhưng nó làm nổi bật sự trùng lặp trong cùng một scope và là một ý tồi dẫn đến kết quả khó hiểu.

Khai báo hàm thường xuất hiện trong block thông thường
Function declarations that appear inside of normal blocks typically hoist to the enclosing scope, rather than being conditional as this code implies:

```js
foo(); // "b"

var a = true;
if (a) {
   function foo() { console.log( "a" ); }
}
else {
   function foo() { console.log( "b" ); }
}
```

However, it's important to note that this behavior is not reliable and is subject to change in future versions of JavaScript, so it's probably best to avoid declaring functions in blocks.

## Review (TL;DR)

We can be tempted to look at `var a = 2;` as one statement, but the JavaScript *Engine* does not see it that way. It sees `var a` and `a = 2` as two separate statements, the first one a compiler-phase task, and the second one an execution-phase task.

What this leads to is that all declarations in a scope, regardless of where they appear, are processed *first* before the code itself is executed. You can visualize this as declarations (variables and functions) being "moved" to the top of their respective scopes, which we call "hoisting".

Declarations themselves are hoisted, but assignments, even assignments of function expressions, are *not* hoisted.

Be careful about duplicate declarations, especially mixed between normal var declarations and function declarations -- peril awaits if you do!
