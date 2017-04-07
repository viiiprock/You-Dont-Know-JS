# You Don't Know JS: *this* & Object Prototypes
# Chương 1: `this` hay  That?

Một trong những cơ chế hãi hùng nhất của JavaScript là từ khoá `this`. Nó là một từ khoá nhận dạng đặc biệt tự động xác định trong scope của mọi function, nhưng những gì chính xác nó đề cập đến còn  làm điêu đứng ngay cả JavaScript dev dày dạn.
> Any sufficiently *advanced* technology is indistinguishable from magic. -- Arthur C. Clarke

Cơ chế của `this` không hẳn ghê gớm như vậy, nhưng các lập trình viên thường diễn giải trong tâm trí bằng cách thêm từ "phức tạp" hoặc "mơ hồ", và không có câu hỏi nào ngoài việc hiểu rõ ràng, `this` có thể xem là sự huyền diệu trong sự hoang của bạn. 

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

Nhưng chúng ta tìm hiểu nó một chút ở đây, để minh hoạ `this` không cho một function tham chiếu lên chính nó như chúng ta thường giả định như thế nào.

Xem đoạn code dưới đây, chúng ta thử theo dõi bao nhiêu lần function `foo` được gọi:

```js
function foo(num) {
	console.log( "foo: " + num );

	// theo dõi bao nhiêu lần `foo` được gọi trong this.count++;
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

While it is true that this approach "solves" the problem, unfortunately it simply ignores the real problem -- lack of understanding what `this` means and how it works -- and instead falls back to the comfort zone of a more familiar mechanism: lexical scope.

**Note:** Lexical scope is a perfectly fine and useful mechanism; I am not belittling the use of it, by any means (see *"Scope & Closures"* title of this book series). But constantly *guessing* at how to use `this`, and usually being *wrong*, is not a good reason to retreat back to lexical scope and never learn *why* `this` eludes you.

To reference a function object from inside itself, `this` by itself will typically be insufficient. You generally need a reference to the function object via a lexical identifier (variable) that points at it.

Consider these two functions:

```js
function foo() {
	foo.count = 4; // `foo` refers to itself
}

setTimeout( function(){
	// anonymous function (no name), cannot
	// refer to itself
}, 10 );
```

In the first function, called a "named function", `foo` is a reference that can be used to refer to the function from inside itself.

But in the second example, the function callback passed to `setTimeout(..)` has no name identifier (so called an "anonymous function"), so there's no proper way to refer to the function object itself.

**Note:** The old-school but now deprecated and frowned-upon `arguments.callee` reference inside a function *also* points to the function object of the currently executing function. This reference is typically the only way to access an anonymous function's object from inside itself. The best approach, however, is to avoid the use of anonymous functions altogether, at least for those which require a self-reference, and instead use a named function (expression). `arguments.callee` is deprecated and should not be used.

So another solution to our running example would have been to use the `foo` identifier as a function object reference in each place, and not use `this` at all, which *works*:

```js
function foo(num) {
	console.log( "foo: " + num );

	// keep track of how many times `foo` is called
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

// how many times was `foo` called?
console.log( foo.count ); // 4
```

However, that approach similarly side-steps *actual* understanding of `this` and relies entirely on the lexical scoping of variable `foo`.

Yet another way of approaching the issue is to force `this` to actually point at the `foo` function object:

```js
function foo(num) {
	console.log( "foo: " + num );

	// keep track of how many times `foo` is called
	// Note: `this` IS actually `foo` now, based on
	// how `foo` is called (see below)
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		// using `call(..)`, we ensure the `this`
		// points at the function object (`foo`) itself
		foo.call( foo, i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 4
```

**Instead of avoiding `this`, we embrace it.** We'll explain in a little bit *how* such techniques work much more completely, so don't worry if you're still a bit confused!

### Its Scope

The next most common misconception about the meaning of `this` is that it somehow refers to the function's scope. It's a tricky question, because in one sense there is some truth, but in the other sense, it's quite misguided.

To be clear, `this` does not, in any way, refer to a function's **lexical scope**. It is true that internally, scope is kind of like an object with properties for each of the available identifiers. But the scope "object" is not accessible to JavaScript code. It's an inner part of the *Engine*'s implementation.

Consider code which attempts (and fails!) to cross over the boundary and use `this` to implicitly refer to a function's lexical scope:

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

There's more than one mistake in this snippet. While it may seem contrived, the code you see is a distillation of actual real-world code that has been exchanged in public community help forums. It's a wonderful (if not sad) illustration of just how misguided `this` assumptions can be.

Firstly, an attempt is made to reference the `bar()` function via `this.bar()`. It is almost certainly an *accident* that it works, but we'll explain the *how* of that shortly. The most natural way to have invoked `bar()` would have been to omit the leading `this.` and just make a lexical reference to the identifier.

However, the developer who writes such code is attempting to use `this` to create a bridge between the lexical scopes of `foo()` and `bar()`, so that `bar()` has access to the variable `a` in the inner scope of `foo()`. **No such bridge is possible.** You cannot use a `this` reference to look something up in a lexical scope. It is not possible.

Every time you feel yourself trying to mix lexical scope look-ups with `this`, remind yourself: *there is no bridge*.

## What's `this`?

Having set aside various incorrect assumptions, let us now turn our attention to how the `this` mechanism really works.

We said earlier that `this` is not an author-time binding but a runtime binding. It is contextual based on the conditions of the function's invocation. `this` binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which the function is called.

When a function is invoked, an activation record, otherwise known as an execution context, is created. This record contains information about where the function was called from (the call-stack), *how* the function was invoked, what parameters were passed, etc. One of the properties of this record is the `this` reference which will be used for the duration of that function's execution.

In the next chapter, we will learn to find a function's **call-site** to determine how its execution will bind `this`.

## Review (TL;DR)

`this` binding is a constant source of confusion for the JavaScript developer who does not take the time to learn how the mechanism actually works. Guesses, trial-and-error, and blind copy-n-paste from Stack Overflow answers is not an effective or proper way to leverage *this* important `this` mechanism.

To learn `this`, you first have to learn what `this` is *not*, despite any assumptions or misconceptions that may lead you down those paths. `this` is neither a reference to the function itself, nor is it a reference to the function's *lexical* scope.

`this` is actually a binding that is made when a function is invoked, and *what* it references is determined entirely by the call-site where the function is called.
