# You Don't Know JS: *this* & Các nguyên mẫu đối tượng (object proptotypes)
# Chương 1: `this` hay  That?

Một trong những cơ chế hãi hùng nhất của JavaScript là từ khoá `this`. Nó là một từ khoá nhận dạng đặc biệt tự động xác định trong scope của mọi function, nhưng những gì chính xác nó đề cập đến còn  làm điêu đứng ngay cả JavaScript dev dày dạn.
> Không phân biệt được công nghệ *tiên tiến* với ma thuật. -- Arthur C. Clarke

Cơ chế của `this` không hẳn ghê gớm như vậy, nhưng các lập trình viên thường diễn giải trong tâm trí bằng cách thêm từ "phức tạp" hoặc "mơ hồ", và không có câu hỏi nào ngoài việc hiểu rõ ràng, `this` có thể xem là sự huyền diệu trong sự hoang mang của bạn. 

**Ghi chú** Từ `this` là một đại từ trong đàm luận chung. Vì vậy, nó có thể rất khó, đặc biệt là bằng lời nói, để xác định chúng ta sử dụng `this` như một đại từ hoặc sử dụng nó để diễn đạt đến từ khoá xác định. Để rõ ràng, tôi sẽ sử dụng `this` như là một từ khoá đặc biệt "this" hay *this* hoặc nếu không thì this. 


## Tại sao lại là `this`?

Nếu cơ chế `this` quá hoang mang, ngay cả đối với lập trình viên JavaScript dày dạn, người ta có thể thắc mắc tại sao nó lại hữu dụng? Nó có rắc rối hơn hay chính đáng? Trước khi tìm hiểu *như thế nào*, chúng ta nên xác định *vì sao*  

Hãy thử minh hoạ động lực và tiện ích của `this`:

```js
function identify() {
	return this.name.toUpperCase();
}

function speak() {
	var greeting = "Hello, I'm " + identify.call( this );
	console.log( greeting );
}

var me = {
	name: "Kyle"
};

var you = {
	name: "Reader"
};

identify.call( me ); // KYLE
identify.call( you ); // READER

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```

Nếu *như thế nào* của đoạn code này vẫn còn làm bạn hoang mang, đừng lo! Chúng ta sẽ tìm hiểu nó cụ thể ngay. Chỉ cần đặt câu hỏi này qua một bên, như vậy chúng ta có thể nhìn rõ *tại sao* hơn.

Đoạn trích này cho phép function `identify()` và `speak()` được tái sử dụng cho object nhiều *ngữ cảnh* (`me` và `you`) thay vì sử dụng nhiều phiên bản function khác nhau cho mỗi object.

Thay vì sử dụng `this`, bạn có thể thông qua một cách rõ ràng trong một object ngữ cảnh cho cả `identify()` và `speak()`.

```js
function identify(context) {
	return context.name.toUpperCase();
}

function speak(context) {
	var greeting = "Hello, I'm " + identify( context );
	console.log( greeting );
}

identify( you ); // READER
speak( me ); // Hello, I'm KYLE
```
Tuy nhiên, cơ chế `this` cung cấp phương thức tao nhã hơn theo kiểu hàm ý "vượt qua" một object liên quan, hướng tới thiết kế API rõ ràng hơn và dễ sử dụng hơn. 

Mô hình bạn sử dụng càng phức tạp, bạn sẽ càng thấy rõ việc dùng một tham số cụ thể tên tuổi thường rối rắm hơn dùng `this`. Khi bạn khám phá object & prototype, bạn sẽ thấy sự hữu dụng của một mớ function có thể tự động tham chiếu với object thích hợp.

## Các nhầm lẫn 

Chúng ta sẽ sớm giải thích `this` *thực sự* hoạt động ra sao, nhưng trước tiên chúng ta phải xoá tan những hiểu nhầm về những gì nó không thực sự đúng. 

Cái tên "this" tạo ra một số nhầm lẫn khi các lập trình viên cố gắng nghĩ về nó theo nghĩa đen quá. Có hai ý nghĩa thường đưa ra nhưng cả hai đều sai.

### Chính nó

Cám dỗ phổ biến đầu tiên là giả định bản thân `this` như một function. Ít nhất, đó là một suy luận có lý về mặt ngữ pháp.

Tại sao bạn muốn tham chiếu một function bên trong chính nó? Một số lý do phổ biến là thứ gì đó như kiểu đệ quy (gọi function từ bên trong nó) hoặc có một event handler mà có thể tự huỷ trong lần gọi đầu tiên.

Các lập trình viên mới làm quen với cơ chế của JS thường tham chiếu function như một object (tất cả các function trong JS đều là object!) cho phép bạn lưu trữ *state* (giá trị bên trong thuộc tính) khi gọi function. Mặc dù điều này là chắc chắn và có một số hạn chế sử dụng, phần còn lại của cuốn sách sẽ mô tả trên nhiều mô hình khác cho *vị trí* tốt hơn để lưu trữ state bên cạnh function object.

Ở đây chúng ta tìm hiểu vấn đề này một chút, để minh hoạ `this` không cho một function tham chiếu lên chính nó như chúng ta thường giả định như thế nào.

Xem đoạn code dưới đây, chúng ta thử theo dõi bao nhiêu lần function `foo` được gọi:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi bao nhiêu lần `foo` được gọi 
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// bao nhiêu lần `foo` được gọi?
console.log( foo.count ); // 0 -- WTF?
```

`foo.count` vẫn là `0`, mặc dù qua bốn lần `console.log` đã chỉ ra rõ ràng `foo(..)` là sự kiện được gọi bốn lần. Sự thất vọng bắt nguồn từ sự diễn dịch ý nghĩa *quá rõ ràng* của `this` (`this.count++`).

Khi `foo.count = 0` được thực thi, thực chất là nó thêm một thuộc tính `count` vào function object `foo`. Nhưng với `this.count` tham chiếu trong function, `this` không trỏ đến function object chút nào, và mặc dù tên thuộc tính giống nhau, object gốc lại khác nhau, và sự nhầm lẫn xảy ra.

**Ghi chú:** Một lập trình viên có trách nhiệm sẽ *phải* hỏi ở chỗ này "nếu tôi đã thêm một thuộc tính `count` nhưng nó lại không phải cái tôi mong muốn, vậy `count` mà tôi đã thêm là cái nào?" Thực tế, nếu cô ấy đào sâu thêm, cô sẽ thấy rằng vô tình cô đã tạo một biến toàn cục `count` (xem Chương 2 để biết nó đã xảy ra *như thế nào*), và hiện tại nó có giá trị `NaN`. Tất nhiên, một khi cô ta đã nhận thấy kết quả đặc biệt này, câu ta sẽ có một câu hỏi tổng quan hơn: "Nó toàn cục như thế nào? và tại sao nó lại kết thúc bằng `NaN` thay cho một giá trị đếm được?" (Xem Chương 2)

Thay vì dừng tại điểm này và đào sâu vào lý do tại sao tham chiếu `this` có vẻ không thoả *kỳ vọng*, và trả lời của những câu hỏi khó nhưng quan trọng này, nhiều lập trình viên đơn giản tránh lỗi này, và khám phá giải pháp khác, như là tạo một object để giữ giá trị `count`:  

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi `foo` được gọi bao nhiêu lần
	
	data.count++;
}

var data = {
	count: 0
};

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// bao nhiêu lần `foo` được gọi?
console.log( data.count ); // 4
```

Trong khi rõ ràng cách tiếp cận này "giải quyết" vấn đề, không may nó lại bỏ qua một vấn đề thực sự -- thiếu hiểu biết ý nghĩa của `this` và cách nó làm việc -- thay vào đó là rút lui vào vùng dễ chịu dưới một cơ chế gần gũi hơn: lexical scope  

**Chú ý:** Lexical scope là một cơ chế tốt và hững dụng; Trên mọi phương diện thì tôi không coi thường việc sử dụng nó (Xem *"Scope & Closures"*). Nhưng lúc nào cũng *đoán* sử dụng `this` như thế nào, hoặc thường xuyên hiểu *sai* thì đây không phải là lý do tốt để chuyển qua sử dụng lexical scope và không bao giờ học *vì sao* `this` lẩn tránh bạn.

Để tham chiếu một function object bên trong nó, bản thân `this` không đầy đủ, bạn cần có một tham chiếu đến function object thông qua một định danh gốc (biến) để trỏ vào nó. 

Xem 2 function dưới đây:

```js
function foo() {
	foo.count = 4; // `foo` tham chiếu đến chính nó
}

setTimeout( function(){
	// function vô danh (không tên), không thể tham chiếu đến chính nó.
}, 10 );
```

Trong function thứ nhất, gọi là "function có tên", `foo` là một tham chiếu có thể sử dụng để tham chiếu đến function bên trong nó.

Nhưng trong ví dụ thứ 2, function callback qua `setTimeout(..)` không có nhận dạng tên (còn gọi là "function vô danh"), do đó không có cách nào tham chiếu đến function object bởi chính nó.

**Ghi chú:** Có một phương thức cũ bị bãi bỏ là `arguments.callee` tham chiếu bên trong một function *cũng* trỏ về function object của function đang được thực thi. Các tham chiếu này thường là cách duy nhất để tiếp cận một function object vô danh bên trong nó. Tuy nhiên, cách tiếp cận tốt nhất là tránh sử dụng chức năng ẩn danh hoàn toàn, ít nhất là đối với các function cần một tham chiếu chính nó, thay vào đó là tạo function có tên (biểu thức). `arguments.callee` không được sử dụng nữa và cũng không nên sử dụng.

Do đó một giải pháp khác cho ví dụ của chúng ta là sử dụng nhận diện `foo` như một tham chiếu function object tại mỗi vị trí, và không sử dụng `this` nữa, nó sẽ hoạt động.

```js
function foo(num) {
	console.log( "foo: " + num );

	// Kiểm tra bao nhiêu lần `foo` được gọi 
	foo.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// Bao nhiêu lần `foo` được gọi?
console.log( foo.count ); // 4
```

Tuy nhiên, cách tiếp cận này các bước tương tự *thực sự* hiểu `this` và dựa hoàn toàn vào lexical scope của biến `foo`.

Cách khác để tiếp cận vấn đề là buộc `this` thực sự trỏ vào `foo` function object:


```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi bao nhiêu lần `foo` được gọi
	// Ghi chú: giờ `this` thực sự là `foo`, dựa vào
	// `foo` được gọi như thế nào (xem bên dưới)
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		// sử dụng `call(..)`, chúng ta chắc chắn `this`
		// tự nó trỏ vào function object (`foo`)
		foo.call( foo, i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// bao nhiêu lần `foo` được gọi?
console.log( foo.count ); // 4
```

**Thay vì tránh né `this`, chúng ta bao lấy nó.** Chúng ta sẽ giải thích các kỹ thuật này hoạt động tốt hơn như thế nào, vì vậy đừng lo lắng nếu bạn vẫn còn một chút lo lắng!

### Phạm vi (Scope) của nó

Quan niệm sai lầm phổ biến tiếp theo là ý nghĩa của `this` là nó bằng cách nào đó tham chiếu đến function scope. Đây là một câu hỏi khéo, vì xét về mặt nào đó nó là đúng, nhưng ở khía cạnh khác nó là sai.

Một cách rõ ràng thì `this` không tham chiếu đến một **lexical scope** của function. Đúng là xét về nội bộ, scope là một dạng giống như object có thuộc tính cho mỗi đối tượng định danh sẵn có. Nhưng scope "object" không truy cập vào code JavaScript. Nó là một phần bên trong của sự thực thi của *Engine*. 

Xem đoạn code đã cố (và thất bại!) vượt qua ranh giới và sử dụng `this` để ngầm tham chiếu vào lexical scope của function: 

```js
function foo() {
	var a = 2;
	this.bar();
}

function bar() {
	console.log( this.a );
}

foo(); //undefined
```

Có nhiều sai sót trong đoạn code này. Trong khi có vẻ như nó được ngoại lệ, đoạn code bạn thấy là sự trích tách từ một đoạn code thực tế được trao đổi trong các diễn đàn. Nó là một minh hoạ tuyệt vời (nếu không buồn) cho việc hiểu sai `this` cho giả định được đặt ra.

Đầu tiên, một sự cố gắng được tạo ra để tham chiếu `bar()` function thông qua `this.bar()`. Hầu như chắc chắn rằng *vô tình* nó hoạt động, nhưng chúng ta sẽ giải thích ngắn gọn *vì sao*. Cách tự nhiên nhất là `bar()` được gọi đã bỏ qua sự dẫn dắt của `this` và đã tạo ra một lexical tham chiếu sự nhận diện 

Tuy nhiên, lập trình viên nào đã viết đoạn code đang cố gắng sử dụng `this` để tạo ra một cái cầu giữa các lexical scope của `foo()` và `bar()`, từ đó `bar()` có thể tiếp cận biến `a` trong scope của `foo()`. **Chẳng có cái cầu xảy ra** Bạn không thể sử dụng `this` để tham chiếu và tìm một thứ gì đó trong lexical scope. Hoàn toàn không thể. 

Mỗi khi bạn cảm thấy mình muốn pha trộn việc tìm kiếm với `this`, luôn nhớ rằng: *không có cầu nào hết*

## `this` là gì?

Với các giả định sai sót khác nhau, giờ chúng ta chuyển qua việc chú ý cơ chế hoạt động thực sự của `this`. 

Chúng ta đã từng nói `this` không phải là một author-time binding mà là một runtime binding. 
We said earlier that `this` is not an author-time binding but a runtime binding. It is contextual based on the conditions of the function's invocation. `this` binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which the function is called.

When a function is invoked, an activation record, otherwise known as an execution context, is created. This record contains information about where the function was called from (the call-stack), *how* the function was invoked, what parameters were passed, etc. One of the properties of this record is the `this` reference which will be used for the duration of that function's execution.

In the next chapter, we will learn to find a function's **call-site** to determine how its execution will bind `this`.

## Review (TL;DR)

`this` binding is a constant source of confusion for the JavaScript developer who does not take the time to learn how the mechanism actually works. Guesses, trial-and-error, and blind copy-n-paste from Stack Overflow answers is not an effective or proper way to leverage *this* important `this` mechanism.

To learn `this`, you first have to learn what `this` is *not*, despite any assumptions or misconceptions that may lead you down those paths. `this` is neither a reference to the function itself, nor is it a reference to the function's *lexical* scope.

`this` is actually a binding that is made when a function is invoked, and *what* it references is determined entirely by the call-site where the function is called.
