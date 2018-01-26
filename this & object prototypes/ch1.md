# You Don't Know JS: *this* & Nguyên mẫu đối tượng (object proptotypes)
# Chương 1: `this` hay là That?

Một trong những cơ chế hãi hùng nhất của JavaScript là từ khoá `this`. Nó là một từ khoá nhận dạng đặc biệt tự động xác định trong scope của mọi hàm, nhưng ngay cả lập trình viên JavaScript dày dặn cũng phải điêu đứng với câu chuyện chính xác nó là gì.

> Công nghệ *tiên tiến* với phép thuật không phân biệt được. -- Arthur C. Clarke

Cơ chế của `this` không hẳn ghê gớm như vậy, nhưng các lập trình viên thường diễn giải trong tâm trí bằng cách thêm từ "phức tạp" hoặc "mơ hồ", `this` có thể xem là sự huyền diệu trong cái hoang mang.

## Tại sao lại là `this`?

Nếu cơ chế `this` quá chua, ngay cả đối với lập trình viên JavaScript dày dạn, người ta có thể thắc mắc tại sao nó lại hữu dụng? Hay nó chỉ đem lại rắc rối? Trước khi tìm hiểu *như thế nào*, chúng ta nên xác định *vì sao*

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

Nếu *như thế nào* của đoạn code này vẫn còn làm bạn bối rối, đừng lo! Chúng ta sẽ tìm hiểu nó cụ thể ngay đây. Chỉ cần đặt câu hỏi này qua một bên, như vậy ta mới có thể nhìn rõ *tại sao* hơn.

Đoạn trích này cho phép hàm `identify()` và `speak()` được tái sử dụng cho object trong nhiều *ngữ cảnh* (`me` và `you`) thay vì sử dụng nhiều phiên bản khác nhau của hàm cho mỗi object.

Thay vì sử dụng `this`, bạn có thể thông qua object một cách rõ ràng trong ngữ cảnh cho cả `identify()` và `speak()`.

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

Tuy nhiên, cơ chế `this` cung cấp phương thức tao nhã hơn theo kiểu ngầm truyền một object tham chiếu, hướng tới thiết kế API rõ ràng hơn và dễ sử dụng hơn.

Mô hình bạn sử dụng càng phức tạp, bạn sẽ càng thấy rõ việc dùng một tham số cụ thể tên tuổi thường rối rắm hơn dùng `this`. Khi bạn khám phá object & prototype, bạn sẽ thấy sự hữu dụng của một tập hợp hàm có thể tự động tham chiếu với object ngữ cảnh thích hợp.

## Các nhầm lẫn

Chúng ta sẽ sớm giải thích `this` *thực sự* hoạt động ra sao, nhưng trước tiên chúng ta phải xoá tan những hiểu nhầm về những gì nó không thực sự đúng.

Cái tên "this" tạo ra một số nhầm lẫn khi các lập trình viên cố gắng nghĩ về nó theo nghĩa đen quá. Có hai ý nghĩa thường đưa ra nhưng cả hai đều sai.

### Chính nó

Cám dỗ phổ biến đầu tiên là giả định bản thân `this` như một hàm. Ít nhất, đó là một suy luận có lý về mặt ngữ pháp.

Tại sao bạn muốn tham chiếu một hàm bên trong chính nó? Một số lý do phổ biến là thứ gì đó như kiểu đệ quy (gọi hàm  từ bên trong nó) hoặc có một event handler mà có thể tự huỷ trong lần gọi đầu tiên.

Các lập trình viên mới làm quen với cơ chế của JS thường tham chiếu hàm như một object (tất cả các hàm trong JS đều là object!) cho phép bạn lưu trữ *state* (giá trị bên trong thuộc tính) khi gọi hàm. Mặc dù điều này là chắc chắn và có một số hạn chế sử dụng, phần còn lại của cuốn sách sẽ mô tả trên nhiều mô hình khác cho *vị trí* tốt hơn để lưu trữ state bên cạnh mô hình object hàm.

Ở đây chúng ta tìm hiểu vấn đề này một chút, để minh hoạ `this` không cho một hàm tham chiếu lên chính nó như chúng ta giả định như thế nào.

Xem đoạn code dưới đây, chúng ta thử theo dõi bao nhiêu lần hàm `foo` được gọi:

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

`foo.count` vẫn là `0`, mặc dù qua bốn lần `console.log` đã chỉ ra rõ ràng `foo(..)` là sự kiện được gọi bốn lần. Sự thất vọng bắt nguồn từ sự diễn dịch *quá trực nghĩa* của `this` (`this.count++`).

Khi `foo.count = 0` được thực thi, thực chất là nó thêm một thuộc tính `count` vào hàm `foo`. Nhưng với `this.count` tham chiếu trong hàm, `this` không trỏ đến hàm chút nào, và mặc dù tên thuộc tính giống nhau, object gốc lại khác nhau, và sự nhầm lẫn xảy ra.

**Ghi chú:** Một lập trình viên có trách nhiệm sẽ *phải* hỏi ở chỗ này "nếu tôi đã thêm một thuộc tính `count` nhưng nó lại không phải cái tôi mong muốn, vậy `count` mà tôi đã thêm là cái nào?" Thực tế, nếu đào sâu thêm, bạn sẽ thấy rằng vô tình bạn đã tạo một biến toàn cục `count` (xem Chương 2 để biết nó đã xảy ra *như thế nào*), và hiện tại nó có giá trị `NaN`. Tất nhiên, một khi bạn đã nhận thấy kết quả đặc biệt này, câu ta sẽ có một câu hỏi tổng quan hơn: "Nó toàn cục như thế nào? và tại sao nó lại kết thúc bằng `NaN` thay cho một giá trị đếm được?" (Xem Chương 2)

Thay vì dừng tại điểm này và đào sâu vào lý do tại sao tham chiếu `this` có vẻ không thoả *kỳ vọng*, và trả lời của những câu hỏi khó nhưng quan trọng này, nhiều lập trình viên đơn giản tránh lỗi này, và khám phá giải pháp khác, ví dụ như tạo một object để giữ giá trị `count`:

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

Trong khi rõ ràng cách tiếp cận này "giải quyết" vấn đề, không may nó lại bỏ qua một vấn đề thực sự -- thiếu hiểu biết ý nghĩa của `this` và cách nó làm việc -- thay vào đó là rút lui vào một cơ chế gần gũi hơn: lexical scope

**Chú ý:** Lexical scope là một cơ chế tốt và hững dụng; Trên mọi phương diện thì tôi không coi thường việc sử dụng nó (Xem *"Scope & Closures"*). Nhưng lúc nào cũng *đoán* sử dụng `this` như thế nào, hoặc thường xuyên hiểu *sai* thì đây không phải là lý do tốt để chuyển qua sử dụng lexical scope và không bao giờ học *vì sao* `this` lẩn tránh bạn.

Để tham chiếu một hàm bên trong chính nó, bản thân `this` thường không đầy đủ, bạn cần có một tham chiếu đến hàm thông qua một định danh gốc (biến) để trỏ vào nó.

Xem 2 hàm dưới đây:

```js
function foo() {
	foo.count = 4; // `foo` tham chiếu đến chính nó
}

setTimeout( function(){
	// hàm vô danh, không thể tham chiếu đến chính nó.
}, 10 );
```

Trong hàm thứ nhất, gọi là "hàm định danh", `foo` là một tham chiếu có thể sử dụng để tham chiếu đến hàm bên trong nó.

Nhưng trong ví dụ thứ 2, hàm callback qua `setTimeout(..)` không có nhận dạng tên (còn gọi là "hàm vô danh"), do đó không có cách nào tham chiếu đến object hàm bởi chính nó.

**Ghi chú:** Có một phương thức cũ bị bãi bỏ là `arguments.callee` tham chiếu bên trong một hàm *cũng* trỏ về object hàm của hàm đang được thực thi. Các tham chiếu này thường là cách duy nhất để tiếp cận một object hàm vô danh bên trong nó. Tuy nhiên, cách tiếp cận tốt nhất là tránh sử dụng chức năng ẩn danh hoàn toàn, ít nhất là đối với các hàm cần một tham chiếu chính nó, thay vào đó là tạo hàm có tên (biểu thức). `arguments.callee` không được sử dụng nữa và cũng không nên sử dụng.

Do đó một giải pháp khác cho ví dụ của chúng ta là sử dụng nhận diện `foo` như một tham chiếu object hàm tại mỗi vị trí, và không sử dụng `this` nữa, nó sẽ hoạt động.

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

Tuy nhiên, cách tiếp cận này tương tự các bước *thực sự* hiểu `this` và dựa hoàn toàn vào lexical scope của biến `foo`.

Cách khác để tiếp cận vấn đề là buộc `this` thực sự trỏ vào `foo` object hàm:


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
		// tự nó trỏ vào object hàm (`foo`)
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

**Thay vì tránh né `this`, chúng ta bao lấy nó.** Chúng ta sẽ giải thích các kỹ thuật này hoạt động tốt hơn như thế nào, vì vậy đừng lo lắng nếu bạn vẫn còn một chút băn khoan!

### Phạm vi (Scope) của nó

Quan niệm sai lầm phổ biến tiếp theo về ý nghĩa của `this` là bằng cách nào đó, nó tham chiếu đến scope của hàm. Đây là một câu hỏi khéo, vì xét về mặt nào đó nó là đúng, nhưng ở khía cạnh khác nó là sai.

Một cách rõ ràng thì `this` không tham chiếu đến một **lexical scope** của hàm. Đúng là xét về nội bộ, scope là một dạng giống như object có thuộc tính cho mỗi object định danh sẵn có. Nhưng scope "object" không thể truy cập vào JavaScript code. Nó là phần bên trong thực thi của *Engine*.

Xem đoạn code đã cố (và thất bại!) vượt qua ranh giới và sử dụng `this` để ngầm tham chiếu vào lexical scope của hàm:

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

Có nhiều sai sót trong đoạn code này. Đoạn code bạn thấy là sự trích tách từ một đoạn code thực tế được trao đổi trong các diễn đàn cộng đồng hỗ trợ. Nó là một minh hoạ tuyệt vời (nếu không đáng buồn) cho việc hiểu sai `this` cho giả định được đặt ra.

Đầu tiên, một sự cố gắng được tạo ra để tham chiếu hàm `bar()` thông qua `this.bar()`. Hầu như chắc chắn rằng *vô tình* nó hoạt động, nhưng chúng ta sẽ giải thích *vì sao* sau. Cách tự nhiên nhất để gọi `bar()` là bỏ qua đầu mối `this` và chỉ cần tạo một tham chiếu lexical đến hàm định danh.

Tuy nhiên, lập trình viên nào đã viết đoạn code trên đang cố gắng sử dụng `this` để tạo ra một cây cầu giữa các lexical scope của `foo()` và `bar()`, từ đó `bar()` có thể tiếp cận biến `a` trong scope của `foo()`. **Chẳng có cái cầu nào xảy ra**. Bạn không thể sử dụng `this` để tham chiếu và tìm một thứ gì đó trong lexical scope. Hoàn toàn không thể.

Mỗi khi bạn cảm thấy mình muốn pha trộn việc tìm kiếm với `this`, luôn nhớ rằng: *không có cây cầu nào hết*

## `this` là gì?

Với các giả định sai sót khác nhau, giờ chúng ta chuyển qua việc chú ý cơ chế hoạt động thực sự của `this`.

Chúng ta đã nhắc `this` không phải là một ràng buộc author-time mà là một ràng buộc runtime. Nó dựa trên ngữ cảnh của điều kiện gọi hàm. Ràng buộc `this` không liên quan với nơi mà hàm được khai báo, thay vào đó, mọi thứ liên quan đến cách gọi hàm.

Khi một hàm được gọi ra, một bản ghi kích hoạt hay còn gọi là ngữ cảnh thực thi được tạo ra. Bản ghi này chứa thông tin về nơi mà hàm được gọi (call-stack), *cách* hàm đã được gọi, tham số nào được truyền,v.v...Một trong như thuộc tính của tham chiếu của bản ghi này là `this`, là cái được sử dụng trong suốt quá trình thực thi hàm.

Trong chương tiếp theo, ta sẽ học cách tìm một **call-site** của hàm để xác định việc thực thi của nó sẽ ràng buộc `this` như thế nào.

## Ôn tập (TL;DR)

Ràng buộc `this` là một đề tài gây bối rối cho nhiều lập trình viên JavaScript chưa bỏ thời gian tìm hiểu cơ chế của nó thực chất hoạt động như thế nào. Việc đoán, thử và sai, sao chép mù quáng từ các câu trả lời từ Stack Overflow không phải là cách hiệu quả để tận dụng cơ chế `this` quan trọng này.

Cho dù bất kỳ giả định hay quan niệm sai lầm nào dẫn lối, để học `this`, bạn phải học cái không phải là `this`. Bản thân `this` không phải là một tham chiếu đến hàm, hay là một tham chiếu đến lexical scope của hàm.

`this` chỉ thực sự là một ràng buộc được tạo ra cho đến khi hàm được gọi, và cái gì nó tham chiếu đều được xác định bởi call-site tại nơi hàm được gọi.

