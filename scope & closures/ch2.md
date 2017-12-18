# You Don't Know JS: Phạm vi & Bao đóng
# Chương 2: Lexical Scope (phạm vi từ vựng)

Trong Chương 1, chúng ta đã tìm hiểu "phạm vi" như một tập hợp quy tắc vận hành cách *Engine* có thể tra cứu biến với tên nhận diện có thể là trong *Scope* gần nhất hoặc bất kỳ *Scope lồng nhau*.

Có hai mô hình nổi bật về cách hoạt động của phạm vi. Cái đầu tiên gọi là **Lexical Scope**, là cái thông thường nhất, được sử dụng phần lớn bởi các ngôn ngữ lập trình, chúng ta sẽ tìm hiểu sâu hơn. Mô hình khác gọi là **Dynamic Scope** vẫn được thường sửa dụng ở vài ngôn ngữ (ví dụ như Bash, vài mô hình trong Pearl).

Dynamic Scope được đề cập ở Phụ lục A. Tôi gợi ý nó chỉ để cung cấp một sự tương phản với Lexical scope, một mô hình mà JavaScript sử dụng.

## Lex-time

Theo thảo luận ở Chương 1 - phần 1, giai đoạn truyền thống đầu tiên của trình dịch ngữ được gọi là lexing (hay tonkenizing). Nếu bạn nhớ lại, quá trình lexing kiểm tra một chuỗi các ký tự mã nguồn và gán ngữ nghĩa cho các token như là kết quả của một số trạng thái phân tích cú pháp.

Chính khái niệm này cung cấp nền tảng để hiểu được phạm vi từ vựng là gì và cái tên đến từ đâu.

Lexical scope là một phạm vi được định nghĩa trong thời gian lexing. Nói cách khác, lexical scope dựa trên các biến và khối phạm vi do bạn tạo tại thời điểm viết, do đó hầu hết viên gạch được xây ra từ ngay thời điểm lexer xử lý code của bạn.

**Ghi chú:**
Chúng ta sẽ hơi thấy có vài cách để đánh lừa lexical scope bằng cách sửa đổi nó sau khi lexer đã truyền qua, nhưng việc này không hay. Bài tốt nhất vẫn là hành xử lexical scope với vài trò lexical thôi, mọi thứ đều theo tự nhiên.

Xem đoạn code dưới đây:

```js
function foo(a) {

	var b = a * 2;

	function bar(c) {
		console.log( a, b, c );
	}

	bar(b * 3);
}

foo( 2 ); // 2 4 12
```
Có ba phạm vi lồng nhau ở đoạn code ở trên. Để dễ hình dung thì cứ nghĩ các phạm vi đó như bong bóng bên trong từng cụm.

<img src="fig2.png" width="500">

**Bong bóng 1** bao gồm phạm vi toàn cục, và chỉ có một đối tượng trong nó: `foo`.

**Bong bóng 2** bao gồm phạm vi của `foo`, bao gồm ba đối tượng: `a`, `bar` and `b`.

**Bong bóng 3** bao gồm phạm vi của `bar`, và chỉ có một đối tượng: `c`.

Phạm vi của các bong bóng được xác định ở tại các cụm scope được viết, cái này chồng cái kia,... Trong chương tiếp theo, chúng ta sẽ thảo luận các đơn vị khác của scope nhưng giờ chỉ cần giả định mỗi hàm tạo một scope bong bóng mới.

Bong bóng `bar` được chứa hoàn toàn trong bong bóng `foo`, bởi vì (và chỉ vì) đó là nơi chúng ta chọn để xác định hàm `bar`.

Lưu ý rằng những bong bóng lồng nhau này được lồng một cách chặt chẽ. Chúng ta không nói về biểu đồ Venn, nơi các bong bóng có thể vượt qua ranh giới. Nói khác đi, không có bong bóng cho một số hàm đồng thời tồn tại bên trong hai bong bóng bên ngoài khác, cũng như không có hàm nào có thể một phần bên trong hai hàm cha.

### Tra cứu

Cấu trúc và vị trí tương quan của các bong bóng phạm vi này giải thích đầy đủ cho cho *Engine* tất cả các nơi nó cần đến để tìm nhận diện.

Ở đoạn code ở trên, *Engine* thực thi biểu thức `console.log(..)`  và tìm ba biến tham chiếu `a`, `b`, và `c`. Đầu tiên nó sẽ tìm ở phạm vi bong bóng bên trong nhất là hàm `bar(...)`. Nếu nó không tìm thấy `a` ở đó, nó sẽ đi lên trên một bậc là phạm vi `foo(...)`. Nếu nó tìm thấy `a` ở đó, thì nó sẽ dùng `a`. Tương tự với `b`. Nhưng `c` được thấy bên trong `bar(...)`

Nếu đã có `c` bên trong `bar (..)` và bên trong `foo (.. )`, lệnh `console.log (..)` đã tìm và sử dụng trong `bar ( ..)` rồi, nên không bao giờ là cái trong `foo (..) `.

**Tra cứu phạm vi dừng lại một khi nó "khớp lệnh"**. Tên định danh giống nhau có thể được đặt ở nhiều lớp khác nhau trong phạm vi lồng nhau, được gọi là "đổ bóng" (nhận dạng bên trong "che bóng" nhận dạng bên ngoài). Bất kể là đổ bóng, phạm vi luôn bắt đầu tra cứu từ phạm vi trong nhất, và làm việc từ trong ra ngoài, từ dưới lên trên cho đến khi nó khớp lệnh và dừng lại.

**Ghi chú:** Các biến toàn cục cũng tự động là thuộc tính của đối tượng toàn cục (`window` của các trình duyệt...) vì thế có thể tham chiếu đến một biến toàn cục không trực tiếp bằng tên từ vựng của nó, thay vào đó là gián tiếp bằng một đại diện thuộc tính toàn cục.

```js
window.a
```
Kỹ thuật này cho phép truy cập vào một biến toàn cục, nếu không thì sẽ bị che bóng. Tuy nhiên, không phải biến toàn cục mà không bị che bóng thì cũng không truy cập được.

Cho dù một hàm được gọi ra từ đâu, hay kể cả được gọi như thế nào, phạm vi từ vựng của nó chỉ xác định tại nơi hàm được khai báo.

Quy trình tra cứu phạm vi từ vựng chỉ áp dụng cho lớp nhận dạng đầu tiên, ví dụ như `a`, `b`, và `c`. Nếu bạn có một tham chiếu đến `foo.bar.baz` trong một mẩu code, phạm vi từ vựng sẽ tra cứu đến nhận diện `foo`, một khi đã định vị biến đó rồi, các quy tắc truy cập thuộc tính đối tượng sẽ tiếp tục giải quyết thuộc tính `bar` và `baz` tương ứng.

## Ăn gian từ vựng

Nếu phạm vi từ vựng chỉ được xác định tại nơi hàm được khai báo, hoàn toàn là một quyết định của author-time (tạm không dịch - phân biệt author-time & run-time), làm cách nào có thể có cách để "chỉnh sửa" (ăn gian) phạm vi từ vựng tại run-time?

JavaScript có hai cơ chế như vậy. Cả hai đều khó chịu như nhau như là một trải nghiệm xấu trong code của bạn. Nhưng những lập luận phản đối đều thường thiếu điểm quan trọng nhất: **ăn gian phạm vi từ vựng dẫn đến hiệu suất nghèo nàn.**

Trước khi tôi giải thích về vấn đề hiệu suất, hãy xem hai cơ chế đó như thế nào.

### `eval`

Hàm `eval(..)` trong JS lấy một chuỗi như là một đối số và xử lý các nội dung của chuỗi như thể nó thực sự là tác giả code (authored) tại thời điểm đó. Nói cách khác, bạn có thể lập trình tạo code bên trong authored code, và chạy code mới tạo đó như thể là nó đã ở đó tại thời điểm author time.

Đánh giá (chơi chữ) `eval(..)` ở đây, cần làm rõ `eval(..)` cho phép bạn sửa đổi môi trường phạm vi từ vựng bằng cách đánh lừa và giả vờ rằng author-time code đó đã tồn tại như thế nào.

Trên các dòng mã tiếp theo sau khi `eval(..)` được thực thi, *Engine* sẽ không biết hoặc quan tâm rằng code trước đó đang được diễn dịch động và do đó đã thay đổi môi trường phạm vi từ vựng. *Engine* chỉ đơn giản thực hiện tra cứu phạm vi từ vựng theo cách nó luôn hoạt động.

Xem ví dụ sau:

```js
function foo(str, a) {
	eval( str ); // ăn gian!
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1, 3
```

Chuỗi `"var b = 3;"` được xử lý như là nó đã tồn tại tại hàm `eval(..)`. Bởi vì code đó xảy ra để khai báo biến `b` mới, nó sửa đổi phạm vi từ vựng hiện tại của `foo(..)`. Thực tế như đã đề cập ở trên thì code đã tạo biến `b` trong `foo(..)` đã che bóng `b` khai báo ở phạm vi bên ngoài (toàn cục).

Khi thực thi `console.log(..)`, nó tìm cả `a` và `b` trong phạm vi của `foo(..)`, và không bao giờ tìm `b` bên ngoài. Vì vậy, chúng ta in ra "1, 3" thay vì "1, 2" như trường hợp thông thường.

**Ghi chú:** Trong ví dụ này, cho đơn giản, chuỗi của "code" chúng ta truyền vào là một chuỗi trực kiện cố định. Nhưng nó có thể dễ dàng tạo ra bằng cách thêm các ký tự dựa trên logic chương trình. `eval(..)` thường được sử dụng để thực thi tạo code động, nhưng việc định lượng động code tĩnh từ một chuỗi trực kiện thường không mang một lợi ích thực tế nào thay cho chỉ cần tạo code trực tiếp.

Mặc định một chuỗi code mà `eval(..)` thực thi chứa một hay nhiều khai báo (bao gồm biến và hàm), hoạt động này sửa đổi phạm vi từ vựng hiện hữu tại nơi `eval(..)` tồn tại. Về mặt kỹ thuật, `eval(..)` có thể được gọi "gián tiếp" bằng một vài thủ thuật (ngoài phạm trù thảo luận) làm cho nó thay vì thực thi ở bối cảnh toàn cục, thì lại sửa đổi nó. Nhưng trong cả hai trường hợp, `eval(..)` có thể sửa đổi theo runtime một phạm vi từ vựng author-time.

**Ghi chú:** `eval(..)` khi được sử dụng ở strict-mode nó hoạt động trong phạm vi từ vựng của chính nó, nghĩa là các khai báo được tạo bên trong `eval()` không thực sự sửa đổi phạm vi.

```js
function foo(str) {
   "use strict";
   eval( str );
   console.log( a ); // ReferenceError: a không xác định
}

foo( "var a = 2" );
```

Có một điều kiện dễ dàng khác trong JavaScript mang lại hiệu ứng tương tự như `eval(..)`. `setTimeout(..)` và `setInterval(..)` *có thể* lấy một chuỗi cho đối số đầu tiên của nó, nội dung được định lượng giống như code của hàm tự tạo ra. Đừng sử dụng cách này, từ lâu người ta đã không tán thành.

Phương thức khởi tạo `new Function(..)` cũng lấy một chuỗi của code trong đối số **cuối** trong nó để biến thành một hàm tự tự động (các đối số đầu tiên), nếu có,là các tham số cho hàm mới). Cú pháp hàm khởi tạo này an toàn hơn `eval(..)`, nhưng cũng nên tránh sử dụng.

Việc sử dụng tự tạo code động trong chương trình thực sự rất hiếm, vì nó làm xuống cấp hiệu suất và hầu như không bao giờ đáng.

### `with`

Tính năng khác nữa (đã hết sử dụng) trong JS để lừa phạm vi từ vựng là `with`. Có vài cách hợp lệ để giải thích cho `with`, nhưng tôi chọn hướng giải thích theo góc độ cách nó tương tác và ảnh hưởng đến phạm vi từ vựng.

`with` được giải thích như là một cách ngắn gọn để tạo nhiều tham chiếu đối với một object mà bản thân nó không phải lặp lại tham chiếu object nhiều lần.

Ví dụ:

```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// sự lặp lại "obj" một cách "tẻ nhạt"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// cách giản tiện hơn
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}
```

Tuy nhiên, có nhiều thứ xảy ra hơn đơn giản chỉ là cách giản tiện cho cách try cập object:

```js
function foo(obj) {
	with (obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo( o1 );
console.log( o1.a ); // 2

foo( o2 );
console.log( o2.a ); // không xác định
console.log( a ); // 2 -- Oops, bị rò ra ngoài toàn cục!
```

Trong ví dụ, hai object `o1` và `o2` được tạo ra. Một có thuộc tính `a`, còn cái khác thì không có. Hàm `foo(...)` lấy tham chiếu `obj` như là một tham số, và gọi `with (obj) { .. }` về tham chiếu đó. Bên trong khối `with`, ta làm cho những gì hiện hữu trở thành tham chiếu từ vựng thông thường cho biến `a`, tham chiếu LHS (xem Chương 1) xảy ra, gán giá trị `2` cho nó.

Khi chúng ta truyền vào `o1`, phép gán `a = 2` tìm thuộc tính `o1.a` và gán giá trị `2` như phản hồi của `console.log(o1.a)`. Tuy nhiên, khi chúng ta truyền vào `o2`, nó không có thuộc tính `a`, chẳng có gì được tạo ra nên `o2.a` giữ nguyên `undefined`.

Nhưng chúng cần lưu ý một hiệu ứng phụ đặc biệt, đó là biến toàn cục `a` được tạo bởi phép gán `a = 2`. Sao lại như vậy?

Lệnh `with` lấy một object không hoặc có các thuộc tính, và **xử lý object như thể nó là một phạm vi từ vựng riêng biệt hoàn toàn**, và do đó các thuộc tính của object được coi như là các định danh từ vựng trong "phạm vi".

**Ghi chú:** Mặc dù  một khối `with` xử lý đối tượng như một phạm vi từ vựng, khai báo `var` thông thường bên trong `with` sẽ không được mở rộng đến khối `with`, thay vào đó là chứa phạm vi hàm.

Trong khi hàm `eval(..)` có thể thay đổi phạm vi từ vựng hiện tại nếu nó có một chuỗi của code với một hay nhiều khai báo bên trong, lệnh `with` lại thực sự tạo ra **một phạm vi hoàn toàn mới** từ object mà bạn đưa vào nó.

Hiểu theo cách này, "phạm vi" được khia báo bởi lệnh `with` khi ta đưa vào `o1` là `o1`, và "phạm vi" này có "nhận diện" trong đó tương ứng thuộc tính `o1.a`. Nhưng khi chúng ta dùng `o2` như là "phạm vi", nó không có nhận diện `a` bên trong, và nguyên tắc của tra cứu nhận diện LHS được thực hiện.

Không phải "phạm vi" của `o2`, hay phạm vi của `foo(..)`, hay kể cả phạm vi toàn cục, có nhận diện `a` được tìm thấy, vì vậy khi `a = 2` được thực thi, kết quả của nó là sự tự tạo của toàn cục (dùng ở non-strict mode).

Muốn thấy `with` chuyển hướng một object và thuộc tính của nó thành một phạm vi với các nhận diện tại runtime là kiểu suy nghĩ lệch lạc lạ lùng. Nhưng nó là cái giải thích rõ ràng nhất mà tôi có thể trình bày kết quả.

**Ghi chú:** Cộng thêm đây là ý tưởng tồi để sử dụng, cả `eval(..)` và `with` đều bị ảnh hưởng (cản trở) bởi Strict Mode. `with` thì hoàn toàn không được phép, trong khi một vài dạng gián tiếp hay không an toàn như `eval(..)` không được phép khi giữ chức năng cốt lõi.

### Hiệu suất

Cả `eval(..)` và `with` gian lận để author-time làm trái việc xác định phạm vi từ vựng bằng cách sửa đổi hay tạo phạm vi từ vựng mới tại runtime.

Nếu bạn hỏi, có lợi hại gì? nếu người cho thêm nhiều tính năng phức tạp và linh hoạt, chẳng phải là tốt hay sao? **Không**

JavaScript *Engine* có một lượng tối ưu hiệu suất được thực hiện trong suốt quá trình biên dịch. Một số đã giản gọn để có thể phân tích tĩnh cần thiết và xác định trước chỗ tất cả các biến và hàm khai báo, để nó có thể giảm tải việc giải quyết các định danh trong quá trình thực thi.

Nhưng nếu *Engine* tìm thấy một `eval(..)` hoặc `with` trong code, về cơ bản nó phải thừa nhận tất cả nhận thức về vị trí định danh không hợp lệ, bởi vì nó không thể biết tại lexing time (tạo từ vựng) có chính xác bao nhiêu code được đưa vào `eval(..)` để sửa đổi phạm vi từ vựng, hoặc nội dung của object bạn có thể chuyển vào `with` để tạo ra phạm vi từ vựng mới.

Nói theo nghĩa bi quan, hầu hết những tối ưu hóa mà nó tạo ra đều vô nghĩa nếu `eval(..)` hay `with` tồn tại, vì nó đơn giản không thực hiện tối ưu hóa nào cả.

Code của bạn sẽ hầu như chắc chắn chạy chậm hơn bởi việc bạn thêm `eval(..)` hay `with` bất kỳ đâu trong code. Dù cho *Engine* có thông minh cỡ nào không thể giới hạn những hiệu ứng phụ của những giả định bi quan đó.

## Ôn tập (TL;DR)

Phạm vi từ vựng nghĩa là phạm vi được xác định bởi author-time tại nơi hàm được khai báo. Giai đoạn lexing của biên dịch cần xảy ra để biết tất cả các định danh được khai báo ở đâu và ra sao, và cả dự đoán việc chúng sẽ được tìm như thế nào trong quá trình thực thi.

Hai cơ chế trong JavaScript  có thể "đánh lừa" phạm vi từ vựng: `eval(..)` và `with`. Dạng này có thể sửa đổi phạm vi từ vựng (tại runtime) bằng cách đánh giá một chuỗi các mã được khai trong đó, hay là tạo ra toàn bộ phạm vi từ vựng mới tại (runtime) bằng cách hành xử tham chiếu object như là "phạm vi" và các thuộc tính của object đó như là định danh của phạm vi.

Nhược điểm của cơ chế này là đánh bạnh khả năng thực hiện tối ưu trong quá trình biên dịch của *Engine* liên quan đến phạm vi tra cứu, bởi *Engine* phải giả định rằng tối ưu hóa như vậy sẽ không hợp lệ. Code sẽ chạy chậm hơn khi sử dụng những tính năng này. **Đừng dùng chún**
