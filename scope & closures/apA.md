# You Don't Know JS: Scope & Closures
# Phụ lục A: Scope động (dynamic scope)

Trong chương 2, ta đã nói về "Dynamic Scope" là tương phản của "Lexical Scope", đó là cách phạm vi hoạt động trong JavaScript (thực tế là nhiều ngôn ngữ khác cũng vậy).

Chúng ta sẽ xem xét một chút nhẹ dynamic scope để coi tương phản đó ra sao. Nhưng quan trọng hơn là dynamic scope thực tế là con cô con cậu của cơ chế khác (`this`) trong JavaScript, ta sẽ coi nó trong phần "*this & Nguyên mẫu đối tượng*".

Như ta đã biết trong Chương 2, lexical scope là một tập hợp các quy tắc về cách tìm kiếm biến như thế nào và ở đâu của *Engine*. Đặc điểm chính của lexical scope là nó được xác định tại author-time khi code được viết ra (giả định bạn không dùng `eval()` hoặc `with` để đánh lừa).

Dynamic scope dường như là ngụ ý rằng có một mô hình mà theo đó phạm vi có thể được mô tả linh hoạt tại runtime hơn là thụ động tại author-time. Xem ví dụ minh họa:

```js
function foo() {
	console.log( a ); // 2
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

Lexical scope giữ tham chiếu RHS đến `a` trong `foo()` sẽ được xử lý thành biến toàn cục `a`, và sẽ có kết quả xuất ra là `2`.

Ngược lại, dynamic scope không quan tâm đến việc khi nào, ở đâu hàm và scope được khai báo, mà đúng hơn là **chúng được gọi từ đâu**. Nói cách khác, chuỗi phạm vi dựa trên call-stack, không phải scope lồng nhau trong code.

Vì vậy, nếu JS có dynamic scope, khi `foo()` được thực thi, **về mặt lý thuyết** thì đoạn code dưới đây sẽ ra kết quả là `3`.

```js
function foo() {
	console.log( a ); // 3  (không phải 2!)
}

function bar() {
	var a = 3;
	foo();
}

var a = 2;

bar();
```

Sao lại vậy? Bởi vì khi `foo()` không thể xử lý tham chiếu biến cho `a`, thay vì nó đi tiếp lên trên đến chuỗi scope được lồng (lexical) tiếp theo, nó đi tiếp tới call-stack để tìm xem `foo()` được gọi gọi từ đâu. Khi `foo()` được gọi từ `bar()`, nó kiểm tra biến trong scope của `bar()` và tìm thấy `a` có giá trị bằng `3`.

Trong giây phút thì có thể bạn nghĩ rằng nó kỳ lạ.

Thực ra là bởi bạn chỉ mới làm việc với (ít nhất là tìm hiểu sâu) code được lexical scope, nên dynamic scope thấy có vẻ xa lạ. Nếu bạn đã từng viết code ở ngôn ngữ dynamic scope, thì đây là bình thường, còn lexical scope mới là xa lạ.

Nói thẳng đuột, JavaScript **không có dynamic scope** mà chỉ có lexical scope. Nhưng mà cơ chế `this` lại là một kiểu dynamic scope.

Mâu thuẫn then chốt: **lexical scope là write-time/author-time trong khi dynamic scope (và `this`!) là runtime**. Lexical scope quan tâm *hàm được khai báo ở đâu*, nhưng dynamic scope quan tâm hàm được *gọi từ đâu*.

Còn `this` thì lại quan tâm việc *hàm được gọi như thế nào*, đây là mối liên hệ của cơ chế `this` với ý tưởng dynamic scope.
