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

1. Nó phải là hàm bao quanh bên ngoài, và phải được gọi ít nhất một lần (mỗi lần gọi tạo nên một module instance).

2. Hàm bên ngoài phải trả ít nhất một hàm bên trong, nhờ vậy hàm bên trong có closure thông qua scope riêng biệt, và có thể truy cập để thay đổi trạng thái riêng biệt đó.

Bản thân một object với thuộc tính hàm bên trong nó không thực sự là một module. Trong bối cảnh observable, một object được trả từ một hàm chỉ có các thuộc tính dữ liệu bên trong nó và không có hàm closure nào thì *không thực sự* là một module.

Đoạn code ở trên cho thấy một trình tạo module độc lập gọi là `CoolModule()`, có thể gọi bao nhiêu lần cũng được, mỗi lần gọi thì nó tạo một module tức thì. Một thay đổi nhỏ với mẫu này là khi bạn quan tâm đến việc chỉ có một lần tạo, đây là một dạng "sigleton":

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

Chúng ta đã chuyển hàm module thành IIFE và gọi nó ngay lập tức và gán giá trị trả lại của nó trực tiếp vào module instance đơn `foo`.

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

Một biến thể mạnh mẽ của module pattern là đặt tên object và bạn trả lại như một API công khai:

```js
var foo = (function CoolModule(id) {
	function change() {
		// sửa đổi API công khai
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

Bằng cách giữ lại tham chiếu bên trong object API công khai bên trong module instance, bạn có thể sửa đổi module instance đó **từ phía trong**, bao gồm phương thức thêm và bớt thuộc tính, và thay đổi giá trị.

### Module hiện đại

Vài trình tải/quản lý module phụ thuộc bao lấy mẫu module thành một API gần gũi hơn. Thay vì kiểm tra bất kỳ một thư viện cụ thể, tôi sẽ giới thiệu một chứng minh đơn giản cho khái niệm này **nhằm mục đích minh họa**:

```js
var MyModules = (function Manager() {
	var modules = {};

	// Cái này gọi là hàm tải/quản lý (loaders/managers) - người dịch
	function define(name, deps, impl) {
		for (var i = 0; i < deps.length; i++) {
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

Mấu chốt của đoạn code trên là `modules[name] = impl.apply(impl, deps)`. Nó sẽ chạy hàm xác định bên ngoài để tạo module, và lưu giá trị trả lại (API của module) thành một danh sách các module được theo dõi theo tên.

Và đây là cách tôi có thể sử dụng để tạo module:

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

Cả module "foo" và "bar" đều được xác định bằng một hàm trả một API công khai. "foo" thậm chí nhận được instance của "bar" như một tham số phụ thuộc, và có thể sử dụng nó tùy ý.

Bạn hãy dành thời gian để kiểm tra đoạn code trên cho đến khi hiểu hoàn toàn sức mạnh của closure để phục vụ cho mục đích của mình. Không có "ma thuật" nào trong trình quản lý module, nó đáp ứng hai đặc tính của module pattern mà tôi đã liệt kê ở trên: gọi một hàm xác định bao ngoài, và trả giá trị như là API của module đó.

Nói cách khác, module là module, kể cả khi bạn tạo một công cụ trên nó.

### Module tương lai

ES6 bổ sung cú pháp cao cấp cho khái niệm module. Khi được tải bởi hệ thống module, ES6 xử lý một file như một module riêng lẻ. Mỗi module có thể vừa nhập các module khác hoặc thành viên API cụ thể, đồng thời xuất các thành viên API công khai của chính nó.

**Ghi chú:** Module nền hàm không phải là nhận dạng mẫu tĩnh (cái mà trình biên dịch hiểu rõ), vì vậy API của nó sẽ không được nhận thấy cho đến run-time. Vì thế, bạn có thể thay đổi một API của module trong quá trình run-time (xem thảo luận về `publicAPI` ở trên).

Ngược lại, ES6 API của module là tĩnh (API không thay đổi tại run-time). Do đó, trình biên dịch kiểm tra trong quá trình tải và biên dịch có một tham chiếu đến thành của một API module đã nhập có *thực sự tồn tại*. Nếu tham chiếu API không tồn tại, trình biên dịch báo lỗi "trước" trong thời gian biên dịch (compiler-time) thay vì chờ cho run-time (báo lỗi, nếu có) theo thông thường.

ES6 module không có định dạng "trực tiếp", nó cần được xác định theo các file riêng lẻ (với mỗi module). Trình duyệt/engine có một trình tải module mặc định (có thể ghi đè, không thuộc vấn đề nói tới ở đây) đồng bộ tải module file khi nó được nhập (import).

Ví dụ:

**bar.js**
```js
function hello(who) {
	return "Let me introduce: " + who;
}

export hello;
```

**foo.js**
```js
// nhập mình `hello()` từ module "bar"
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
// nhập toàn bộ "foo" và "bar"
module foo from "foo";
module bar from "bar";

console.log(
	bar.hello( "rhino" )
); // Let me introduce: rhino

foo.awesome(); // LET ME INTRODUCE: HIPPO
```

**Ghi chú:** Các file riêng lẻ **"foo.js"** và **"bar.js"** cần được tạo, với nội dung như hai đoạn code trên. Sau đó chương trình của bạn có thể tải/nhập các module đó để sử dụng chúng như đã trình bày ở đoạn code thứ ba.

`import` một hay nhiều thành viên từ API của module trong phạm vi gần nhất, mỗi cái là một biến (`hello` trong trường hợp của ta). `module` nhập toàn bộ API module thành các biến (`foo`, `bar` trong trường hợp ví dụ). `export` xuất một định danh (biến, hàm) thành API công khai cho module gần nhất. Các biểu thức này có thể sử dụng nhiều lần trong việc xác định module nếu cần thiết.

Nội dung bên trong *file module* được xử lý như một scope closure bên ngoài, cũng như hàm closure đã xem ở trên.

## Ôn tập (TL;DR)

Closure được xem như sự giác ngộ về thế giới bí ẩn bên trong JavaScript. Nhưng nó lại là một tiêu chuẩn và rõ ràng khi chúng ta viết code trong môi trường lexical scope, hàm và giá trị có thể truyền xung quanh theo ý muốn.

**Khi một hàm có thể ghi nhớ và truy cập lexical scope của nó kể cả khi nó được gọi ngoài lexical scope được gọi là closure.**

Closure có thể làm chúng ta sai sót, ví dụ với loop, nếu ta không cẩn thận khi nhận biết chúng và cách chúng hoạt động. Nhưng nó cũng là một công cụ mạnh mẽ vô hạn, cho phép triển khai các mẫu (pattern) như *module* theo nhiều dạng khác nhau.

Module đòi hỏi hai đặc điểm chính:
* 1) Hàm bao ngoài được gọi để tạo scope bao ngoài.
* 2) Giá trị trả về của hàm bao ngoài phải bao gồm tham chiếu đến ít nhất một hàm bên trong để sau đó nó có closure thông qua phạm vi riêng (private) bên trong của hàm đó.

Và ta cũng thấy closure xuất hiện liên tục ở trong code của mình, đây là khả năng để nhận biết và tận dụng chúng cho mục đích của mình!
