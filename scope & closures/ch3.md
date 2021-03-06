# You Don't Know JS: Scope & Closures
# Chương 3: Hàm vs. Block scope

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

Đầu tiên, để ý rằng bao lệnh hàm được bắt đầu bằng `(function...` đối lập với `function...`. Trong khi nhìn nó là chi tiết phụ nhưng nó lại là thay đổi chính. Thay vì ứng xử hàm như một khai báo chuẩn, thì hàm lại coi như là một biểu thức của hàm.

**Ghi chú:** Cách đơn giản để phân biết khai báo hay biểu thức là vị trí của từ "function" trong câu lệnh (không chỉ khác một dòng, mà là cả câu lệnh). Nếu "function" là cái trước nhất của biểu thức, thì nó là khai báo. Ngược lại, nó là một biểu thức hàm.

Mấu chốt khác biệt mà ta thấy ở đây là một khai báo hàm và biểu thức hàm liên quan đến nơi tên của nó được ràng buộc như một định danh.

So sánh hai đoạn code trên. Nếu đoạn code đầu, cái tên `foo` được ràng buộc với phạm vi của nó, và ta gọi trực tiếp `foo()`. Trong đoạn thứ hai, cái tên `foo` không ràng buộc với phạm vi của nó, mà chỉ ràng buộc với bên trong chính hàm của nó.

Nói khác đi, `(function foo(){ .. })` là một biểu thức nghĩa là định danh `foo` chỉ được tìm thấy trong phạm vi `..` chỉ ra, không phải phạm vi bên ngoài. Ẩn `foo` bên trong chính nó nghĩa là không làm ảnh hưởng phạm vi của nó.

### Vô danh vs. Có tên

Bạn đã quen thuộc với biểu thức hàm là tham chiếu callback, như là:

```js
setTimeout( function(){
	console.log("I waited 1 second!");
}, 1000 );
```

Cái này được gọi là "biểu thức hàm vô danh", bởi vì `function()...` không có tên định danh. Biểu thức hàm có thể vô danh, nhưng khai báo hàm phải có tên.

Hàm vô danh nhanh và tiện gõ, và nhiều thư viện và công cụ có xu hướng khuyến khích điều này, nhưng có vài vấn đề cần phải nắm rõ:

1. Hàm vô danh sẽ không có tên trong truy dấu, khó debug.

2. Không có tên, nếu hàm muốn tham chiếu đến nó, hoặc đệ quy, ..., tham chiếu **đã bị bỏ** `arguments.callee` lại cần thiết. Ví dụ khác nữa là khi một hàm điều khiển sự kiện cần unbind chính nó sau khi chạy.

3. Hàm vô danh làm code khó đọc hơn. Một cái tên còn chính là document của code.

**Biểu thức hàm trực tiếp** rất mạnh và hữu dụng -- câu hỏi giữa hàm ẩn danh vs. hàm có tên cũng không ảnh hưởng. Đặt tên tất nhiên là tốt hơn, nhưng không có nhược điểm. Dù gì tốt nhất vẫn phải luôn đặt tên hàm:

```js
setTimeout( function timeoutHandler(){ // <-- coi nè, tui có tên đó!
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

Ta có một hàm như một biểu thức được đặt trong `( )`, chúng ta có thể xử lý hàm đó bằng thêm `()` vào phía cuối `(function foo(){ .. })()`. `( )` đằng trước tạo ra một biểu thức cho hàm, và `()` thực thi hàm.

Mẫu này rất thông thường, một vài năm trước cộng đồng đã đồng ý cho cụm từ: **IIFE**, đại diện cho **I**mmediately **I**nvoked **F**unction **E**xpression.

Đương nhiên, IIFE không cần tên -- dạng thông thường của IIFE được sử dụng theo cách vô danh. Việc đặt tên IIFE không phổ biến nhưng lợi ích đối với hàm ẩn danh nói trên thì đây cũng là việc tốt để thực hành.

```js
var a = 2;

(function IIFE(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Có một biến tấu nhỏ trong dạng IIFE truyền thống, một số người thích: `(function(){ .. }())`. Nhìn kỹ để thấy sựa khác biệt. Trong dạng đầu tiên, biểu thức hàm được bao trong `( )`, và sau đó `()` gọi hàm nằm ngay bên ngoài. Trong dạng thứ 2, `()` lại được bỏ vào trong `( )`.

Hai dạng này giống hệt nhau. **Tùy theo phong cách bạn chọn thôi.**

Biến tấu khác của IIFE cũng hay thấy là sử dụng sự kiện trong sự kiện, chỉ gọi hàm, và truyền vào tham số.

Ví dụ:

```js
var a = 2;

(function IIFE( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

})( window );

console.log( a ); // 2
```

Ta gọi tham chiếu `window` nhưng đặt tên tham số là `global`, nên ta có một cách mô tả rõ ràng giữa đại diện toàn cục vs. không toàn cục. Đương nhiên bạn có thể truyền bất cứ gì vào phạm vi bên trong bạn cần, và bạn có thể đặt tên tham số bất kỳ. Nó hầu như cũng chỉ là phong cách.

Ứng dụng khác của mẫu này cũng giải quyết một hiệu ứng phụ là định danh `undefined` mặc định có thể có giá trị không hợp lệ ghi đè dẫn đến kết quả không mong muốn. Bằng cách đặt tên tham số `undefined`, nhưng không truyền bất kỳ giá trị đối số nào, ta có thể đảm bảo giá trị không xác định ở trong khối code:

```js
undefined = true; // đây là đặt mìn ngầm cho code! cần tránh!

(function IIFE( undefined ){

	var a;
	if (a === undefined) {
		console.log( "Undefined an toàn ở đây!" );
	}

})();
```

Và biến thể khác của IIFE là đảo ngược thứ tự các thứ, hàm có thể thực thi sau khi viện dẫn tham số để truyền vào nó. Mẫu này được sử dụng trong dự án UDM (Universal Module Definition). Dù nó hơi dài dòng, một số người lại thấy vậy lại dễ hiểu.

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
Hàm `def` được xác định trong phần thứ hai của đoạn code, và sau đó truyền như một tham số (gọi là `def`) vào hàm `IIFE` ở phần đầu của đoạn code. Cuối cùng, tham số `def` (hàm) được gọi, truyền `window` vào với tham số `global`.

## Block scope

Trong khi hàm là đơn vị thường thấy của phạm vi, và quy mô của nó trong thiết kế JS cũng lan rộng khắp chương trình, các đơn vị khác thì lại có thể dẫn đến sự rõ ràng, sạch sẽ để bảo trì code.

Nhiều ngôn ngữ khác JavaScript hỗ trợ Block Scope, và người lập trình các ngôn ngữ đó thường quen với khái niệm này, còn dân chỉ chơi JavaScript có thể thấy khái niệm này hơi lạ lẫm.

Nhưng mặc dù bạn chưa bao giờ viết bất kỳ dòng code nào theo lối block-scoped, bạn vẫn quen với kiểu này như một thành ngũ trong JavaScript:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Ta khai báo biến `i` trực tiếp trong đầu vòng lặp for, bởi vì ý định của chúng ta là chỉ sử dụng `i` cho ngữ cảnh của vòng lặp này, và cơ bản bỏ qua ảnh hưởng của phạm vi ngoài (hàm hay toàn cục).

Đó là tất cả nội dung của block scope: khai báo biến tại nơi nó được sử dụng càng cục bộ càng tốt. Ví dụ khác:

```js
var foo = true;

if (foo) {
	var bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}
```

Ta dùng biến `bar` chỉ trong ngữ cảnh của lệnh if, nên nó tạo ra cảm giác rằng ta có thể khai báo nó bên trong khối if. Tuy nhiên, nơi khai báo lại không liên quan khi sử dụng `var`, vì nó luôn thuộc về scope bên trong nó. Đoạn code này cơ bản là "giả" block-scoping, và nên dựa vào việc tự thực thi chứ đừng vô tình sử dụng `bar` ở chỗ khác trong phạm vi.

Block scope là công cụ để mở rộng "Principle of Least ~~Privilege~~ Exposure" (Nguyên tắc tối thiểu) để ẩn thông tin trong hàm thành ẩn thông tin trong khối của code.

Xem lại đoạn code:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Vì sao ảnh hưởng toàn bộ phạm vi hàm với biến `i` lại chỉ sử dụng (và chỉ nên sử dụng) cho vòng lặp for?

Điều quan trọng nhất là nhà phát triển muốn nó tự kiểm để tránh vô tình sử dụng biến ngoài mục đích, chẳng hạn như báo lỗi biến không xác định nếu bạn sử dụng biến sai chỗ. Block scope cho biến `i` làm cho `i` chỉ khả dụng cho vòng lặp for, sẽ lỗi nếu `i` truy cập chỗ khác trong hàm. Việc này chắc chắn biến không được tái sử dụng nhầm lẫn và khó bảo trì.

Thực tế đáng buồn là xét ở bề mặt thì JavaScript không có cơ sở cho block scope, bạn khai thác thêm mới có.

### `with`

Chúng ta đã tìm hiểu `with` ở Chương 2. Dù nó là một cấu trúc kỳ quặc nhưng nó chính là ví dụ của block scope, trong đó, phạm vi được tào từ object chỉ tồn tại trong vòng đời của `with`, và không ở trong toàn bộ phạm vi.

### `try/catch`

Một chi tiết nhỏ rằng từ JavaScript ES3 đã chỉ định khai báo biến trong mệnh đề `catch` của `try/catch` để block-scoped cho khối `catch`.

Ví dụ:

```js
try {
	undefined(); // thực thi bất hợp lệ nhằm được chấp nhận!
}
catch (err) {
	console.log( err ); // hoạt động!
}

console.log( err ); // ReferenceError: `err` không tìm thấy
```

Như bạn thấy, `err` chỉ tồn tại trong mệnh đề `catch`, và báo lỗi khi bạn muốn thao chiếu nó đâu đó.

**Ghi chú:** Trong khi hành vi này được xác định và đúng với tất cả môi trường JS tiêu chuẩn (có thể ngoại trừ một số trình duyệt IE cũ), nhiều linter có vẻ vẫn khó chịu với nhiều hơn hai mệnh đề `catch` trong cùng một phạm vi mà mỗi khai báo biến lỗi cùng với tên định danh, mặc dù việc này không cần định nghĩa lại vì các biến đã block-scoped an toàn.

Để tránh những cảnh báo không cần thiết, một số lập trình viên sẽ đặt tên biến `catch` với `err1`, `err2`... Một số khác chỉ đơn giản tắt báo trùng tên biên của linter.

Bản chất của `catch` block-scoping trông có vẻ vô dụng, nhưng trong Phụ Lục B sẽ giải thích vì sao nó hữu dụng.

### `let`

Chúng ta đã thấy JavaScript cũng chỉ có vài hành vi lạ phơi bày chức năng block scope. Nếu đó là những gì ta có (điều đã xảy ra trong nhiều năm), thì block scoping chẳng có lợi ích cho lập trình viên JavaScript.

May mắn là ES6 đã thay đổi điều này, từ khóa `let` ra như một cách khai báo biến khác bên cạnh `var`.

Từ khóa `let` gắn liền việc khai báo với phạm vi của bất kỳ khối nào (thường là trong `{ .. }`) chứa nó.

```js
var foo = true;

if (foo) {
	let bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}

console.log( bar ); // ReferenceError
```
Sử dụng `let` để gắn một biến vào một block hiện hữu có gì đó hơi ngầm. Nó có thể làm bạn nhầm nếu bạn không để ý block nào có biến nào suốt quá trình phát triển code bằng việc di chuyển block, bao nó trong block khác...

Tạo các block biệt lập cho block-scoping có thể giải quyết một số mối lo, cho thấy rõ nó có được gắn liền hay không. Thông thường, đoạn code ngầm được ưa dùng hơn code biệt lập, nhưng kiểu tách block-scoping này dễ diễn đạt, và tự nhiên phù hợp hơn cách block-scoping hoạt động như trong các ngôn ngữ khác:

```js
var foo = true;

if (foo) {
	{ // <-- khối biệt lập
		let bar = foo * 2;
		bar = something( bar );
		console.log( bar );
	}
}

console.log( bar ); // ReferenceError
```

Ta có thể tạo một block ngẫu nhiên cho `let` đơn giản bằng cách thêm cặp `{ .. }` bất cứ chỗ nào trong một cú pháp hợp lệ. Trong trường hợp này, bạn tạo một block biệt lập trong lệnh if, nó giúp dễ dàng hơn khi di chuyển trong quá trình refactor mà không ảnh hưởng vị trí và ngữ nghĩa của lệnh if đi kèm.

**Ghi chú:** Cách khác để diễn đạt block scope rõ ràng có thể xem phụ lục B.

Trong Chương 4, ta sẽ tìm hiểu hoisting, việc khai báo được đưa ra trước cho toàn bộ phạm vi chứa nó.

Tuy nhiên, khai báo được tạo ra với `let` sẽ *không* đưa lên trong toàn bộ block nó xuất hiện. Bởi việc khai báo sẽ không "tồn tại" cho đến khi có biểu thức khai báo.

```js
{
   console.log( bar ); // ReferenceError!
   let bar = 2;
}
```

#### Gom rác

Lý do khác cho block-scoping là sự hữu ích liên quan đến closures và gom rác để lấy lại bộ nhớ. Tôi sẽ minh họa ngắn ở đây, cơ chế closure sẽ được giải thích chi tiết trong Chương 5.

```js
function process(data) {
	// làm gì đó thú vị
}

var someReallyBigData = { .. };

process( someReallyBigData );

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

Hàm callback điều khiển `click` không cần biến `someReallyBigData` gì hết. Nghĩa là về mặt lý thuyết, sau khi `process(..)` chạy, cấu trúc dữ liệu hao bộ nhớ được gom lại. Tuy nhiên, nó kiểu giống như JS engine vẫn giữ cấu trúc đâu đó, khi hàm `click` có một closure trên toàn bộ scope.

Block-scoping làm cho engine hiểu rõ nó không cần giữ `someReallyBigData` xung quanh:

```js
function process(data) {
	// làm gì đó
}

// bất kỳ khai báo trong khối này sẽ "ra đi" sau đó!
{
	let someReallyBigData = { .. };

	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

Khai báo các khối riêng biệt cho biến ràng buộc cục bộ là một công cụ mạnh mẽ.

#### Vòng lặp `let`

Một trường hợp đặc biệt mà `let` tỏa sáng là trong vòng lặp for.

```js
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```

Không chỉ có `let` trong đầu vòng lặp ràng buộc `i` vào thân của vòng lặp, mà còn tái ráng buộc nó vào mỗi lần lặp, đảm bảo gán lại giá trị của nó từ cuối của lần lặp trước.

Đây là cách khác để minh họa hành vi mỗi lần lặp:

```js
{
	let j;
	for (j=0; j<10; j++) {
		let i = j; // chuyển qua mỗi lần lặp!
		console.log( i );
	}
}
```

Lý do tại sao ràng buộc từng lần lặp lại hay sẽ thảo luận rõ ở Chương 5.

Vì khai báo `let` đính kèm khối tùy ý chứ không phải phạm vi hàm bao quanh (hay toàn cục), nó có thể tạo được tại nơi code hiện hữu có một khai báo ẩn `var` trong function scoped, việc thay thế `var` bằng `let` có thể cần chú ý khi refactor code.

ví dụ:

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

Code này có thể dễ dàng refactor như sau:

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
Nhưng cẩn thận với các thay đổi khi sử dụng biến trong block scope:

```js
var foo = true, baz = 10;

if (foo) {
	let bar = 3;

	if (baz > bar) { // <-- đừng quên `bar` khi thay đổi!
		console.log( baz );
	}
}
```

Xem phụ lục B cho kiểu khác (cụ thể hơn) của block-scoping, có thể cung cấp một hướng để bảo trì/refactor code dễ dàng hơn.

### `const`

Bên cạnh `let`, ES6 cũng giới thiệu `const`, cũng là tạo ra biến trong block-scoped, nhưng giá trị được cố định (constant - hằng số). Bất kỳ ý định thay đổi giá trị của nó sẽ bị lỗi.

```js
var foo = true;

if (foo) {
	var a = 2;
	const b = 3; // block-scoped trong mệnh đề `if`

	a = 3; // just fine!
	b = 4; // error!
}

console.log( a ); // 3
console.log( b ); // ReferenceError!
```

## Ôn tập (TL;DR)

Hàm là một trong những đơn vị thông thường của phạm vi (scope) trong JavaScript. Biến và hàm được khai báo bên trong
hàm khác sẽ "ẩn" đối với "scope" bao ngoài nó, đây là một nguyên tắc thiết kế phần mềm tốt.

Nhưng hàm không phải là đơn vị duy nhất của scope. Block-scope đề cập đến ý tưởng rằng biến và hàm có thể thuộc một block bất kỳ (chính xác là `{...}`) chứ không chỉ là hàm trên nó.

Bắt đầu từ ES3, cấu trúc `try/catch` có block-scope trong mệnh đề `catch`.

Trong ES6, từ khóa `let` (bà con của `var`) được giới thiệu cho phép khai báo biến trong block bất kỳ. `if (..) { let a = 2; }` sẽ khai báo biến `a` rằng nó gắn liền chính nó trong `{...}` của mệnh đề `if`.

Mặc dù vậy, block scope cũng không nên được thực hiện như là thay thế hoàn toàn cho `var` trong phạm vi hàm. Cả hai đều có thể cùng tồn tại, và người lập trình có thể sử dụng cả kỹ thuật function-scope và block-scope để phù hợp với việc bảo trì, đọc code.

#### Xem thêm
[Principle of Least Privilege](http://en.wikipedia.org/wiki/Principle_of_least_privilege)
