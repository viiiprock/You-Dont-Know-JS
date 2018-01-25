# You Don't Know JS: Scope & Closures
# Phục lục B: Polyfilling Phạm Vi Block

Trong Chương 3, chúng ta khám phá Block Scope. Ta thấy rằng mệnh đề `with` và `catch` đều là hai ví dụ nhỏ của block scopetồn tồn tại trong JS từ ES3.

Khi ES6 giới thiệu `let` cuối cùng cũng cung cấp đầy đủ khả năng mở rộng của block-scoping cho code của chúng ta. Có rất nhiều điều hấp dẫn, cả hàm và phong cách code mà block scope sẽ cho phép.

Nhưng nếu như chúng ta sử dụng block scope trong môi trường trước ES6.

Xem đoạn code sau:

```js
{
	let a = 2;
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Với môi trường ES6 thì đoạn này chơi ngon, nhưng với bản trước ES6 `catch` sẽ là câu trả lời.

```js
try {
	throw 2
} catch(a) {
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Whoa! Code nhìn xấu vãi cả dị. Ta thấy `try/catch` xuất hiện để ép việc ném ra lỗi, nhưng "error" chỉ quăng ra giá trị `2`, và sau đó khai báo biến sẽ nhận giá trị bên trong mệnh đề `catch(a)`. Bể não.

Đúng vậy, mệnh đề `catch` có block-scoping của nó, nghĩa là nó có thể sử dụng như một polyfill cho block scope trong môi trường tiền ES6.

"Nhưng...chẳng ai muốn viết code xấu vậy!" Đúng vậy, cũng giống việc chẳng ai viết code theo phong cách code được biên dịch từ CoffeeScript cả. Nhưng đây không phải vấn đề.

Vấn đề là công cụ này có thể chuyển code ES6 có thể làm việc với môi trường tiền ES6. Bạn có thể viết code theo block-scoping với những ích lợi từ nó, rồi để cho công cụ ở bước built lo phần chuyển đổi giúp code có thể làm việc ngon lành khi triển khai.

E hèm, đây là lối đi ưa thích cho mọi người trong việc chuyển đổi ES6: sử dụng một trình biên dịch code để chuyển code ES6 thành code tương thích ES5 trong quá trình (các trình duyệt - người dịch) hoán chuyển từ tiền ES6 sang ES6.

## Traceur

Google bảo trì một dự án tên là "Traceur" [Google Traceur](http://traceur-compiler.googlecode.com/git/demo/repl.html), với nhiệm vụ chính xác là chuyển các tính năng ES6 sang tiền ES6 (hầu hết là ES5, nhưng không phải tất cả!) để sử dụng chung. Ủy ban TC39 dựa vào công cụ này (và các công cụ khác) để kiểm tra ngữ nghĩa của các tính năng họ chỉ định.

Traceur tạo gì trong đoạn code của ta? Bạn đoán rồi đó!

```js
{
	try {
		throw undefined;
	} catch (a) {
		a = 2;
		console.log( a );
	}
}

console.log( a );
```

Với việc sử dụng các công cụ như vậy, chúng ta bắt đầu khai thác lợi thế của block scope bất kể ta có nhắm đến ES6 hay không, bởi vì `try/catch` đã tồn tại và làm việc theo cách này từ hồi ES3.

## Khối ngầm vs. minh bạch

Trong Chương 3, chúng ta nhận diện vài cạm bẫy tiềm tàng với tính bảo trì/refactor của code khi giới thiệu về block-scoping. Có cách nào khác để tận dụng lợi thế của block scope mà giảm thiểu nhược điểm này?

Xem xét hình thức thay thế của `let`, gọi là "let block" hay "câu lệnh let" (tương phản với "khai báo let").

```js
let (a = 2) {
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Thay vì ngầm chiếm lấy block hiện hữu, lệnh let tạo một block rõ ràng cho ràng buộc scope của nó. Không chỉ làm nổi bật các block minh bạch hơn, mà có lẽ thiết thực hơn trong việc refactor code, nó tạo ra gì đó sạch hơn về mặt ngữ pháp, buộc tất cả các khai báo lên trên đầu block. Nó làm cho bấy kỳ block nào cũng dễ thấy và scope gì của nó hay không.

Là một khuôn mẫu, nó phản ánh cách tiếp cận của nhiều người trong việc scope hóa hàm khi họ tự dời/hoist tất cả khai báo `var` lên trên đầu hàm. Lệnh let đặt chúng lên đầu block theo ý định, và nếu bạn không sử dụng khai báo `let` rải rác, khai báo block scoping của bạn sẽ dễ dàng nhận diện để bảo trì hơn.

Nhưng lại có một vấn đề, dạng lệnh let không có trong ES6. Và cả Traceur cũng không chấp nhận dạng code này.

Ta có hai lựa chọn. Ta có thể bắng bằng cách sử dụng cú pháp ES6 hợp lệ và một chút khuôn phép:

```js
/*let*/ { let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Nhưng các công cụ tồn tại để giải quyết vấn đề của chúng ta. Vì vậy lựa chọn khác là viết các khối lệnh let rõ ràng và để công cụ chuyển chúng thành code hợp lệ.

Vì vậy tôi đã xây dụng một công cụ gọi là [let-er](https://github.com/getify/let-er) để xử lý vấn đề này. *let-er* là một trình thông dịch code ở bước build, nhiệm vụ duy nhất của nó là tìm dạng lệnh let và thông dịch, những dạng code khác nó vẫn để nguyên, kể cả khai báo let. Bạn có thể sử dụng *let-er* như
is a build-step code transpiler, but its only task is to find let-statement forms and transpile them. It will leave alone any of the rest of your code, including any let-declarations. You can safely use *let-er* as the first ES6 transpiler step, and then pass your code through something like Traceur if necessary.

Moreover, *let-er* has a configuration flag `--es6`, which when turned on (off by default), changes the kind of code produced. Instead of the `try/catch` ES3 polyfill hack, *let-er* would take our snippet and produce the fully ES6-compliant, non-hacky:

```js
{
	let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

So, you can start using *let-er* right away, and target all pre-ES6 environments, and when you only care about ES6, you can add the flag and instantly target only ES6.

And most importantly, **you can use the more preferable and more explicit let-statement form** even though it is not an official part of any ES version (yet).

## Performance

Let me add one last quick note on the performance of `try/catch`, and/or to address the question, "why not just use an IIFE to create the scope?"

Firstly, the performance of `try/catch` *is* slower, but there's no reasonable assumption that it *has* to be that way, or even that it *always will be* that way. Since the official TC39-approved ES6 transpiler uses `try/catch`, the Traceur team has asked Chrome to improve the performance of `try/catch`, and they are obviously motivated to do so.

Secondly, IIFE is not a fair apples-to-apples comparison with `try/catch`, because a function wrapped around any arbitrary code changes the meaning, inside of that code, of `this`, `return`, `break`, and `continue`. IIFE is not a suitable general substitute. It could only be used manually in certain cases.

The question really becomes: do you want block-scoping, or not. If you do, these tools provide you that option. If not, keep using `var` and go on about your coding!
