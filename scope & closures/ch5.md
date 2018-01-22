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

Linh hồn của đoạn code trên là điều chúng ta *mong muốn* là các số "1", "2", .. "5" sẽ được in ra sau mỗi giây tương ứng.

Khi bạn chạy đoạn code này, bạn sẽ có kết quả là "6" được in ra 5 lần theo mỗi giây.

**Hả?**

Trước tiên, tôi sẽ giải thích `6` ở đâu ra. Điều kiện chấm dứt vòng lặp khi `i` *không* `<=5`. Trường hợp đầu tiên của việc này là `i` bằng 6. Vì vậy đầu ra phản ánh giá trị cuối cùng của `i` sau khi vòng lặp kết thúc.

Điều này có vẻ rõ ràng, hàm timeout callback đều chạy ngon sau khi hoàn thành vòng lặp. Trong thực tế, khi bộ đếm thực hiện, kể cả nếu `setTimeout(.., 0)` cho mỗi lần lặp, tất cả các hàm callback đó đều chạy đúng sau khi hoàn thành vòng lặp, do đó nó in `6` cho mỗi lần.

Đi vào sâu hơn, code của ta thiếu cái gì mà đáng lẽ nó phải thực hiện đúng như ta muốn?

Cái thiếu là cái chúng ta đang cố thực hiện rằng mỗi vòng lặp bắt 1 bản sao của `i` tại thời điểm lặp. Nhưng theo cách hoạt động của scope, thì ta có 5 hàm được xác định riêng biệt theo mỗi lần lặp, tất cả đều được **đóng kín bởi cùng một scope toàn cục**, và chỉ có một `i` trong nó.

Theo cách này, tất nhiên tất cả các hàm đều có chung một tham chiếu đến cùng một `i`. Có gì đó về cấu trúc vòng lặp làm ta bối rối và nghĩ rằng có gì đó phức tạp, nhưng không phải vậy, chẳng có gì khác biệt với việc khai báo các hàm callback đó 5 lần tuần tự cái này tiếp cái kia mà không dùng vòng lặp.

Ok, vậy, quay lại câu hỏi của chúng ta. Cái gì thiếu? Ta cần thêm closure scope. Đặc biệt là chúng cần một closure scope mới cho mỗi lần lặp.

Chúng ta đã học trong Chương 3 rằng IIFE tạo scope bằng cách khai báo một hàm và thực thi nó ngay lập tức.

Thử xem:

```js
for (var i=1; i<=5; i++) {
	(function(){
		setTimeout( function timer(){
			console.log( i );
		}, i*1000 );
	})();
}
```

Nó có chạy không?

**Không.** Nhưng tại sao? Rõ ràng là có thê lexical scope. Mỗi hàm timeout callback đóng qua mỗi lần lặp scope của riêng nó được tạo bởi IIFE tương ứng.

Bởi vì hàm IIFE của ta là một scope rỗng chẳng làm gì cả. Nó cần có gì đó hữu dụng hơn, tức nó cần một biến riêng, là bản sao của giá trị `i` sau mỗi lần lặp.


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

**Eureka! Đã chạy!**

Phiên bản được ưa thích là:

```js
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})( i );
}
```

Cách dùng IIFE trong mỗi lần lặp tạo ra một scope mới cho mỗi lần lặp, cho phép hàm timeout call back cơ hội đóng thông qua scope mới cho mỗi lần lặp, và nó có biến trong mỗi lần lặp giúp ta try cập.

Vấn đề được giải quyết!

### Gặp lại block scoping

Xem xét cẩn thận các phân tích ở giải pháp trên. Chúng ta sử dụng IIFE để tạo ra scope cho mỗi lần lặp. Nói cách khác, chúng ta cần *block-scope* của mỗi mỗi lần lặp. Chương 3 cho ta thấy khai báo `let` trấn một block và khai báo một biến tại đó.

**Cơ bản nó tạo một block trong scope để chúng ta có thể đóng nó.** Vì vậy đoạn code dưới đây chạy ngon:

```js
for (var i=1; i<=5; i++) {
	let j = i; // yay, block-scope cho closure!
	setTimeout( function timer(){
		console.log( j );
	}, j*1000 );
}
```

*Nhưng chưa hết!* Nó có một hành vi đặc biệt được xác định cho khai báo `let` được sử dụng trên đầu của vòng lặp for. Hành vi này cho biết rằng biến sẽ được khai báo không chỉ một lần cho vòng lặp, **mà còn là mỗi lần lặp**. Và nó được khởi tạo tại mỗi  lần lặp với giá trị từ cuối cho đến lần lặp trước. (mới thấy `var` và `let` khác biệt rõ rệt nhé - người dịch)

```js
for (let i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

Block scoping và closure đã tay trong tay hoạt động cùng nhau, giải quyết mọi thứ trên thế giới. Tôi không biết bạn sao chớ nó làm cho tôi vui với JavaScript.

## Mô-dun (module)

Có những mẫu code sử dụng sức mạnh của closure nhưng không xuất hiện trên bề mặt mà thường là callback. Hãy kiểm tra một trong những kiểu mạnh nhất trong đó: *mô-đun*.

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

Hiện tại, đoạn code trên chưa có observable closure. Chúng ta chỉ có biến riêng `something`, `another`, và hàm `doSomething()` và `doAnother()` bên trong, cả biến và hàm đều có lexical scope bên trong phạm vi của `foo()`.

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

Đây là mẫu trong JavaScript gọi là *module*. Cách thông thường nhất để viết một mẫu module thường gọi là "Revealing Module", cách viết như ở trên.

Hãy kiểm xem có gì trong đoạn code trên.

Trước hết, `CoolModule()` chỉ là một hàm, nhưng nó phải được gọi để khởi tạo một module. Nếu không thực thi hàm bên ngoài, việc tạo scope bên trong và closure sẽ không xảy ra.

Tiếp theo, hàm `CoolModule()` trả về một object, biểu thị bởi cú pháp object-literal `{ key: value, ... }`. Object mà ta trả có tham chiếu trong nó đến các hàm phía trong, giữ cho chúng ẩn và riêng biệt. Bạn nên nghĩ object này trả giá trị như một **API công khai của module**.

Việc trả giá trị cho object cuối cùng gán cho biến bên ngoài `foo`, sau đó ta có thể truy cập những phương thức theo API, ví dụ `foo.doSomething()`.

**Ghi chú:** Không nhất thiết là ta phải trả một object (literal) thực tế từ module. Ta chỉ cần trả trực tiếp hàm bên trong. jQuery là một ví dụ, dấu `$` là một API công khai của jQuery module, nhưng bản thân nó cũng là một hàm (có thuộc tính, và các hàm đều là object).

Hàm `doSomething()` và `doAnother()` có closure thông qua scope bên trong của module (có được nhờ gọi `CoolModule()`). Khi ta chuyển hàm trong lexical scope ra ngoài, ta thiếu lập một điều kiện rằng closure nào có thể được observe và thực hiện bằng cách tham chiếu vào object chúng ta trả.

Mô tả đơn giản hơn, có hai yêu cầu cho một module pattern thực hiện:

1. Nó phải là hàm bao quanh bên ngoài, và phải được gọi ít nhất một lần (mỗi lần gọi tạo nên một module tức thì).

2. Hàm bên ngoài phải trả ít nhất một hàm bên trong, nhờ vậy hàm bên trong có closure thông qua scope riêng biệt, và có thể truy cập để thay đổi trạng thái riêng biệt đó.

Bản thân một object với thuộc tính hàm bên trong nó không thực sự là một module. Trong bối cảnh observable, một object được trả từ một hàm chỉ có các thuộc tính dữ liệu bên trong nó và không có hàm closure nào thì *không thực sự* là một module.

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
