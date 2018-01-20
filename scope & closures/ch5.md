# You Don't Know JS: Scope & Closures
# Chương 5: Scope closures (phạm vi bao đóng/đóng kín)

Tới chương này thì ta đã nắm rõ scope hoạt động như thế nào rồi.

Giờ chúng ta chuyển qua chú ý đến một vấn đề vô cùng quan trọng, nhưng cũng rất khó nắm bắt, là một phần của ngôn ngữ và *gần như là truyền thuyết*: **closure**. Theo như những gì ta tìm hiểu về lexical scope cho đến nay, thì lợi ích của closure là hiển nhiên. Nhưng *có một người đằng sau bức màn bí mật, chúng ta sẽ phải tìm anh ta*. Không, anh ta không phải Crockford (cha đẻ JS - người dịch)!

Tuy nhiên, nếu bạn còn thắc mắc về lexical scope, tốt nhất là nên quay về Chương 2 trước khi tiếp tục.

## Khai sáng

Với những người có kinh nghiệm với JavaScript, nhưng có lẽ chưa bao giờ nắm bắt đầy đủ khái niệm của closure, việc *hiểu closure* có thể coi như là đạt cảnh giới niết bàn cần phải có sự phấn đấu và hy sinh mới đạt được.

Tôi nhớ vài năm trước, khi tôi đã nắm vững JavaScript, nhưng vẫn không hiểu closure là gì. Có một gợi ý rằng ở *phía bên kia* của ngôn ngữ, có nhiều hứa hẹn hơn những gì tôi có. Tôi nhớ tôi đã đọc hết source code của một framework và cố gắng hiểu nó hoạt động ra sao. Tôi nhớ lần đầu tiên cái gì đó như "module pattern" đã gợi lên trong tâm trí. Tôi nhớ những khoảnh khắc *a-ha!*.

Những gì tôi đã không biết sau đó, cái gì đã khiến tôi cả năm để hiểu, và những gì tôi hy vọng truyền đặt được cho bạn là bí mất: **closure luôn ở xung quanh JavaScript, bạn phải nhận ra và nắm bắt lấy nó.** Closure không phải công cụ đặc biệt mà bạn phải học thêm về cú pháp và pattern. Không, closure thậm chí cũng không phải vũ khí mà bạn phải học cách làm chủ như Luke đã luyện trong The Force (xem Star wars - người dịch).

Closure xảy ra như là kết quả của viết code dựa trên lexical scope. Đôi khi là nó chỉ xảy ra. Thậm chí bạn còn không thực sự có ý định tạo closure để tận dụng lợi thế của chúng. Closure được tạo ra suốt quá trình code. Những gì bạn *đang thiếu* là bối cảnh để nhận ra, nắm bắt và dùng như đòn bẩy cho ý riêng.

Giây phút khai sáng hẳn là: **oh, closure thực sự xuất hiện suốt trong code của mình, giờ mình có thể thấy chúng** Hiểu closure giống như Neo thấy Ma trận lần đầu.

## Thực chất của vấn đề

OK, tán phét đủ rồi.

Giờ là những gì bạn cần để biết, hiểu, nắm bắt closure:

> Closure là khi một hàm có khả năng nhớ và truy cập lexical scope của nó ngay cả khi hàm đó được thực thi bên ngoài lexical scope của nó.

Hãy xem vài đoạn code minh họa.

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
```

Đoạn code này nhìn tương tự như trong phần Nested Scope (phạm vi lồng nhau). Hàm `bar()` *truy cập* đến biến `a`
ở trong phạm vi bao ngoài vì quy tắc tìm kiếm lexical scope (trường hợp này là một tìm kiếm RHS)

Đây phải "closure" không?

Chà, về kỹ thuật thì... *có lẽ*. Nhưng những gì bạn cần biết như ở trên thì...*không chính xác*. Tôi nghĩ cách chính xác nhất để giải thích `bar()` tham chiếu `a` thông qua quy tắc tìm kiếm lexical scope, và các quy tắc đó *chỉ* (quan trọng!) là **một phần** của closure.

Từ góc độ hàn lâm thuần túy, hàm `bar()` ở trên được giải thích là nó có *closure* trong phạm vi của `foo()` (và thậm chí thực ra là trong phần con lại của scope nó có quyền truy cập đến, phạm vi toàn cục chẳng hạn). Còn nói hơi khác thì `bar()` đóng kín trong phạm vi của `foo()`. Vì sao? Vì `bar()` xuất hiện lồng bên trong `foo()`, đơn giản thẳng đuột ruột ngựa.

Nhưng, xác định closure theo cách này không trực tiếp *observable (quan sát được)*, và chúng ta cũng không thấy closure *thể hiện* gì trong đoạn code trên. Ta thấy rõ lexical scope, nhưng closure vẫn là gì đó ẩn sâu bên trong.

Hãy xem đoạn code mang closure ra ngoài ánh sáng:

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar; // lấy hàm bar như một giá trị và trả về cho hàm foo
}

var baz = foo();

baz(); // 2 -- Whoa, thấy closure rồi nha các mẹ.
```

Hàm `bar()` có lexical scope truy cập vào scope của `foo()`. Nhưng sau đó, ta lấy chính bản thân hàm `bar()`, và truyền nó đi như một giá trị. Trường hợp này, ta `return` chính hàm `bar` như một tham chiếu.

Sau khi thực thi `foo()`, chúng ta gán giá trị trả về (hàm `bar()` bên trong) cho một biến gọi là `baz`, sau đó chúng ta gọi `baz()`, và dĩ nhiên nó gọi hàm bên trong `bar()`, chẳng qua là theo cách nhận diện tham chiếu khác mà thôi.

Chắc chắn `bar()` được thực thi. Nhưng trong trường hợp này, nó lại thực thi *bên ngoài* lexical scope mà nó đã khai báo.

Sau khi `foo()` thực thi, thông thường chúng ta cho rằng toàn bộ scope bên trong của `foo()` sẽ ra đi, vì *Engine* sử dụng công cụ *Gom rác* đi kèm và giải phóng bộ nhớ khi nó không còn sử dụng. Vì dường như nội dung của `foo()` đã không còn sử dụng, nó có thể coi là *mất*.

Nhưng "điều kỳ diệu" của closusre không để điều này xảy ra. Nghĩa là phần bên trong scope vẫn đang "sử dụng", không đi đâu hết. Ai dùng nó? **Chính hàm `bar()`**.

Vì `bar()` đã được khai báo nên nó có lexical scope closure qua phạm vi bên trong của `foo()`, việc này đã giúp cho `bar()` tồn tại trong việc tham chiếu về sau.

**`bar()` vẫn có một tham chiếu đến phạm vi, và điều này được gọi là closure.**

Vì vậy, vài microseconds sau đó, khi biến `baz` được gọi (gọi hàm `bar` bên trong), nó có quyền truy cập đến author-time của lexical scope, nên nó có thể tiếp cận biến `a` như ta mong muốn.

Hàm được gọi ngon lành cành đào từ bên ngoài của author-time lexical scope. **Closure** cho phép hàm tiếp tục truy cập lexical scope đã xác định tại author-time.

Tất nhiên bất kỳ phương cách nào mà hàm có thể *truyền đi xung quanh* như một giá trị, và được viện dẫn ở chỗ khác, đều là mô tả của việc observe/thực hiện closure.

```js
function foo() {
	var a = 2;

	function baz() {
		console.log( a ); // 2
	}

	bar( baz );
}

function bar(fn) {
	fn(); // cha mạ ơi, tui thấy closure !
}
```

Ta truyền hàm `baz` bên trong cho `bar`, và gọi hàm đó (tức là `fn`), và như vậy closure của nó qua phạm vi bên trong `foo()` được observe bằng cách truy cập `a`.

Cách truyền hàm như vậy có thể theo cách gián tiếp.

```js
var fn;

function foo() {
	var a = 2;

	function baz() {
		console.log( a );
	}

	fn = baz; // gán `baz` cho biến toàn cục
}

function bar() {
	fn(); // closure đây nè!
}

foo();

bar(); // 2
```

Dù chúng ta sử dụng phương tiện gì để *vận chuyển* hàm bên trong ra ngoài lexical scope, nó vẫn giữ một tham chiếu phạm vi tại nơi nó được khai báo, và dù ta thực thi nó ở đâu, closure sẽ được thực hiện.

## Giờ tôi đã thấy

Các đoạn code ở trên cũng chỉ để minh họa cấu trúc của việc sử dụng closure. Nhưng tôi hứa là nó còn hơn cả đồ chơi mới. Tôi hứa là closure nó luôn hiện hữu xung quanh code. Hãy xem đoạn code sau:

```js
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "Hello, closure!" );
```

Ta lấy hàm bên trong (`timer`) và truyền nó vào `setTimeout(..)`. Nhưng `timer` có một phạm vi closure qua `wait()`, và giữ một tham chiếu đến biến `message`.

Một ngàn mili giây sau khi ta thực thi `wait()`, scope bên trong nó sẽ thay vì mất đi, nhưng hàm bên trong `timer` vẫn có closure trong phạm vi.

Đi sâu vào mấu chốt của *Engine*, hàm tiện ích dựng sẵn `setTimeout(..)` tham chiếu đến vài tham số, tạm gọi là `fn` hay `func` hay đại loại tương tự vậy. *Engine* sẽ gọi hàm đó, nghĩa là hàm `timer` bên trong được gọi, và tham chiếu lexical scope vẫn nguyên vẹn.

**Closure.**

Nếu bạn đã dùng jQuery (hay bất kỳ JS framework nào):
```js
function setupBot(name,selector) {
	$( selector ).click( function activator(){
		console.log( "Activating: " + name );
	} );
}

setupBot( "Closure Bot 1", "#bot_1" );
setupBot( "Closure Bot 2", "#bot_2" );
```

Tôi không chắc bạn viết code kiểu nào, còn tôi thì thường viết code có trách nhiệm điều khiển toàn bộ "trang bị vũ trang" của closure, nên ví dụ trên là thực tiễn!

Về cơ bản, bất kỳ *khi nào* và *tại đâu* bạn xử lý hàm (hàm truy vập vào lexical scope riêng của chúng) như một giá trị hạng nhất và truyền chúng đâu đó, bạn có thể thấy hàm đó thực hiện closure. Là timers, event handler, gọi Ajax, web worker, cửa sổ thông báo, hay bất kỳ nhiệm vụ động bộ, bất đồng bộ, khi bạn truyền trong một hàm *callback*, tức là đã dùng closure.

**Ghi chú:** Chương 3 đã giới thiệu mẫu IIFE. Có người thường hay bảo bản thân IIFE là một ví dụ cho thực hiện closure, dựa theo xác định ở trên thì tôi có ý kiến không đồng ý lắm.

```js
var a = 2;

(function IIFE(){
	console.log( a );
})();
```

Code này hoạt động, nhưng nó hoàn toàn không phải là một observation của closure. Vì sao? bởi vì hàm `IIFE` không được thực thi bên ngoài lexical scope. Nó vẫn gọi ngay trong cùng một scope mà nó đã được khai báo (scope bên ngoài có biến `a`). `a` được tìm thế theo cách tra cứu lexical scope thông thường, không phải là qua closure.

Về mặt kỹ thuật, trong khi closure xảy ra ở thời điểm khai báo, IIFE không phải như vậy.

Mặc dù bản thân IIFE không phải là ví dụ của closure, nó lại tạo ra scope, và nó là một trong những công cụ thông thường nhất để ta tạo ra scope. Vì vậy IIFE có mỗi liên hệ mật thiết với closure, mặc dù bản thân nó không thực hiện closure.

Và giờ thì dừng đọc đi mấy bạn, tôi có nhiệm vụ cho bạn đây. Giờ bạn mở code của bạn lên, coi coi có hàm nào là closure hay giá trị là hàm không.


Giờ thì rõ rồi nhé!

## Vòng lặp + Closure

Ví dụ điển hình để minh họa closure là vòng lặp for.

```js
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

**Ghi chú:** Linter (trình dò lỗi JavaScript - người dịch) thường cảnh báo khi bạn đưa hàm vào trong vòng lặp do nhiều lập trình viên chưa nắm được closure. Tôi sẽ giải thích làm thế nào tận dụng toàn bộ sức mạnh của closure ở đây.


  Linters often complain when you put functions inside of loops, because the mistakes of not understanding closure are **so common among developers**. We explain how to do so properly here, leveraging the full power of closure. But that subtlety is often lost on linters and they will complain regardless, assuming you don't *actually* know what you're doing.

The spirit of this code snippet is that we would normally *expect* for the behavior to be that the numbers "1", "2", .. "5" would be printed out, one at a time, one per second, respectively.

In fact, if you run this code, you get "6" printed out 5 times, at the one-second intervals.

**Huh?**

Firstly, let's explain where `6` comes from. The terminating condition of the loop is when `i` is *not* `<=5`. The first time that's the case is when `i` is 6. So, the output is reflecting the final value of the `i` after the loop terminates.

This actually seems obvious on second glance. The timeout function callbacks are all running well after the completion of the loop. In fact, as timers go, even if it was `setTimeout(.., 0)` on each iteration, all those function callbacks would still run strictly after the completion of the loop, and thus print `6` each time.

But there's a deeper question at play here. What's *missing* from our code to actually have it behave as we semantically have implied?

What's missing is that we are trying to *imply* that each iteration of the loop "captures" its own copy of `i`, at the time of the iteration. But, the way scope works, all 5 of those functions, though they are defined separately in each loop iteration, all **are closed over the same shared global scope**, which has, in fact, only one `i` in it.

Put that way, *of course* all functions share a reference to the same `i`. Something about the loop structure tends to confuse us into thinking there's something else more sophisticated at work. There is not. There's no difference than if each of the 5 timeout callbacks were just declared one right after the other, with no loop at all.

OK, so, back to our burning question. What's missing? We need more ~~cowbell~~ closured scope. Specifically, we need a new closured scope for each iteration of the loop.

We learned in Chapter 3 that the IIFE creates scope by declaring a function and immediately executing it.

Let's try:

```js
for (var i=1; i<=5; i++) {
	(function(){
		setTimeout( function timer(){
			console.log( i );
		}, i*1000 );
	})();
}
```

Does that work? Try it. Again, I'll wait.

I'll end the suspense for you. **Nope.** But why? We now obviously have more lexical scope. Each timeout function callback is indeed closing over its own per-iteration scope created respectively by each IIFE.

It's not enough to have a scope to close over **if that scope is empty**. Look closely. Our IIFE is just an empty do-nothing scope. It needs *something* in it to be useful to us.

It needs its own variable, with a copy of the `i` value at each iteration.

```js
for (var i=1; i<=5; i++) {
	(function(){
		var j = i;
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})();
}
```

**Eureka! It works!**

A slight variation some prefer is:

```js
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})( i );
}
```

Of course, since these IIFEs are just functions, we can pass in `i`, and we can call it `j` if we prefer, or we can even call it `i` again. Either way, the code works now.

The use of an IIFE inside each iteration created a new scope for each iteration, which gave our timeout function callbacks the opportunity to close over a new scope for each iteration, one which had a variable with the right per-iteration value in it for us to access.

Problem solved!

### Block Scoping Revisited

Look carefully at our analysis of the previous solution. We used an IIFE to create new scope per-iteration. In other words, we actually *needed* a per-iteration **block scope**. Chapter 3 showed us the `let` declaration, which hijacks a block and declares a variable right there in the block.

**It essentially turns a block into a scope that we can close over.** So, the following awesome code "just works":

```js
for (var i=1; i<=5; i++) {
	let j = i; // yay, block-scope for closure!
	setTimeout( function timer(){
		console.log( j );
	}, j*1000 );
}
```

*But, that's not all!* (in my best Bob Barker voice). There's a special behavior defined for `let` declarations used in the head of a for-loop. This behavior says that the variable will be declared not just once for the loop, **but each iteration**. And, it will, helpfully, be initialized at each subsequent iteration with the value from the end of the previous iteration.

```js
for (let i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

How cool is that? Block scoping and closure working hand-in-hand, solving all the world's problems. I don't know about you, but that makes me a happy JavaScripter.

## Modules

There are other code patterns which leverage the power of closure but which do not on the surface appear to be about callbacks. Let's examine the most powerful of them: *the module*.

```js
function foo() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}
}
```

As this code stands right now, there's no observable closure going on. We simply have some private data variables `something` and `another`, and a couple of inner functions `doSomething()` and `doAnother()`, which both have lexical scope (and thus closure!) over the inner scope of `foo()`.

But now consider:

```js
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
}

var foo = CoolModule();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

This is the pattern in JavaScript we call *module*. The most common way of implementing the module pattern is often called "Revealing Module", and it's the variation we present here.

Let's examine some things about this code.

Firstly, `CoolModule()` is just a function, but it *has to be invoked* for there to be a module instance created. Without the execution of the outer function, the creation of the inner scope and the closures would not occur.

Secondly, the `CoolModule()` function returns an object, denoted by the object-literal syntax `{ key: value, ... }`. The object we return has references on it to our inner functions, but *not* to our inner data variables. We keep those hidden and private. It's appropriate to think of this object return value as essentially a **public API for our module**.

This object return value is ultimately assigned to the outer variable `foo`, and then we can access those property methods on the API, like `foo.doSomething()`.

**Note:** It is not required that we return an actual object (literal) from our module. We could just return back an inner function directly. jQuery is actually a good example of this. The `jQuery` and `$` identifiers are the public API for the jQuery "module", but they are, themselves, just a function (which can itself have properties, since all functions are objects).

The `doSomething()` and `doAnother()` functions have closure over the inner scope of the module "instance" (arrived at by actually invoking `CoolModule()`). When we transport those functions outside of the lexical scope, by way of property references on the object we return, we have now set up a condition by which closure can be observed and exercised.

To state it more simply, there are two "requirements" for the module pattern to be exercised:

1. There must be an outer enclosing function, and it must be invoked at least once (each time creates a new module instance).

2. The enclosing function must return back at least one inner function, so that this inner function has closure over the private scope, and can access and/or modify that private state.

An object with a function property on it alone is not *really* a module. An object which is returned from a function invocation which only has data properties on it and no closured functions is not *really* a module, in the observable sense.

The code snippet above shows a standalone module creator called `CoolModule()` which can be invoked any number of times, each time creating a new module instance. A slight variation on this pattern is when you only care to have one instance, a "singleton" of sorts:

```js
var foo = (function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
})();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

Here, we turned our module function into an IIFE (see Chapter 3), and we *immediately* invoked it and assigned its return value directly to our single module instance identifier `foo`.

Modules are just functions, so they can receive parameters:

```js
function CoolModule(id) {
	function identify() {
		console.log( id );
	}

	return {
		identify: identify
	};
}

var foo1 = CoolModule( "foo 1" );
var foo2 = CoolModule( "foo 2" );

foo1.identify(); // "foo 1"
foo2.identify(); // "foo 2"
```

Another slight but powerful variation on the module pattern is to name the object you are returning as your public API:

```js
var foo = (function CoolModule(id) {
	function change() {
		// modifying the public API
		publicAPI.identify = identify2;
	}

	function identify1() {
		console.log( id );
	}

	function identify2() {
		console.log( id.toUpperCase() );
	}

	var publicAPI = {
		change: change,
		identify: identify1
	};

	return publicAPI;
})( "foo module" );

foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```

By retaining an inner reference to the public API object inside your module instance, you can modify that module instance **from the inside**, including adding and removing methods, properties, *and* changing their values.

### Modern Modules

Various module dependency loaders/managers essentially wrap up this pattern of module definition into a friendly API. Rather than examine any one particular library, let me present a *very simple* proof of concept **for illustration purposes (only)**:

```js
var MyModules = (function Manager() {
	var modules = {};

	function define(name, deps, impl) {
		for (var i=0; i<deps.length; i++) {
			deps[i] = modules[deps[i]];
		}
		modules[name] = impl.apply( impl, deps );
	}

	function get(name) {
		return modules[name];
	}

	return {
		define: define,
		get: get
	};
})();
```

The key part of this code is `modules[name] = impl.apply(impl, deps)`. This is invoking the definition wrapper function for a module (passing in any dependencies), and storing the return value, the module's API, into an internal list of modules tracked by name.

And here's how I might use it to define some modules:

```js
MyModules.define( "bar", [], function(){
	function hello(who) {
		return "Let me introduce: " + who;
	}

	return {
		hello: hello
	};
} );

MyModules.define( "foo", ["bar"], function(bar){
	var hungry = "hippo";

	function awesome() {
		console.log( bar.hello( hungry ).toUpperCase() );
	}

	return {
		awesome: awesome
	};
} );

var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );

console.log(
	bar.hello( "hippo" )
); // Let me introduce: hippo

foo.awesome(); // LET ME INTRODUCE: HIPPO
```

Both the "foo" and "bar" modules are defined with a function that returns a public API. "foo" even receives the instance of "bar" as a dependency parameter, and can use it accordingly.

Spend some time examining these code snippets to fully understand the power of closures put to use for our own good purposes. The key take-away is that there's not really any particular "magic" to module managers. They fulfill both characteristics of the module pattern I listed above: invoking a function definition wrapper, and keeping its return value as the API for that module.

In other words, modules are just modules, even if you put a friendly wrapper tool on top of them.

### Future Modules

ES6 adds first-class syntax support for the concept of modules. When loaded via the module system, ES6 treats a file as a separate module. Each module can both import other modules or specific API members, as well export their own public API members.

**Note:** Function-based modules aren't a statically recognized pattern (something the compiler knows about), so their API semantics aren't considered until run-time. That is, you can actually modify a module's API during the run-time (see earlier `publicAPI` discussion).

By contrast, ES6 Module APIs are static (the APIs don't change at run-time). Since the compiler knows *that*, it can (and does!) check during (file loading and) compilation that a reference to a member of an imported module's API *actually exists*. If the API reference doesn't exist, the compiler throws an "early" error at compile-time, rather than waiting for traditional dynamic run-time resolution (and errors, if any).

ES6 modules **do not** have an "inline" format, they must be defined in separate files (one per module). The browsers/engines have a default "module loader" (which is overridable, but that's well-beyond our discussion here) which synchronously loads a module file when it's imported.

Consider:

**bar.js**
```js
function hello(who) {
	return "Let me introduce: " + who;
}

export hello;
```

**foo.js**
```js
// import only `hello()` from the "bar" module
import hello from "bar";

var hungry = "hippo";

function awesome() {
	console.log(
		hello( hungry ).toUpperCase()
	);
}

export awesome;
```

```js
// import the entire "foo" and "bar" modules
module foo from "foo";
module bar from "bar";

console.log(
	bar.hello( "rhino" )
); // Let me introduce: rhino

foo.awesome(); // LET ME INTRODUCE: HIPPO
```

**Note:** Separate files **"foo.js"** and **"bar.js"** would need to be created, with the contents as shown in the first two snippets, respectively. Then, your program would load/import those modules to use them, as shown in the third snippet.

`import` imports one or more members from a module's API into the current scope, each to a bound variable (`hello` in our case). `module` imports an entire module API to a bound variable (`foo`, `bar` in our case). `export` exports an identifier (variable, function) to the public API for the current module. These operators can be used as many times in a module's definition as is necessary.

The contents inside the *module file* are treated as if enclosed in a scope closure, just like with the function-closure modules seen earlier.

## Review (TL;DR)

Closure seems to the un-enlightened like a mystical world set apart inside of JavaScript which only the few bravest souls can reach. But it's actually just a standard and almost obvious fact of how we write code in a lexically scoped environment, where functions are values and can be passed around at will.

**Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.**

Closures can trip us up, for instance with loops, if we're not careful to recognize them and how they work. But they are also an immensely powerful tool, enabling patterns like *modules* in their various forms.

Modules require two key characteristics: 1) an outer wrapping function being invoked, to create the enclosing scope 2) the return value of the wrapping function must include reference to at least one inner function that then has closure over the private inner scope of the wrapper.

Now we can see closures all around our existing code, and we have the ability to recognize and leverage them to our own benefit!
