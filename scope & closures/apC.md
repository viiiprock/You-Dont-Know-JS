# You Don't Know JS: Scope & Closures
# Phụ lục C: Lexical-this

Dù tiêu đề này không nhắm đến cơ chế `this` gì cả, đây là một đề tài ES6 mà `this` có liên quan đến lexical scope khá quan trọng.

ES6 bổ sung một dạng cú pháp quan trọng cho việc khai báo hàm gọi là "hàm mũi tên" (arrow function). Trông như sau:

```js
var foo = a => {
	console.log( a );
};

foo( 2 ); // 2
```
Nó còn được gọi là "mũi tên bự", thường được gợi ý như là dạng viết tắt của từ khóa `function` *thừa thải rườm rà*.

Nhưng có vấn đề quan trọng hơn khi dùng hàm mũi tên ngoài việc tiết kiệm gõ phím khi khai báo. Ngắn gọn thì đoạn code dưới đây có trục trặc:

```js

var obj = {
	id: "chuẩn",
	cool: function coolFn() {
		console.log( this.id );
	}
};

var id = "chưa chuẩn";

obj.cool(); // chuẩn

setTimeout( obj.cool, 100 ); // chưa chuẩn
```

Vấn đề ở đây là mất ràng buộc `this` tại hàm `cool()`. Có vài cách để xử lý vụ này, nhưng thường thì dùng giải pháp `var self = this;`.

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		var self = this;

		if (self.count < 1) {
			setTimeout( function timer(){
				self.count++;
				console.log( "chuẩn chưa?" );
			}, 100 );
		}
	}
};

obj.cool(); // chuẩn chưa?
```

Không phải dùng cần (sa) quá nhiều, "giải pháp" `var self = this` chỉ phân phối cho toàn bộ hiểu và dùng đúng ràng buộc `this`, và thay vì thoái lui về cái gì đó mà chúng ta cảm thấy thoải mái: lexical scope, `self` trở thành một định danh có thể giải quyết thông qua lexical scope và closure, không quan tâm những gì xảy ra với ràng buộc `this` trên đường đi.

Người ta không thích viết cái gì dài dòng, đặc biệt là khi cứ phải lặp lại mãi. Bởi vậy, một động lực của ES6 là hỗ giảm bớt hoàn cảnh này, thực tế là *sửa* một vấn đề bản chất, như là cái này.

Giải pháp hàm mũi tên của ES6 giới thiệu một hành vi gọi là "lexical this".

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( () => { // gì vậy, hàm mũi tên?
				this.count++;
				console.log( "chuẩn chưa?" );
			}, 100 );
		}
	}
};

obj.cool(); // chuẩn chưa?
```

Giải thích ngắn gọn ở trên cho thấy hàm mũi tên không hành xử tất cả như hàm thông thường khi nói đến ràng buộc `this`. Nó loại bỏ tất cả các quy tắc thông thường của ràng buộc `this`, và thay vào đó lấy ngay giá trị `this` của lexical scope bao ngoài, bất kể nó là gì.

Vì vậy, trong đoạn code trên, hàm mũi tên không nhận được loại bỏ ràng buộc `this` của nó theo một số cách không đoán được, nó chỉ "kế thừa" ràng buộc `this` của hàm `cool()` (điều này đúng nếu ta gọi nó như đã trình bày).

Trong khi nó làm cho code ngắn hơn, quan điểm của tôi là hàm mũi tên chỉ thực sự chỉ làm cho code thành một cú pháp gây sai lầm thường thấy cho lập trình viên nhầm lẫn và pha trộn quy tắc "this binding" với quy tắc "lexical scope".

Ngược lại với quan điểm trên: tại sao lại đi gặp rắc rối và rườm rà bằng cách sử dụng mô hình `this`, chỉ cần cắt bớt và trộn nó với tham chiếu lexical. Việc nắm bắt một trong những cách tiếp cận cho bất kỳ đoạn code mà không phải trộn chúng trong cùng một đoạn code có vẻ tự nhiên hơn.

**Ghi chú:** một nhược điểm khác của hàm mũi tên là chúng vô danh. Xem Chương 3 để biết lý do tại sao hàm vô danh ít được chú trọng bằng hàm định danh.

Theo quan điểm của tôi với vấn đề này thì nên nắm bắt và sử dụng cơ chế `this` một cách chính xác.

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( function timer(){
				this.count++; // `this` an toàn nhờ `bind(..)`
				console.log( "càng chuẩn" );
			}.bind( this ), 100 ); // xem kìa, `bind()`!
		}
	}
};

obj.cool(); // càng chuẩn
```

Dù bạn thích hàm mũi tên với hành vi lexical-this hơn, hay bạn thích sử dụng `bind()` hơn, điều quan trọng cần ghi chú là hàm mũi tên *không chỉ* đơn thuần là đỡ mất công gõ chữ "function". Nó có một *sự khác biệt về hành vi có chủ đích* mà chúng ta cần học và hiểu, nếu ta chọn.

Giờ bạn đã hiểu lexical scope (và closure), hiểu lexical-this cũng không phải là thừa!
