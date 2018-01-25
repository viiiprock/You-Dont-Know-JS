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

"Nhưng...chẳng ai muốn viết code xấu vậy!" Đúng vậy, cũng giống việc chẳng ai viết code theo phong cách code được phiên dịch  từ CoffeeScript cả. Nhưng đây không phải vấn đề.

Vấn đề là công cụ này có thể chuyển code ES6 có thể làm việc với môi trường tiền ES6. Bạn có thể viết code theo block-scoping với những ích lợi từ nó, rồi để cho công cụ ở bước built lo phần chuyển đổi giúp code có thể làm việc ngon lành khi triển khai.

E hèm, đây là lối đi ưa thích cho mọi người trong việc chuyển đổi ES6: sử dụng một trình phiên dịch code để chuyển code ES6 thành code tương thích ES5 trong quá trình (các trình duyệt - người dịch) hoán chuyển từ tiền ES6 sang ES6.

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

Vì vậy tôi đã xây dụng một công cụ gọi là [let-er](https://github.com/getify/let-er) để xử lý vấn đề này. *let-er* là một trình phiên dịch  code ở bước build, nhiệm vụ duy nhất của nó là tìm dạng lệnh let và phiên dịch , những dạng code khác nó vẫn để nguyên, kể cả khai báo let. Bạn có thể sử dụng *let-er* một cách an toàn như là bước phiên dịch  ES6 ban đầu, sau đó chuyển code của bạn qua gì đó như Traceur nếu cần thiết.

Hơn nữa, *let-er* có một cờ cấu hình (configuration flag) `--es6`, có thể bật (mặc định là tắt), thay đổi kiểu code được tạo. Thay vì dùng `try/catch`, *let-er* sẽ chuyển đoạn code tuân thủ ES6 đầy đủ mà không phải hack gì cả:

```js
{
	let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Bạn có thể dùng *let-er* ngay bây giờ và quan trọng nhất, **bạn có thể sử dụng dạng lệnh let một cách thích hợp và rõ ràng** mặc dù nó không (chưa) phải là phần chính thức của ES.

## Hiệu suất

Tôi nêu nhanh về hiệu suất `try/catch` một chút để giải thích câu hỏi "tại sao sử dụng IIFE để tạo scope?"

Trước tiên, hiệu suất của `try/catch` chậm hơn nhưng lại không có một giả định hợp lý rằng phải dùng cách này, hay nó vốn phải vậy. Từ khi TC39 chính thức chấp nhận trình phiên dịch ES6 sử dụng `try/catch`, nhóm Traceur đã yêu cầu Crome cải thiện hiệu suất của `try/catch`, và rõ ràng là họ có động cơ để đòi hỏi.

Tiếp đến, IIFE không phải là cách so sánh ngang bằng với `try/catch`, vì bất kỳ đoạn code nào được bao bởi hàm đều thay đổi ý nghĩa bên trong code, ý nghĩa của `this`, `return`, `break`, và `continue`. IIFE không phải là một thay thế phù hợp. Nó chỉ có thể dùng thủ công tùy trường hợp.

Câu hỏi cụ thể là: bạn có thực sự muốn block-scoping hay không? Nếu có thì công cụ cho bạn một phương án. Nếu không, hãy cứ dùng `var` và tiếp tục code thôi.