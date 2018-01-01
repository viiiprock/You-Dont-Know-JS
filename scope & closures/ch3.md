# You Don't Know JS: Scope & Closures
# Chương 3: Hàm vs. Khối phạm vi

**Vài lời: Sau vài chương dịch thì tôi thấy văn phong tác giả có phần rườm rà, nhiều đoạn lặp lại không cần thiết, mặt khác thì tôi cũng bận nhiều việc nên từ chương này tôi sẽ dịch tóm lược những vấn đề quan trọng, không sa đà vào câu chữ của tác giả nữa**

Như chúng ta đã khám phá trong chương 2, scope bao gồm một tập hợp các "bong bóng", mỗi scope hoạt động như vật chứa trong đó xác định các định danh (biến, function). Các bong bóng tổ hợp (nesting) gọn gàng bên trong bong bóng khác, và tổ hợp này được xác định tại author-time.

Nhưng chính xác là cái gì tạo ra bong bóng mới? Có phải chỉ có hàm? Có cấu trúc nào khác tạo ra các bong bóng trong phạm vi?

## Phạm vi từ các Hàm
Câu trả lời phổ biến nhất cho các câu hỏi trên là JavaScript có scope nền function (function-based scope). Nghĩa là, mỗi function bạn khai báo sẽ tự tạo bong bóng, và không có bất kỳ cấu trúc nào khác tự tạo bong bóng. Nếu chúng ta để ý thì điều này không hoàn toàn đúng.

Trước hết, ta hãy khám phá function scope & các hàm ý của nó:

```js
function foo(a) {
	var b = 2;

	// some code

	function bar() {
		// ...
	}

	// more code

	var c = 3;
}
```


Trong đoạn trích này, bong bóng phạm vi của `foo(..)` bao gồm nhận dạng `a`, `b`, `c` và `bar`. Một khai báo xuất hiện *ở đâu* trong phạm vi **không quan trọng**, bất kể biến hoặc bong bóng phạm vi hàm chứa nó.

`bar(..)` có bong bóng của riêng nó. Phạm vi toàn cũng chỉ có một định danh gắn liền: `foo`

Bởi vì `a`, `b`, `c`, và `bar` đều thuộc về bong bóng phạm vi của `foo(..)`, nó sẽ không thể tiếp cận bên ngoài `foo(..)`. Dòng code dưới đây đều có kết quả lỗi `ReferenceError`, khi các định danh đều không có sẵn tại phạm vi toàn cục:

```js
bar(); // thất bại

console.log( a, b, c ); // cả 3 đều thất bại
```

Tuy vậy, các định danh (`a`, `b`, `c`, `foo`, và `bar`) đều có thể tiếp cận được bên trong `foo(..)`, và cũng hợp lệ bên trong `bar(..)` (giả định không có khai báo che bóng nào bên trong `bar(..)`).

Phạm vi hàm khuyến khích ý tưởng tất cả các biến thuộc về hàm, và có thể sử dụng/tái sử dụng xuyên suốt trong hàm đó (tiếp cận được trong phạm vi lồng nhau). Các thiết kế này khá là hữu dụng, và mang lại các biến JS sự "linh hoạt" khi lấy giá trị với các kiểu khác nhau khi cần.

Mặt khác nếu bạn không cẩn thận, biến đã tồn tại bên trong phạm vi có thể sẽ dẫn đến kết quả không mong muốn.

## Ẩn bên trong phạm vi

Theo cách nghĩ thông thường thì bạn khai báo hàm, viết code bên trong. Nếu nghĩ ngược lại thì sẽ thấy lợi thế: lấy một đoạn code đã viết bất kỳ, bao nó bằng một hàm khai báo mới, nó sẽ tạo hiệu ứng ẩn code.

Kết quả thực tế là tạo một phạm vi bong bóng quanh đoạn code, bất kỳ khai báo nào bên trong được gắn với phạm vi của hàm mới, tức là bạn có thể dấu code bên trong phạm vi của một hàm.

Lý do hữu ích của việc giấu code?

Xuất phát từ nguyên lý thiết kế phần mềm "Nguyên tắc ưu tiên thấp nhất", có khi gọi là "Quyền tối thiểu", hay "Phơi bày thấp nhất", thường dùng trong thiết kế API cho một module/object, chúng ta chỉ lộ những phần tối thiểu cần thiết và giấu những phần còn lại.

Nguyên tắc này dẫn đến việc lựa chọn phạm vi nào sẽ chứa biến và hàm. Nếu tất cả các biến và hàm đều ở toàn cục, nó sẽ truy cập được ở bất kỳ phạm vi lồng nhau. Tức là vi phạm nguyên tắc "tối thiểu ..."

Ví dụ:

```js
function doSomething(a) {
	b = a + doSomethingElse( a * 2 );

	console.log( b * 3 );
}

function doSomethingElse(a) {
	return a - 1;
}

var b;

doSomething( 2 ); // 15
```

Trong đoạn code trên, biến `b` và hàm `doSomethingElse(..)` ẩn đi chi tiết cách `doSomething(..)` hoạt động ra sao. Tạo phạm vi bao vây việc truy cập vào `b` và `doSomethingElse(..)` không chỉ không cần thiết mà còn có thể "nguy hiểm", nó có thể có kết quả không mong muốn, dù vô tình hay cố ý thì điều này cũng vi phạm nguyên tắc tiền giả định của `doSomething(..)`.

Thiết kế hợp lý hơn sẽ ẩn chi tiết bên trong phạm vi của `doSomething(..)`:

```js
function doSomething(a) {
	function doSomethingElse(a) {
		return a - 1;
	}

	var b;

	b = a + doSomethingElse( a * 2 );

	console.log( (b * 3) );
}

doSomething( 2 ); // 15
```

Giờ đây, `b` và `doSomethingElse(..)` không thể tiếp cận từ bên ngoài mà chỉ được điều khiển bởi `doSomething(..)`. Chi tiết được thiết kế biệt lập, hoạt động của hàm và kết quả không bị ảnh hưởng, nhưng thiết kế giữ cho chi tiết được ẩn đi, điều này sẽ làm cho ứng dụng tốt hơn.

### Tránh trùng lặp

Lợi ích khác của việc ẩn biến và hàm trong phạm vi là tránh được sự trùng lặp không chủ định giữa hai định danh cùng tên nhưng khác phương thức sử dụng. Việc trùng lặp sẽ ghi đè giá trị không mong muốn.

Ví dụ:

```js
function foo() {
	function bar(a) {
		i = 3; // thay đổi `i` bên trong phạm vi vòng lặp for
		console.log( a + i );
	}

	for (var i=0; i<10; i++) {
		bar( i * 2 ); // oops, lặp vô định!
	}
}

foo();
```
Phép gán `i = 3` bên trong `bar(..)` ghi đè, nhưng `i` lại được khai báo trong `foo(..)` trong vòng lặp for. Trường hợp này kết quả là vòng lặp vô tận, vì `i` được đặt giá trị "cứng" `3` và luôn luôn có giá trị `< 10`.

Phép gán bên trong `bar(..)` cần được khai bái một biến nội bộ để sử dụng, bất kể thên nào được chọn. `var i = 3;` sẽ sửa lỗi ( "biến đổ bóng" `i`). Phương án (không thay thế) khác là chọn định danh khác, chẳng hạn như `var j = 3;`. Nhưng việc thiết kế phần mềm tự nhiên sẽ có tên định danh giống nhau, cho nên việc ẩn khai báo bên trong là cách tốt nhất trong trường hợp này.

#### "Namespaces" toàn cục

Một ví dụ rõ ràng về việc va chạm biến xảy ra trong phạm vi toàn cục là khi nhiều thư viện nạp vào chương trình có thể dễ dàng va chạm nhau nếu bạn không ẩn biến và hàm.

Các thư viện như vậy thường tạo ra một biến duy nhất, thường là object, với một tên duy nhất trong toàn cục. Object này
sử dụng như là một "không gian tên" cho thư viện đó, các chức năng phơi bày như các thuộc tính của object.

Ví dụ:

```js
var MyReallyCoolLibrary = {
	awesome: "stuff",
	doSomething: function() {
		// ...
	},
	doAnotherThing: function() {
		// ...
	}
};
```

#### Quản lý module

Phương án khác hiện đại hơn để tránh va chạm là "module", sử dụng bất kỳ quản lý phụ thuộc. Theo cách này thì không có thư viện nào thêm vào định danh toàn cục, thay vào đó là phải nhập các nhận dạng vào một phạm vi cụ thể thông qua cơ chế khác nhau của bộ phận quả lý module phụ thuộc (dependecy manager).

Cần lưu ý là các công cụ này không có phương thức "thần thánh" nào để miễn quy tắc phạm vi từ vựng. Nó chỉ đơn giản sử dụng quy tắc đã giải thích là không có bất kỳ định danh chung nào trong phạm vi, thay vào đó là giữ cá thể, không va chạm, ngăn ngừa bất kỳ sự va chạm vô tình nào.

Xem Chương 5 để biết thêm về module pattern.

## Hàm đóng vai trò phạm vi

Chúng ta đã thấy rằng có thể lấy bất kỳ đoạn code và bao nó bằng một hàm, điều này tạo ra hiệu quả "giấu" bất kỳ biến hoặc hàm đã khai báo với phía bên ngoài phạm vi hoặc hàm khác nằm bên trong phạm vi.

```js
var a = 2;

function foo() { // <-- chèn
	var a = 3;
	console.log( a ); // 3

} // <--
foo(); // <-- và

console.log( a ); // 2
```

Đầu tiên là chúng ta phải khai báo một tên hàm `foo()`, nghĩa là bản thân định danh `foo` "làm bẩn" phạm vi bên trong, chúng ta đồng thời gọi tên hàm để nó thực thi.

Lý tưởng hơn nếu hàm không cần tên (hoặc tên không làm dơ phạm vi bên trong), và nếu hàm tự động thực thi.

May mắn là JavaScript có cả giải pháp cho hai vấn đề.

```js
var a = 2;

(function foo(){ // <-- thêm cái này

	var a = 3;
	console.log( a ); // 3

})(); // <-- và này

console.log( a ); // 2
```

Phân tích.

First, notice that the wrapping function statement starts with `(function...` as opposed to just `function...`. While this may seem like a minor detail, it's actually a major change. Instead of treating the function as a standard declaration, the function is treated as a function-expression.

**Note:** The easiest way to distinguish declaration vs. expression is the position of the word "function" in the statement (not just a line, but a distinct statement). If "function" is the very first thing in the statement, then it's a function declaration. Otherwise, it's a function expression.

The key difference we can observe here between a function declaration and a function expression relates to where its name is bound as an identifier.

Compare the previous two snippets. In the first snippet, the name `foo` is bound in the enclosing scope, and we call it directly with `foo()`. In the second snippet, the name `foo` is not bound in the enclosing scope, but instead is bound only inside of its own function.

In other words, `(function foo(){ .. })` as an expression means the identifier `foo` is found *only* in the scope where the `..` indicates, not in the outer scope. Hiding the name `foo` inside itself means it does not pollute the enclosing scope unnecessarily.

### Vô danh vs. Có tên

Bạn đã quen thuộc với biểu thức hàm là tham chiếu callback, như là:

```js
setTimeout( function(){
	console.log("I waited 1 second!");
}, 1000 );
```

Cái này được gọi là "biểu thức hàm vô danh", bởi vì `function()...` không có tên định danh. Function expressions can be anonymous, but function declarations cannot omit the name -- that would be illegal JS grammar.

Anonymous function expressions are quick and easy to type, and many libraries and tools tend to encourage this idiomatic style of code. However, they have several draw-backs to consider:

1. Anonymous functions have no useful name to display in stack traces, which can make debugging more difficult.

2. Without a name, if the function needs to refer to itself, for recursion, etc., the **deprecated** `arguments.callee` reference is unfortunately required. Another example of needing to self-reference is when an event handler function wants to unbind itself after it fires.

3. Anonymous functions omit a name that is often helpful in providing more readable/understandable code. A descriptive name helps self-document the code in question.

**Inline function expressions** are powerful and useful -- the question of anonymous vs. named doesn't detract from that. Providing a name for your function expression quite effectively addresses all these draw-backs, but has no tangible downsides. The best practice is to always name your function expressions:

```js
setTimeout( function timeoutHandler(){ // <-- Look, I have a name!
	console.log( "I waited 1 second!" );
}, 1000 );
```

### Invoking Function Expressions Immediately (IIFE - Gọi biểu thức hàm tức thì)

```js
var a = 2;

(function foo(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Now that we have a function as an expression by virtue of wrapping it in a `( )` pair, we can execute that function by adding another `()` on the end, like `(function foo(){ .. })()`. The first enclosing `( )` pair makes the function an expression, and the second `()` executes the function.

This pattern is so common, a few years ago the community agreed on a term for it: **IIFE**, which stands for **I**mmediately **I**nvoked **F**unction **E**xpression.

Of course, IIFE's don't need names, necessarily -- the most common form of IIFE is to use an anonymous function expression. While certainly less common, naming an IIFE has all the aforementioned benefits over anonymous function expressions, so it's a good practice to adopt.

```js
var a = 2;

(function IIFE(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

There's a slight variation on the traditional IIFE form, which some prefer: `(function(){ .. }())`. Look closely to see the difference. In the first form, the function expression is wrapped in `( )`, and then the invoking `()` pair is on the outside right after it. In the second form, the invoking `()` pair is moved to the inside of the outer `( )` wrapping pair.

These two forms are identical in functionality. **It's purely a stylistic choice which you prefer.**

Another variation on IIFE's which is quite common is to use the fact that they are, in fact, just function calls, and pass in argument(s).

For instance:

```js
var a = 2;

(function IIFE( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

})( window );

console.log( a ); // 2
```

We pass in the `window` object reference, but we name the parameter `global`, so that we have a clear stylistic delineation for global vs. non-global references. Of course, you can pass in anything from an enclosing scope you want, and you can name the parameter(s) anything that suits you. This is mostly just stylistic choice.

Another application of this pattern addresses the (minor niche) concern that the default `undefined` identifier might have its value incorrectly overwritten, causing unexpected results. By naming a parameter `undefined`, but not passing any value for that argument, we can guarantee that the `undefined` identifier is in fact the undefined value in a block of code:

```js
undefined = true; // setting a land-mine for other code! avoid!

(function IIFE( undefined ){

	var a;
	if (a === undefined) {
		console.log( "Undefined is safe here!" );
	}

})();
```

Still another variation of the IIFE inverts the order of things, where the function to execute is given second, *after* the invocation and parameters to pass to it. This pattern is used in the UMD (Universal Module Definition) project. Some people find it a little cleaner to understand, though it is slightly more verbose.

```js
var a = 2;

(function IIFE( def ){
	def( window );
})(function def( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

});
```

The `def` function expression is defined in the second-half of the snippet, and then passed as a parameter (also called `def`) to the `IIFE` function defined in the first half of the snippet. Finally, the parameter `def` (the function) is invoked, passing `window` in as the `global` parameter.

## Blocks As Scopes

While functions are the most common unit of scope, and certainly the most wide-spread of the design approaches in the majority of JS in circulation, other units of scope are possible, and the usage of these other scope units can lead to even better, cleaner to maintain code.

Many languages other than JavaScript support Block Scope, and so developers from those languages are accustomed to the mindset, whereas those who've primarily only worked in JavaScript may find the concept slightly foreign.

But even if you've never written a single line of code in block-scoped fashion, you are still probably familiar with this extremely common idiom in JavaScript:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

We declare the variable `i` directly inside the for-loop head, most likely because our *intent* is to use `i` only within the context of that for-loop, and essentially ignore the fact that the variable actually scopes itself to the enclosing scope (function or global).

That's what block-scoping is all about. Declaring variables as close as possible, as local as possible, to where they will be used. Another example:

```js
var foo = true;

if (foo) {
	var bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}
```

We are using a `bar` variable only in the context of the if-statement, so it makes a kind of sense that we would declare it inside the if-block. However, where we declare variables is not relevant when using `var`, because they will always belong to the enclosing scope. This snippet is essentially "fake" block-scoping, for stylistic reasons, and relying on self-enforcement not to accidentally use `bar` in another place in that scope.

Block scope is a tool to extend the earlier "Principle of Least ~~Privilege~~ Exposure" [^note-leastprivilege] from hiding information in functions to hiding information in blocks of our code.

Consider the for-loop example again:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Why pollute the entire scope of a function with the `i` variable that is only going to be (or only *should be*, at least) used for the for-loop?

But more importantly, developers may prefer to *check* themselves against accidentally (re)using variables outside of their intended purpose, such as being issued an error about an unknown variable if you try to use it in the wrong place. Block-scoping (if it were possible) for the `i` variable would make `i` available only for the for-loop, causing an error if `i` is accessed elsewhere in the function. This helps ensure variables are not re-used in confusing or hard-to-maintain ways.

But, the sad reality is that, on the surface, JavaScript has no facility for block scope.

That is, until you dig a little further.

### `with`

We learned about `with` in Chapter 2. While it is a frowned upon construct, it *is* an example of (a form of) block scope, in that the scope that is created from the object only exists for the lifetime of that `with` statement, and not in the enclosing scope.

### `try/catch`

It's a *very* little known fact that JavaScript in ES3 specified the variable declaration in the `catch` clause of a `try/catch` to be block-scoped to the `catch` block.

For instance:

```js
try {
	undefined(); // illegal operation to force an exception!
}
catch (err) {
	console.log( err ); // works!
}

console.log( err ); // ReferenceError: `err` not found
```

As you can see, `err` exists only in the `catch` clause, and throws an error when you try to reference it elsewhere.

**Note:** While this behavior has been specified and true of practically all standard JS environments (except perhaps old IE), many linters seem to still complain if you have two or more `catch` clauses in the same scope which each declare their error variable with the same identifier name. This is not actually a re-definition, since the variables are safely block-scoped, but the linters still seem to, annoyingly, complain about this fact.

To avoid these unnecessary warnings, some devs will name their `catch` variables `err1`, `err2`, etc. Other devs will simply turn off the linting check for duplicate variable names.

The block-scoping nature of `catch` may seem like a useless academic fact, but see Appendix B for more information on just how useful it might be.

### `let`

Thus far, we've seen that JavaScript only has some strange niche behaviors which expose block scope functionality. If that were all we had, and *it was* for many, many years, then block scoping would not be terribly useful to the JavaScript developer.

Fortunately, ES6 changes that, and introduces a new keyword `let` which sits alongside `var` as another way to declare variables.

The `let` keyword attaches the variable declaration to the scope of whatever block (commonly a `{ .. }` pair) it's contained in. In other words, `let` implicitly hijacks any block's scope for its variable declaration.

```js
var foo = true;

if (foo) {
	let bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}

console.log( bar ); // ReferenceError
```

Using `let` to attach a variable to an existing block is somewhat implicit. It can confuse if you're not paying close attention to which blocks have variables scoped to them, and are in the habit of moving blocks around, wrapping them in other blocks, etc., as you develop and evolve code.

Creating explicit blocks for block-scoping can address some of these concerns, making it more obvious where variables are attached and not. Usually, explicit code is preferable over implicit or subtle code. This explicit block-scoping style is easy to achieve, and fits more naturally with how block-scoping works in other languages:

```js
var foo = true;

if (foo) {
	{ // <-- explicit block
		let bar = foo * 2;
		bar = something( bar );
		console.log( bar );
	}
}

console.log( bar ); // ReferenceError
```

We can create an arbitrary block for `let` to bind to by simply including a `{ .. }` pair anywhere a statement is valid grammar. In this case, we've made an explicit block *inside* the if-statement, which may be easier as a whole block to move around later in refactoring, without affecting the position and semantics of the enclosing if-statement.

**Note:** For another way to express explicit block scopes, see Appendix B.

In Chapter 4, we will address hoisting, which talks about declarations being taken as existing for the entire scope in which they occur.

However, declarations made with `let` will *not* hoist to the entire scope of the block they appear in. Such declarations will not observably "exist" in the block until the declaration statement.

```js
{
   console.log( bar ); // ReferenceError!
   let bar = 2;
}
```

#### Garbage Collection

Another reason block-scoping is useful relates to closures and garbage collection to reclaim memory. We'll briefly illustrate here, but the closure mechanism is explained in detail in Chapter 5.

Consider:

```js
function process(data) {
	// do something interesting
}

var someReallyBigData = { .. };

process( someReallyBigData );

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

The `click` function click handler callback doesn't *need* the `someReallyBigData` variable at all. That means, theoretically, after `process(..)` runs, the big memory-heavy data structure could be garbage collected. However, it's quite likely (though implementation dependent) that the JS engine will still have to keep the structure around, since the `click` function has a closure over the entire scope.

Block-scoping can address this concern, making it clearer to the engine that it does not need to keep `someReallyBigData` around:

```js
function process(data) {
	// do something interesting
}

// anything declared inside this block can go away after!
{
	let someReallyBigData = { .. };

	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

Declaring explicit blocks for variables to locally bind to is a powerful tool that you can add to your code toolbox.

#### `let` Loops

A particular case where `let` shines is in the for-loop case as we discussed previously.

```js
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```

Not only does `let` in the for-loop header bind the `i` to the for-loop body, but in fact, it **re-binds it** to each *iteration* of the loop, making sure to re-assign it the value from the end of the previous loop iteration.

Here's another way of illustrating the per-iteration binding behavior that occurs:

```js
{
	let j;
	for (j=0; j<10; j++) {
		let i = j; // re-bound for each iteration!
		console.log( i );
	}
}
```

The reason why this per-iteration binding is interesting will become clear in Chapter 5 when we discuss closures.

Because `let` declarations attach to arbitrary blocks rather than to the enclosing function's scope (or global), there can be gotchas where existing code has a hidden reliance on function-scoped `var` declarations, and replacing the `var` with `let` may require additional care when refactoring code.

Consider:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	if (baz > bar) {
		console.log( baz );
	}

	// ...
}
```

This code is fairly easily re-factored as:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	// ...
}

if (baz > bar) {
	console.log( baz );
}
```

But, be careful of such changes when using block-scoped variables:

```js
var foo = true, baz = 10;

if (foo) {
	let bar = 3;

	if (baz > bar) { // <-- don't forget `bar` when moving!
		console.log( baz );
	}
}
```

See Appendix B for an alternate (more explicit) style of block-scoping which may provide easier to maintain/refactor code that's more robust to these scenarios.

### `const`

In addition to `let`, ES6 introduces `const`, which also creates a block-scoped variable, but whose value is fixed (constant). Any attempt to change that value at a later time results in an error.

```js
var foo = true;

if (foo) {
	var a = 2;
	const b = 3; // block-scoped to the containing `if`

	a = 3; // just fine!
	b = 4; // error!
}

console.log( a ); // 3
console.log( b ); // ReferenceError!
```

## Review (TL;DR)

Functions are the most common unit of scope in JavaScript. Variables and functions that are declared inside another function are essentially "hidden" from any of the enclosing "scopes", which is an intentional design principle of good software.

But functions are by no means the only unit of scope. Block-scope refers to the idea that variables and functions can belong to an arbitrary block (generally, any `{ .. }` pair) of code, rather than only to the enclosing function.

Starting with ES3, the `try/catch` structure has block-scope in the `catch` clause.

In ES6, the `let` keyword (a cousin to the `var` keyword) is introduced to allow declarations of variables in any arbitrary block of code. `if (..) { let a = 2; }` will declare a variable `a` that essentially hijacks the scope of the `if`'s `{ .. }` block and attaches itself there.

Though some seem to believe so, block scope should not be taken as an outright replacement of `var` function scope. Both functionalities co-exist, and developers can and should use both function-scope and block-scope techniques where respectively appropriate to produce better, more readable/maintainable code.

[^note-leastprivilege]: [Principle of Least Privilege](http://en.wikipedia.org/wiki/Principle_of_least_privilege)
