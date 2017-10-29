# You Don't Know JS: Up & Going
# Chương 2: Hiểu về JavaScript

Trong chương trước, tôi giới thiệu những cụm căn bản của lập trình như là biến, vòng lặp, điều kiện, hàm. Tất nhiên, tất cả các code đều trình diễn theo ngôn ngữ JS. Nhưng trong chương này, tôi muốn tập trung đặc biệt vào những điều cần biết về JS để bạn khởi đầu trở thành một lập trình viên JS.

Chúng ta sẽ giới thiệu một vài nguyên lý trong chương này một cách không đầy đủ cho đến những phần tiếp theo bộ sách *YDKJS*. Bạn có thể nghĩ chương này là một mục khái quát các chuyên đề trong series.

Đặc biệt nếu JS còn mới mẻ đối với bạn, bạn cần bỏ ít thời gian để xem lại các lý thuyết và ví dụ nhiều lần. Bất kỳ một nền tảng tốt nào cũng phải đặt từng viên gạch một, cho nên đừng hy vọng bạn ngay lập tức hiểu liền trong lần đầu.

Cuộc phiêu lưu của bạn để học sâu về JS bắt đầu từ đây.

**Ghi chú:** Như tôi đề cập ở Chương 1, bạn phải tự mình thử tất cả các code khi bạn học trong suốt chương này. Chú ý rằng một vài chỗ phù hợp với phiên bản mới nhất của JS tại thời điểm viết bài này (thường gọi là "ES6" cho phiên bản 6 của ECMAScript -- tên chính thức của JS). Nếu bạn định sử dụng phiên bản trình duyệt cũ hơn trước thời ES6, code có thể không hoạt động. Bạn cần nâng cấp trình duyệt.

## Giá trị & Kiểu

Như chúng ta đã khẳng định trong Chương 1, JS có kiểu của giá trị, không có kiểu của biến. Các kiểu có sẵn là:

* `string`
* `number`
* `boolean`
* `null` and `undefined`
* `object`
* `symbol` (new to ES6)

JavaScript có một biểu thức `typeof` có thể kiểm tra giá trị và cho bạn biết kiểu của nó là gì.

```js
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"
```

Giá trị trả lại từ biểu thức `typeof` luôn là một trong sáu kiểu ở dạng giá trị string( 7 đối với ES6 -- kiểu "symbol"). Đó là, `typeof "abc"` trả lại `"string"`, không phải `string`.

Chú ý rằng đoạn code này, biến `a` giữ mọi kiểu của giá trị, và mặc dù là có bề ngoài, `typeof a` sẽ không hỏi "kiểu của `a`", mà là "kiểu của giá trị hiện tại trong `a`." Trong JS chỉ có giá trị mới có kiểu; biến chỉ đơn giản là vật chứa các giá trị đó.

`typeof null` là một trường hợp thú vị, bởi vì nó trả sai thành `"object"`, trong khi bạn mong muốn nó trả `"null"`.

**Cảnh báo:** Đây là một lỗi trường kỳ của JS và dường như sẽ không bao giờ được sửa. Quá nhiều code trên web liên quan đến lỗi mà cố sửa nó thì nó sẽ tạo ra nhiều lỗi hơn.

Đồng thời, chú ý `a = undefined`. Chúng ta thiết lập rõ ràng `a` là giá trị `undefined`, nhưng nó cũng có hành vi không khác với biến chưa có giá trị, ví dụ như `var a;` ở đoạn code bên trên. Một biến có thể nhận trạng thái "undefined" này bằng nhiều cách khác nhau, bao gồm hàm trả lại không có giá trị và các sử dụng `void`.

### Objects

Kiểu `object` đề cập đến một phức hợp giá trị mà bạn có thể lập các thuộc tính (vị trí được đặt tên) mà mỗi cái đều có thể có giá trị của riêng chúng với bất kỳ kiểu nào. Đây có lẽ là một trong nhưng kiểu hữu dụng nhất trong JS.

```js
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
```

Thật hữu ích khi xem giá trị của `obj` một cách trực quan:

<img src="fig4.png">

Thuộc tính có thể truy cập bằng *dấu chấm* (vd: `obj.a`) hay *dấu ngoặc* (vd: `obj["a"]`). Dấu chấm ngắn hơn và dễ đọc hơn, vì vậy nó được ưa thích hơn.

Dấu ngoặc hữu dụng nếu bạn có một tên thuộc tính có ký tự đặc biệt trong nó, `obj["hello world!"]` -- các thuộc tính như vậy thường được là *chìa khóa* khi truy cập thông qua dấu ngoặc. Dấu ngoặc `[ ]` đòi hỏi phải có một biến (giải thích sau) hoặc một `string` *nguyên bản* ( được bao bởi `" .. "` hoặc `' .. '`).

Đương nhiên, dấu ngoặc cũng hữu dụng nếu bạn muốn tiếp cận một thuộc tính/chìa khóa nhưng tên được lưu trữ ở biến khác, như là:

```js
var obj = {
	a: "hello world",
	b: 42
};

var b = "a";

obj[b];			// "hello world"
obj["b"];		// 42
```

**Ghi chú:** Xem thêm tập *this & Object Prototypes* đặc biệt ở Chương 3 để biết thêm về `object`.

Có một vài kiểu giá trị mà bạn có thể tương tác với chương trình JS: *array* và *function*. Nhưng thay vì là các kiểu dựng sẵn phù hợp, nó phải được coi như là các kiểu thứ cấp -- phiên bản đặc biệt của kiểu `object`.

#### Mảng (array)

Trong mảng là `object` giữ các giá trị (của bất kỳ kiểu nào) có vị trí theo chỉ số, chứ không phải theo trong một khóa/ thuộc tính được đặt tên. Ví dụ:

```js
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```

**Ghi chú:** Các ngôn ngữ thường được đếm từ 0, JS cũng dùng `0` như là chỉ mục của giá trị đầu tiên trong mảng.

Để dễ hình dung về `arr` hãy xem hình dưới:

<img src="fig5.png">

Bởi vì mảng là một object đặc biệt (như `typeof` đã ngụ ý), nó có thể có thuộc tính, bao gồm thuộc tính `length` cũng được tự động cập nhật.

Về mặt lý thuyết, bạn có thể sử dụng mảng như một object bình thường với tên thuộc tính tự bạn đặt, hoặc bạn có thể sử dụng một `object` nhưng chỉ cho các thuộc tính số (`0`, `1`, ...) tương tự như một mảng (wtf). Tuy nhiên, điều này thường được coi là sử dụng không đúng loại tương ứng.

Cách tự nhiên và tốt nhất là sử dụng mảng cho các giá trị được xác định vị trí theo số và sử dụng `object` cho các thuộc tính có tên.

#### Hàm

Một kiểu `object` con khác mà bạn sẽ sử dụng trong suốt chương trình JS là hàm:

```js
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

Một lần nữa, hàm là kiểu con của `objects` -- `typeof` trả kết quả `"function"` để hàm ý rằng `function` là một kiểu chính -- và có thể có các thuộc tính, nhưng bạn chỉ thường sử dụng thuộc tính đối tượng hàm trong trường hợp giới hạn (ví dụ `foo.bar`).

**Chú ý:** Thông tin chi tiết về giá trị và kiểu của JS, xem hai chương đầu của phần *Kiểu & Ngữ pháp*.

### Các phương thức kiểu dựng sẵn

Những kiêu dựng sẵn và kiểu con mà chúng ta vừa bàn luận có hành vi rõ ràng như các thuộc tính và phương thức của nó rất mạnh mẽ, hữu dụng.

Ví dụ:

```js
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```

Việc "làm cách nào" đằng sau lệnh `a.toUpperCase()` phức tạp hơn chỉ là phương thức hiện có trong giá trị.

Tóm tắt thì nó là một dạng bao lấy object `String` (viết hoa `S`), thường được gọi là "native," cặp với `string` nguyên thủy; đó là object bao ngoài định nghĩa phương thức `toUpperCase()` trên nguyên mẫu của nó.

Khi bạn sử dụng giá trị ban đầu `"hello world"` như một `object` bằng cách tham chiếu một thuộc tính hay phương thức (vd: `a.toUpperCase()` trong đoạn code trên), JS tự động "đóng hộp" giá trị cho đối tượng bao ngoài của nó.

Một giá trị `string` có thể được bao bởi một `String` object, một `number` có thể được bao bởi `Number` object, và `boolean` được bao bởi `Boolean` object. Phần lớn, bạn không cần phải lo lắng hoặc trực tiếp sử dụng những dạng bao object của giá trị -- Sử dụng định dạng giá trị nguyên thủy trong thực tế, JS sẽ lo phần còn lại.

**Ghi chú:** Để biết thêm về các bản chất của JS và "đóng hộp", xem Chương 3 *Kiểu & Ngữ pháp*. Để hiểu hơn về prototype và object, xem Chương 5 *this & Object Proptotypes*

### So sánh các giá trị.

Có hai kiểu so sánh giá trị chính mà bạn cần thực hiện trong chương trình JS: *bằng nhau* và không *bằng nhau*. Kết quả của bất kỳ sự so sánh nào đều là `boolean` (`true` hoặc `false`), bất kể kiểu già trị nào được so sánh.

#### Sự cưỡng bức

Chúng ta đã nói ngắn gọn về sự ép buộcở Chương 1, nhưng chúng ta sẽ thảo luận thêm ở đây.

Sự ép buộcxuất hiện trong 2 dạng của JavaScript: *minh bạch* và *tiềm ẩn*. Sự ép buộcminh bạch đơn giản là bạn thấy rõ ràng trong code có một sự chuyển đổi từ dạng này sang dạng khác, trong khi đó sự ép buộctiềm ẩn là khi chuyển đổi dạng có thể xảy ra nhiều hơn một hiệu ứng phụ của một số hoạt động khác.

Bạn có thể từng nghe ý kiến rằng "sự ép buộc là ma quỷ" được nêu ra trong một số sự kiện tại một vị trí rõ ràng nào đó, nơi mà sự ép buộc có thể tạo nên những kết quả bất ngờ. Có lẽ đối với các developer thì không có gì thất vọng hơn khi một ngôn ngữ làm họ bất ngờ.

Sự ép buộc không phải là ma quỷ cũng như mang lại sự bất ngờ. Thực tế, phần lớn các trường hợp chính bạn có thể xây dựng với kiểu ép buộc hợp lý và dễ hiểu, thậm chí còn có thể sử dụng để *cải thiện* khả năng đọc code. Nhưng chúng ta không đi sâu vào tranh luận điều này. -- Chương 4 *Kiểu & Ngữ pháp* sẽ giải quyết hết các vấn đề.

Dưới đây là ví dụ của ép buộc *minh bạch*::

```js
var a = "42";

var b = Number( a );

a;				// "42"
b;				// 42 -- số!
```

Và dưới đây là ví dụ của ép buộc *không minh bạch*:

```js
var a = "42";

var b = a * 1;	// "42" ép buộc ngầm 42 tại đây

a;				// "42"
b;				// 42 -- số!
```

#### Đúng và Sai

Trong chương 1, chúng ta đã đề cập ngắn gọn đến tính "truthy" và "falsy" tự nhiên của giá trị: khi một giá trị không phải `boolean` bị ép thành `boolean`, nó có trở thành tính `true` hay `false` tương ứng?

Danh sách cụ thể của giá trị "falsy" trong JS:

* `""` (chuỗi rỗng)
* `0`, `-0`, `NaN` (`number` không hợp lệ)
* `null`, `undefined`
* `false`

Bất kỳ giá trị nào không phải "falsy" thì là "truthy". Đây là ví dụ:

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)

Điều quan trọng là phải nhớ rằng giá trị `boolean` chỉ theo sự cưỡng ép "truthy"/"falsy" nếu nó bị ép theo `boolean`. Cũng không phải khó khăn gì để tự nhầm lẫn với một tình huống có vẻ như nó đang buộc một giá trị thành `boolean` khi nó không phải.

#### Đẳng thức

Có bốn loại đẳng thức: `==`, `===`, `!=`, và `!==`. Dạng `!` tất nhiên là bản "không bình đẳng" đối xứng với các đối chiếu của nó; không nên nhầm lẫn giữa *không bằng tuyệt đối* và *không bằng tương đối*.

Sự khác biệt giữa `==` và `===` thường được đặc trưng hóa rằng `==` kiểm tra bằng nhau của giá trị và `===` kiểm tra bằng nhau cả giá trị lẫn kiểu. Tuy nhiên, điều này không đúng. Cách thích hợp để đặc trưng hóa chúng là `==` kiểm tra bằng nhau của giá trị với việc ép buộc được cho phép, và `===` không cho phép cưỡng bức; `===` thường được gọi là "bình đẳng nghiêm ngặt" (strict equality) vì lý do này.

Xem sự ép buộc ngầm cho phép bằng cách so sánh bình đẳng `==` và không được phép với sự ép buộc nghiêm ngặt `===`:

```js
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false
```

Trong việc so sánh `a == b`, JS nhận thấy rằng kiểu không trùng khớp, nên nó phải qua một loạt các bước theo trình tự ép buộc một hoặc cả hai giá trị khác kiểu cho đến khi chúng phù hợp, tại điểm này một phép so sánh giá trị đơn giản có thể được kiểm tra.

Nếu bạn nghĩ, có hai cách để `a == b` có thể `true` thông qua ép buộc. Hoặc nó được so sánh là `42 == 42` hoặc nó là `"42" == "42"`, thì nó là cái nào?

Câu trả lời:  `"42"` trở thành `42`, để so sánh `42 == 42`. Trong ví dụ đơn giản, nó cũng không thực sự quan trọng tiến trình nào xảy ra, vì kết quả cũng như nhau. Nhưng đối với trường hợp phức tạp hơn thì nó quan trọng bởi vì nó không chỉ là kết quả của việc so sánh, mà là *làm thế nào* để dẫn tới điều này.

`a === b` tạo nên `false` bởi vì sự ép buộc không cho phép, vì vậy sự so sánh đơn giản của giá trị đương nhiên sai. Nhiều lập trình viên cảm thấy `===`có thể dự đoán được, vậy nên họ hay dùng mẫu này và tránh xa `==`. Tôi nghĩ cách nhìn này khá ngắn. Tôi tin `==` là một công cụ quan trong để giúp chúng ta lập trình, *nếu bạn bỏ thời gian học cách nó hoạt động thế nào*

Chúng ta sẽ không bao hết chi tiết nền tảng về phương thức so sánh `==` ép buộc như thế nào. Hầu hết khá là hợp lý, nhưng cũng có một vài trường hợp góc cạnh quan trọng cũng nên cẩn thận. Bạn có thể đọc phần 11.9.3 của đặc tính ES5 (http://www.ecma-international.org/ecma-262/5.1/) để biết luật chính xác, và bạn sẽ ngạc nhiên cơ chế này đơn giản thế nào khi so sánh với những cường điệu trái ngược xung quanh nó.

Để làm rõ toàn bộ những chi tiết của vài điều cần ghi nhớ đơn giản, giúp bạn biết được khi nào thì dùng `==` hay `===`, tôi liệt kê một số nguyên tắc sau:

* Nếu một trong hai bên trong phép so sánh là có thể là giá trị `true` hoặc `false`, tránh dùng `==` mà dùng `===`.
* Nếu một trong hai bên trong phép so sánh là có thể một giá trị cụ thể (`0`, `""`, hoặc `[]` -- array rỗng), tránh `==` mà dùng `===`.
* Trong *tất cả* các trường hợp khác, bạn yên tâm dùng `==`. Không chỉ vì nó an toàn, đơn giản là giúp code của bạn dễ đọc hơn trong nhiều trường hợp.

Những nguyên tắc trên là để yêu cầu bạn suy nghĩ nghiêm túc về code của bạn và về những loại giá trị nào có thể đi qua các biến để được so sánh bằng. Nếu bạn chắc chắn về giá trị, và `==` an toàn, hãy dùng nó! Nếu bạn không thể chắc chắn về giá trị, sử dụng `===`. Đơn giản vậy thôi.

Ký hiệu không bằng `!=` đi cặp với `==`, và `!==` đi với `===`. Tất cả các quy tắc và quan sát chúng ta vừa thảo luận đều cũng được áp dụng với phép so sánh không bằng này.

Bạn nên chú ý đặc biệt về các so sánh `==` và `===` khi so sánh hai giá trị không phải là nguyên thủy, như là `object` ( bao gồm `function` và `array`). Bởi vì các giá trị đó là tham chiếu, cả `==` và `===` đều kiểm tra khi nào các tham chiếu phù hợp chứ không phải các giá trị bên trong.

Ví dụ, `array` được mặc định ép buộc sang `string` bằng cách đơn giản gộp tất cả các giá trị với dấu phẩy (`,`) ở giữa. Bạn có thể nghĩ là hai `array` với nội dung như nhau có thể là `==`, nhưng không phải:

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

**Chý ý:** Để biết thêm về quy tắc so sánh `==`, xem ES5 specification (section 11.9.3) và cũng có bàn bạc trong Chương 4 của *Kiểu & Ngữ pháp* trong bộ này; xem Chương 2 để biết thêm về giá trị với tham chiếu.

#### Bất bình đẳng

Toán tử `<`, `>`, `<=`, và `>=` sử dụng cho bất bình đẳng, được đề cập trong đặc là như là "so sánh quan hệ". Thông thường chúng được sử dụng với các giá trị so sánh theo thứ tự như `number`. Như `3 < 4` cũng dễ hiểu.

Nhưng giá trị JavaScript `string` cũng có thể được so sánh bất bình đẳng, sử dụng nguyên tắc alphabe thông thường (`"bar" < "foo"`).

Về sự ép buộc thì sao? Tương tự như nguyên tắc so sánh `==` (mặc dù không giống hệt vậy) áp dụng trong toán tử bất bình đẳng. Đáng chú ý, không có toán tử "bất bình đẳng nghiêm ngặt" không cho phép ép buộc như `===` như trong "bình đẳng nghiêm ngặt".

Consider:

```js
var a = 41;
var b = "42";
var c = "43";

a < b;		// true
b < c;		// true
```

Chuyện gì đang xảy ra? ở phần 11.8.5 của đặc tính kỹ thuật ES5, nó nói rằng nếu cả hai giá trị trong so sánh `<` đều là `string`, như ví dụ `b < c` trên, việc so sánh được thực hiện bằng từ điển học (??). Nhưng nếu một trong hai giá trị không phải là `string`, như so sánh `a < b`, thì cả hai giá trị bị ép thành `number`, và phép so sánh số thông thường diễn ra.

Hãy nhớ rằng là không có "bất bình đẳng nghiêm ngặt" để sử dụng, cái hay nhất là bạn có thể sử dụng phép so sánh với nhiều kiểu giá trị khác nhau khi một trong các giá trị không phải là một giá trị số hợp lệ, ví dụ:

```js
var a = 42;
var b = "foo";

a < b;		// false
a > b;		// false
a == b;		// false
```

Khoan, cách nào mà tất cả ba phép so sánh trên có thể `false`? Bởi vì giá trị `b` bị ép buộc thành "một giá trị number không hợp lệ" `NaN` trong `<` và `>`, và đặc điểm kỹ thuật cho biết `NaN` có thể là lớn hoặc nhỏ hơn bất kỳ một giá trị khác.

Phép so sánh `==` thất bại vì một lý do khác. `a == b`  có thể thất bại kể cả được thông dịch ở cả hai cách `42 == NaN` hay `"42" == "foo"` -- như chúng ta đã giải thích lúc trước.

**Chú ý:** Để biết thêm thông tin về luật của phép so sánh bất bình đẳng, xem phần 11.8.5 của đặc tính kỹ thuật ES5, đồng thời tham khảo đề mục *Kiểu & Ngữ pháp* Chương 4 của bộ sách này.

## Biến

Trong JavaScript, tên biến (bao gồm tên hàm) phải là *nhận diện* hợp lệ. Sự nghiêm ngặt và hoàn chỉnh của các nguyên tắc cho các ký tự hợp lệ trong việc định danh hơi phức tạp một chút khi bạn xem xét các ký tự không phổ biến như là Unicode. Nếu bạn chỉ xem xét các ký tự ASCII tiêu biểu thì các quy tắc lại trở nên đơn giản.

Một định danh nên bắt đầu với `a`-`z`, `A`-`Z`, `$`, hay `_`. Và nó có thể chứa bất kỳ các ký tự như vậy cùng với số từ `0`-`9`.
Nói chung, các quy tắc tương tự áp dụng cho tên thuộc tính như là một biến số nhận diện. Tuy nhiên, một số từ nhất định không thể được sử dụng như các biến, nhưng cũng OK khi đặt tên thuộc tính. Những từ này gọi là từ "dành riêng", và bao gồm các từ khóa JS (`for`, `in`, `if`, v.v...) cũng như `null`, `true`, và `false`.

**Ghi chú:** Để biết thêm thông tin về từ dành riêng, xem Phụ lục A của tập *Kiểu & ngữ pháp*

### Function Scopes (Phạm vi hàm)

Bạn sử dụng từ khóa `var` để khai báo biến cho scope chức năng gần nhất, hoặc là scope toàn cục nếu nó nằm ở tầng trên cùng ngoài tất cả các function.

#### Hoisting

Khi bất kỳ một `var` xuất hiện bên trong phạm vi, việc khai báo có thể thực hiện mọi nơi trong toàn bộ scope đó.

Một cách ẩn dụ, hành vi này gọi là *hoisting*, khi khai báo `var` "di chuyển" lên trên đầu scope của chính nó. Về mặt kỹ thuật, quá trình này được giải thích chính xác hơn bằng code được biên dịch như thế nào, nhưng tạm thời chúng ta bỏ qua chi tiết.

Ví dụ:

```js
var a = 2;

foo();					// hoạt động nhờ khai báo `foo()` được "dời lên"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// khai báo được "dời" lên trên đầu của `foo()`
}

console.log( a );	// 2
```

**Chú ý:** Đây không thường là một ý hay khi dựa vào biến *hoisting* để sử một biến trước đó trong scope của nó hơn là `var` được khai báo bởi chính nó, điều này có thể gây bối rối. Cách thông thường và được chấp nhận để sử dụng hàm *hoisted*, là gọi trước khi nó được khai báo như chúng ta làm với `foo()`.

#### Scopes lồng nhau.

Khi bạn khai báo một biến, nó có hiệu lực ở toàn bộ trong scope đó, kể cả scope con. Ví dụ:

```js
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b );		// 1 2
	}

	bar();
	console.log( a );				// 1
}

foo();
```

Chú ý là `c` khôn có bên trong `bar()`, bởi vì nó chỉ được khai báo bên trong scope `baz()`, và tương tự `b` không có trong `foo()`.

Nếu bạn muốn tiếp cận giá trị của một biến trong một scope không có nó, bạn sẽ gặp lỗi `ReferenceError`. Nếu bạn cố lập một biến chưa được khai báo, bạn cũng sẽ vô tình tạo một biến ở tầng cao nhất - toàn cục (bad!) hoặc gặp lỗi, tùy vào "strict mode" (xem "Strict Mode"). Hãy xem ví dụ:

```js
function foo() {
	a = 1;	// `a` không được khai báo thông thường
}

foo();
a;			// 1 -- oops, tự động trở thành biến toàn cục :(
```

Đây là một trường hợp vô cùng tệ. Đừng làm điều này, bạn phải luôn khai báo biến một cách bình thường.

Trong phương thức khai báo biến cho function, ES6 *cho phép* bạn khai báo các biến thuộc về các khối (block) riêng biệt `{...}` sử dụng từ khóa `let`. Bên cạnh các sắc thái chi tiết, nguyên tắc của scope sẽ hoạt động y với những gì chúng ta đã thấy ở các function:

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

Bởi vì sử dụng `let` thay vì `var`, `b` chỉ thuộc về biểu thức `if` chứ không phải toàn bộ scope của function `foo()`. Tương tự, `c` chỉ thuộc về vòng lặp `while`. Cụm phạm vi hóa rất hữu ích để quản lý biến scope theo phương thức tốt hơn, giúp cho code của bạn dễ dàng bảo trì.

**Ghi chú:** Xem thêm phần *Scope & Closures* để nắm rõ hơn. Đối với phạm vi hóa `let`, xem phần *ES6 & Beyond*.

## Điều kiện

Thêm một số thông tin về biểu thức `if` chúng ta đã giới thiệu ngắn gọn trong Chương 1, Javascript cung cấp một vài cơ chế mà chúng ta cần xem qua.

Đôi khi bạn sẽ thấy mình viết một loạt điều kiện `if..else..if` như sau:

```js
if (a == 2) {
	// làm gì đó
}
else if (a == 10) {
	// làm gì đó khác
}
else if (a == 42) {
	// làm gì đó khác nữa
}
else {
	// không thì sẽ là
}
```

Cấu trúc này hoạt động, nhưng hơi bị rườm rà vì bạn cần kiểm thử `a` cho mỗi trường hợp. Đây là một phương thức khác với biểu thức `switch`:

```js
switch (a) {
	case 2:
		// làm gì đó
		break;
	case 10:
		// làm gì đó khác
		break;
	case 42:
		//  làm gì đó khác nữa
		break;
	default:
		// không thì sẽ là
}
```

`break` quan trọng nếu bạn muốn mỗi biểu thức trong một `case` hoạt động. Nếu bạn bỏ sót `break` ở một `case`, và `case` đó chạy, sự thực thi sẽ tiếp tục đến biểu thức `case` tiếp theo vì `case` kia đã khớp. Cái này được gọi là "thất bại" đôi khi lại hữu dụng:

```js
switch (a) {
	case 2:
	case 10:
		// làm gì đó
		break;
	case 42:
		// làm gì đó khác
		break;
	default:
		// không thì sẽ là
}
```

Ở đây, nếu `a` là `2` hoặc `10`, nó sẽ sẽ thực thi biểu thức "làm gì đó".

Một dạng khác của điều kiện trong JavaScript là "điều hành điều kiện", thường được gọi là "toán tử bậc 3". Nó như là một dạng rút gọn của biểu thức `if..else`, ví dụ:

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// tương tự với:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

Nếu biểu thức (`a > 41` ở đây) thỏa `true`, kết qủa là mệnh đề đầu tiên(`"hello"`), ngược lại là kết quả (`"world"`), và cho dù kết quả là gì thì đều gán vào `b`.

Điều hành điều kiện không nhất thiết phải gán như vậy, nhưng tất nhiên đây là cách được dùng nhiều nhất.

**Chi ghú:** Để biết thêm kiểm tra điều kiện và mẫu khác của `switch` và `? :`, xem phần *Kiểu & Ngữ pháp*.

## Chế độ "nghiêm ngặt" (strict)

ES5 bổ sung "strict mode" cho ngôn ngữ, giúp các hành vi nhất định có nguyên tắc chặt chẽ hơn. Tổng quan thì sự strict được xem như là giữ cho code an toàn và phù hợp hơn. Đồng thời, tôn trong chế độ strict giúp cho bạn được tối ưu hóa hơn bởi cơ chế. Chế độ strict là bàn thắng lớn của code, và bạn nên sử dụng nó cho toàn bộ chương trình của mình.

Bạn có thể tham gia chế độ strict cho một hàm riêng biệt hay toàn bộ file, tùy thuộc bạn đặt chế độ strict đó ở đâu:

```js
function foo() {
	"use strict";

	// Đoạn code này theo chế độ strict

	function bar() {
		// Đoạn code này theo chế độ strict
	}
}

// Đoạn code này không có chế độ strict
```

So sánh với:

```js
"use strict";

function foo() {
	// Đoạn code này theo chế độ strict

	function bar() {
		// Đoạn code này theo chế độ strict
	}
}

// Đoạn code này theo chế độ strict
```

Điểm khác biệt mấu chốt (cải tiến!) ở chế độ strict là không cho phép tự động tiềm ẩn khai báo biến toàn cục khi bỏ qua `var`:

```js
function foo() {
	"use strict";	// đặt chế độ strict
	a = 1;			// `var` thiếu, ReferenceError
}

foo();
```

Bạn sẽ gặp lỗi nếu bạn chuyển sang chế độ strict, hoặc code sẽ dính bug, bạn có thể bị cám dỗ việc né tránh strict. Nhưng bản năng đó là một ý tưởng tồi để lạm dụng. Nếu chế độ strict gây ra các vấn đề trong chương trình, gần như chắc chắn nó là dấu hiệu rằng chương trình của bạn cần khắc phục.

Chế độ strict không chỉ giúp cho code của bạn theo một lối an toàn hơn, tối ưu hóa hơn, mà nó cũng là đại diện cho tương lai của ngôn ngữ. Bắt đầu với chế độ này sẽ dễ dàng hơn là sau mới chuyển qua.

**Ghi chú:** Xem thêm Chương 5 của phần *Kiểu & Ngữ pháp*.

## Hàm là giá trị

Tới giờ, chúng ta đã đề cập hàm như là một cơ chế đầu tiên của *scope* trong JavaScript. Cú pháp khai báo hàm thông thường như sau:

```js
function foo() {
	// ..
}
```

Cơ bản `foo` là một biến trong phạm vi bao quanh bên ngoài được tham chiếu với `function` khai báo, mặc dù dựa trên cú pháp điều này không rõ ràng. Vậy nên, `function` bản thân nó là một giá trị, như là `42` hay `[1,2,3]`.

Mới nghe có vẻ lạ, nên có thể bạn cần một phút suy ngẫm chuyện này.

This may sound like a strange concept at first, so take a moment to ponder it. Not only can you pass a value (argument) *to* a function, but *a function itself can be a value* that's assigned to variables, or passed to or returned from other functions.

As such, a function value should be thought of as an expression, much like any other value or expression.

Ví dụ:

```js
var foo = function() {
	// ..
};

var x = function bar(){
	// ..
};
```

The first function expression assigned to the `foo` variable is called *anonymous* because it has no `name`.

The second function expression is *named* (`bar`), even as a reference to it is also assigned to the `x` variable. *Named function expressions* are generally more preferable, though *anonymous function expressions* are still extremely common.

For more information, see the *Scope & Closures* title of this series.

### Immediately Invoked Function Expressions (IIFEs)

Trong đoạn code trên, muốn biểu thức function được thực thi - chúng ta phải có thêm `foo()` hoặc `x()`.

Có một cách khác để thực hi một biểu thức function, nó thường được gọi là *immediately invoked function expression - (tạm dịch) Biểu thức function thực hiện ngay lập tức* (IIFE):

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

`( .. )` bao ngoài `(function IIFE(){ .. })` function là một sắc thái cần thiết trong ngữ pháp JS để bảo vệ nó khỏi bị hành xử giống như function thông thường.

Dấu `()` cuối cùng của biểu thức `})();` là chính xác cái gì sẽ thực thi tức thì trước nó.

Cái này có vẻ lạ, nhưng không phải xa lạ với cái nhìn đầu tiên. Xem ví dụ tương tự giữa  `foo` và `IIFE`:

```js
function foo() { .. }

// biểu thức function đại diện `foo`,
// sau đó `()` thực thi nó
foo();

// biểu thức function `IIFE`,
// sau đó `()` thực thi nó
(function IIFE(){ .. })();
```

Như bạn thấy, liệt kê `(function IIFE(){ .. })` trước khi nó thực thi `()` là cần thiết tương tự `foo` trước khi thực thi nó bằng `()`; trong cả hai trường hợp, function đại diện thực thi ngay tức thì sau dấu `()`.

Bỏi vì IIFE chỉ là một function và function thì tạo *phạm vi* biến, sử dụng IIFE theo cách này thường là để khai báo biến không ảnh hưởng đến code bên ngoài IIFE:

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42
```

IIFEs có thể trả kết quả:

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

Giá trị `42` được `return` từ `IIFE`- thực thi function được đặt tên theo `x`.

### Closure (đóng kín)

*Closure* là một trong những khái niệm JS quan trọng nhất, và ít được hiểu nhất. Tôi sẽ không đi sâu ở đây, và sẽ phân tích ở *Scope & Closures*. Nhưng tôi sẽ nêu một vài vấn đề để bạn có cái nhìn tổng quan về khái niệm của nó. Nó sẽ là một trong nhưng kỹ thuật quan trọng nhất của bạn.

Bạn có thể nghĩ closure là một cách để "nhớ" và tiếp tục tiếp cận scope của function (biến của nó) kể cả khi function đã hoàn tất.

Xem:

```js
function makeAdder(x) {
	// tham số `x` là một biến bên trong function `add()`, vì vậy nó là một "closure" thông qua nó.
	function add(y) {
		return y + x;
	};

	return add;
}
```

Mối tương quan giữa function `add(...)` được trả với mỗi lần gọi hàm `makeAdder(..)` trên nó là ghi nhớ giá trị `x` được truyền vào `makeAdder(..)`. Giờ hãy sử dụng `makeAdder(..)`:

```js
// `plusOne` có một mối quan hệ khép kín với hàm `add(..)`
// thông qua tham số `x`
// của hàm `makeAdder(..)` trên nó
var plusOne = makeAdder( 1 );

// `plusTen` tương tự
var plusTen = makeAdder( 10 );

plusOne( 3 );		// 4  <-- 1 + 3
plusOne( 41 );		// 42 <-- 1 + 41

plusTen( 13 );		// 23 <-- 10 + 13
```

Code hoạt động như sau:

1. Khi gọi `makeAdder(1)`, chúng ta có được quy chiếu với `add(..)` bên trong nó là `x` bằng `1`. Chúng ta gọi là hàm tham chiếu `plusOne(..)`.
2. Tương tự khi ta gọi `makeAdder(10)`, chúng ta lại có một quy chiếu khác đến `add(..)` rằng `x` là `10`. Chúng ta gọi là hàm tham chiếu `plusTen(..)`.
3. Khi chúng ta gọi `plusOne(3)`, nó cộng `3` (`y` bên trong) với `1` (được ghi nhớ bởi `x`), và chúng ta có kết quả là `4`.
4. Khi chúng ta gọi `plusTen(13)`, nó cộng `13` (`y` bên trong) với `10` (được ghi nhớ bởi  `x`), và chúng ta có kết quả là `23`.

Đừng lo lắng nếu nó có thể xa lạ và hơi bối rối lúc ban đầu -- có thể là vậy! Ta sẽ có nhiều bài tập để hiểu nó đầy đủ hơn. Hãy tin tôi, khi bạn đã hiểu, nó là một trong những những kỹ thuật bá đạo và hữu dụng nhất trong tất cả chương trình. Nó tất nhiên là đáng để cho não bạn căng lên chút. Trong phần tiếp theo, tôi sẽ có một ít bài tập với closure.

#### Modules

Cách sử dụng closure trong JS nhiều nhất là module pattern (mẫu mô-đun). Module cho phép bạn xác định các chi tiết thực hiện khép kín (biến, hàm) cá thể, ẩn khỏi các phần bên ngoài, không giống như API công khai có thể tiếp cận từ phía ngoài.

Xem:

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// bên trong là các phương thức login
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// Tạo ngay một module `User`
var fred = User();

fred.login( "fred", "12Battery34!" );
```

Hàm `User()` thực hiện với vai trò là scope ngoài cùng chứa biến `username` và `password`, trong khi hàm `doLogin()` bên trong với các nội dung của module `User` đều là cục bộ và không thể tiếp cận từ bên ngoài.

**Chú ý:** Chúng ta không gọi `new User()` mặc dù nó có vẻ thông thường với mọi bạn đọc.`User()` chỉ là một hàm, không phải một class để khởi tạo, nên chỉ gọi nó bình thường. Sử dụng `new` là không thích hợp và đương nhiên là lãng phí tài nguyên.

Thực thi `User()` tạo ra một module `User` tức thì -- toàn bộ scope mới được tạo ra, và do đó một bản sao hoàn toàn mới cũa mỗi biến/hàm bên trong. Chúng ta gán nó với `fred`. Nếu chúng ta chạy tiếp `User()`, chúng ta sẽ có một trường hợp mới tách biệt với `fred`.

Hàm `doLogin()` bên trong có một closure đối với `username` và `password`, nghĩa là nó vẫn giữ nguyên khả năng truy cập đến chúng sau khi hàm `User()` đã chạy xong.

`publicAPI` là một object với một thuộc tính/phương thức `login` trong nó. Khi chúng ta trả `publicAPI` từ `User()`, nó trở nên tức thì mà ta gọi là `fred`.

Lúc này, hàm `User()` đã hoàn tất thực thi. Thông thường, chúng ta nghĩ biến bên trong như là `username` và `password` đã biến mất. Nhưng không, bởi có một closure bên trong hàm `login()` giữ nó tồn tại.

Đó là lý do tại sao chúng ta gọi `fred.login(..)` cũng như khi gọi hàm `doLogin(..)` và nó vẫn truy cập biến `username` và `password` bên trong.

Có cái nhìn thoáng qua về closure và module pattern thật là một cơ hội tốt, một vài vấn đề trong nó vẫn còn bối rối. Vậy cũng ổn! Nó cũng cần vài thứ để nạp vào đầu bạn.

Từ chỗ này, xem phần *Scope & Closures* sẽ có sự khám phá sâu hơn.

## Xác định `this`

Một concept khác của Javascript hay bị hiểu sai là xác định `this`. Chúng ta cũng có hẳn một phần nói về nó *this & Object Prototypes*, ở đây ta chỉ giới thiệu sơ qua thôi.

Trong mọi người thường nghĩ `this` liên quan đến "mẫu hướng đối tượng", trong JS `this` lại là cơ chế khác.

Nếu một hàm có `this` bên trong nó, sự quy chiếu của `this` này thường chỉ đến một `object`. Nhưng `object` nào thì tùy vào hàm nó gọi cái gì.

Điều quan trọng là `this` đó *không* tham chiếu đến bản thân hàm của nó, và đây là đây chính là điều hay bị hiểu lầm.

Đây là một minh họa nhanh:

```js
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );	// "obj2"
new foo();			// undefined
```

Có 4 nguyên tắc cho việc lập `this` ra sao, và nó được thể hiện bằng bốn dòng cuối của đoạn code.

1. `foo()` kết thúc việc thiết lập `this` bằng một object toàn cục ở chế độ không nghiêm ngặt (strict mode) -- với chế độ nghiêm ngặt thì `this`sẽ là `undefined` và bạn sẽ gặp lỗi khi truy vấn thuộc tính `bar` -- vì vậy `"global"` là giá trị tìm thấy cho `this.bar`.
2. `obj1.foo()` đặt `this` cho object `obj1`.
3. `foo.call(obj2)` đặt `this` cho object `obj2`.
4. `new foo()` đặt `this` cho một object rỗng mới.

Dòng cuối: để hiểu `this` trỏ đi đâu, bạn phải xem xét hàm được gọi như thế nào. Nó sẽ là một trong 4 cách vừa được thể hiện, và nó sẽ trả lời `this` là gì.

**Ghi chú:** Để biết thêm về `this`, xem Chương 1 và 2 của phần *this & Object Prototypes*.

## Prototypes (nguyên mẫu)

Cơ chế nguyên mẫu trong JavaScript khá là phức tạp. Chúng ta chỉ xem qua nó ở đây. Bạn sẽ dành nhiều thời gian với nó hơn trong Chương 4-6 của phần *this & Object Prototypes*.

Khi bạn tham chiếu một thuộc tính bên trong object, nếu thuộc tính đó không tồn tại, JS sẽ tự động sử dụng tham chiếu nguyên mẫu bên trong để tìm object khác để tìm thuộc tính. Bạn có thể cho rằng đây gần như là dự phòng nếu thuộc tính bị thiếu.

Các mối liên kết tham chiếu nội bộ nguyên mẫu từ một object đến dự phòng của nó xảy ra tại thời điểm object được tạo. Cách đơn giản nhất để minh họa nó là hàm có sẵn `Object.create(..)`.


Ví dụ:

```js
var foo = {
	a: 42
};

// tạo `bar` và liên kết với `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- ủy thác đến `foo`
```

Đây là sơ đồ mối quan hệ giữa `foo` và `bar`:

<img src="fig6.png">

Thuộc tính `a` không thực sự tồn tại trong object `bar`, nhưng vì liên kết nguyên mẫu `bar` với `foo`, JavaScript tự động tìm `a` trong `foo` object.

Sự liên kết có lẽ là một đặc trưng đặc biệt khác lạ của ngôn ngữ. Cách thông dụng nhất của tính năng này được sử dụng nhiều nhất là cố giả lập/giả tạo một cơ chế "class" với "sự thừa kế".

Nhưng cách tự nhiên hơn để áp dụng nguyên mẫu là một pattern gọi là "hành vi ủy quyền (delegation)", nơi bạn cố ý thiết kế các đối tương liên kết có thể *ủy thác* từ một đến nhiều các phần cần hành vi đó.

**Ghi chú:** Để biết thêm về nguyên mẫu và hành vi ủy quyền, xem Chương 4-6 của phần *this & Object Prototypes*.

## Cũ & Mới

Một số tính năng của JS chúng ta cũng đã biết, nhiều tính năng còn lại chúng ta sẽ tìm hiểu trong các phần còn lại, có những tính năng không phù hợp với trình duyệt cũ. Trên thực tế, có cả một số tính năng đặc trưng còn chưa được triển khai trên các trình duyệt ổn định.

Vì vậy, bạn phải làm gì với hàng mới? Bạn có phải chờ hàng năm hay cả thập kỷ để để cho các trình duyệt cũ chìm vào trong bóng tối?

Đó là cách nhiều người nghĩ vậy, nhưng đó là ý nghĩ không "lành mạnh" đối với việc tiếp cận JS.

Có hai kỹ thuật chính để đem đồ chơi mới của JavaScript chiến trên trình duyệt cũ: polyfilling và transpiling.

### Polyfilling

Từ "polyfill" là một thuật ngữ được phát kiến bởi (Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) được dùng để mô tả việc xác định một tính năng mới và tái tạo lại đoạn code có thể chạy trên môi trường JS cũ với những hành vi tương tự.

Ví dụ, ES6 định nghĩa một tiện ích gọi là `Number.isNaN(..)` để cung cấp một phương thức kiểm tra chính xác không lỗi cho giá trị `NaN` thay cho tiện ích gốc `isNaN(..)`. Nhưng cũng dễ dàng để polyfill tiện ích đó nên bạn có thể bắt đầu sử dụng nó bất kể người dùng cuối có dùng trình duyệt ES6 hay không:

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

Câu lệnh `if` bảo vệ việc áp dụng polyfill trong trình duyệt ES6. Nếu nó chưa có, chúng ta định nghĩa `Number.isNaN(..)`.

**Ghi chú:** Việc kiểm tra mà chúng ta thực hiện tận dụng lợi thế của giá trị quái đản `NaN`, nó là giá trị duy nhất trong toàn bộ ngôn ngữ không bằng với chính nó. Ví vậy giá trị `NaN` là loại duy nhất có thể làm cho `x !== x` trở thành `true`.

Không phải tất cả các tính năng mới để có thể polyfill được hoàn toàn. Đôi khi hầu hết các hành vi có thể được polyfill, nhưng cũng có những sai lệch nhỏ. Bạn phải rất rất cẩn thận khi tự thực hiện polyfill, để đảm bảo rằng bạn đang tuân thủ các đặc điểm kỹ thuật càng chặt chẽ càng tốt.

Hoặc tốt hơn, sử dụng một bộ polyfill tin cậy đã được cung cấp bởi ES5-Shim (https://github.com/es-shims/es5-shim) và ES6-Shim (https://github.com/es-shims/es6-shim).

### Transpiling

Không có cách nào để polyfill cú pháp mới khi đã được thêm vào ngôn ngữ. Cú pháp mới sẽ báo lỗi không nhận/không hợp pháp trong JS engine cũ.

Vì vậy phương án tiếp tốt hơn là sử dụng công cụ để chuyển đoạn code của bạn sang đoạn code cũ tương ứng. Các này thường được gọi là "transpiling", cụm từ của transforming + compiling.

Về bản chất, mã nguồn của bạn được tạo theo dạng cú pháp mới, nhưng những gì bạn triển khai trên trình duyệt là mã nguồn cũ. Bạn thường cho transpiler trong quá trình build, tương tự như linter (kiểm lỗi cú pháp) & minifier (nén code).

Bạn có thể thắc mắc vì sao bạn phải cú pháp mới chỉ để chuyển nó theo cách cũ -- tại sao không viết luôn code kiểu cũ cho rồi?

Có vài lý do quan trọng để bạn quan tâm đến transpiling:

* Cú pháp mới được thiết kế để đưa vào ngôn ngữ làm cho code của bạn dễ đọc dễ bảo trì hơn. Code cũ tương ứng thường phức tạp hơn rất nhiều. Bạn sẽ muốn viết theo cách mới với cú pháp rõ ràng hơn, không chỉ cho riêng bạn mà còn cho thành viên khác trong nhóm của bạn.
* Nếu bạn chỉ transpile cho trình duyệt cũ, sử dụng cú pháp mới cho trình duyệt mới, bạn sẽ tận dụng được hiệu suất của cú pháp mới. Nó đồng thời giúp người tạo ra trình duyệt có nhiều thực tế để kiểm tra và tối ưu trình duyệt của họ.
* Sử dụng cú pháp mới sớm hơn cho phép thử nghiệm mạnh mẽ hơn trong thực tế, đưa ra phản hồi cho ủy ban JavaScript (TC39). Nếu phát hiện sớm các vấn đề, họ có thể thay đổi/sửa chữa trước khi lỗi thiết kế ngôn ngữ trở nên vĩnh viễn.

Đây là một ví dụ nhanh cho transpiling. ES6 thêm một tính năng gọi là "tham chiếu mặc định" như thế này:

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

Đơn giản phải không? Cũng rất hữu dụng nữa! Nhưng mà cú pháp mới này không tương thích với ES engine cũ. Vì vậy nó sẽ transpiler sang code cũ để phục vụ môi trường cũ.

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

Như bạn thấy, nó kiểm tra nếu giá trị `arguments[0]` là `void 0` (tức `undefined`), thì sẽ là giá trị mặc định `2`; ngược lại, nó sẽ chỉ định vào bất kỳ gì truyền vào.

Bây giờ ngoài việc có thể sử dụng cú pháp đẹp hơn cho trình duyệt cũ, nhìn vào mã transpile có thể giải thích hành vi rõ ràng hơn.

Bạn có thể không nhận ra rằng `undefined` là giá trị duy nhất không thể truyền vào thông qua việc cho một tham số mặc định khi nhìn vào bản ES6, nhưng ở bản transpile thì nó rõ ràng hơn.

Chi tiết quan trọng cuối cùng nhấn mạnh vào transpiler là không nên cho nó là một phần tiêu chuẩn của cộng đồng phát triển JS. JS sẽ tiếp tục phát triển nhanh chóng hơn bao giờ hết, nên chỉ vài tháng là có vài tính năng mới được bổ sung.

Nếu bạn mặc định sử dụng transpiler, bạn phải luôn cho phép nó chuyển qua cú pháp mới khi bạn thấy nó hữu dụng thay vì phải chờ đợi nhiều năm cho trình duyệt hiện tại lu mờ.

Có vài transpiler để bạn chọn, dưới đây là 2 cách để bạn xem xét:

* Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
* Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5

## Non-JavaScript

Tới giờ, điều duy nhất chúng ta quan tâm là bản thân JS. Thực tế thì hầu hết JS được viết để chạy và tương tác với môi trường như trình duyệt. Nhưng nói nghe hơi lạ là những gì bạn viết, thẳng ra là vậy, không được điều khiển bởi JavaScript.

Ví dụ đoạn code mà bạn hay dùng nhất trong DOM API không phải là JavaScript

```js
var el = document.getElementById( "foo" );
```
Biến `document` tồn tại như một biến toàn cục khi code của bạn chạy trên trình duyệt. Nó không được cung cấp bởi JS engine hay được đặc biệt điều khiển bởi đặc tính JavaScript. Nó lấy dạng của gì đó trông kinh hơn JS `object` thông thường, mà cũng không chính xác là nó. Nó là một `object` đặc biệt, thường được gọi là "host object".

Hơn nữa, phương thức `getElementById(..)` ở `document` trông như hàm JS thông thường, nhưng nó chỉ là một giao diện mỏng được lộ ra với phương thức tích hợp bởi DOM từ trình duyệt. Trong một vài trình duyệt mới, lớp này có thể ở trong JS, nhưng bản chất thì DOM và hành vi của nó được thực hiện bởi gì đó như là C/C++.

Một ví dụ khác là với input/output (I/O).

Mọi người ưa thích pop up `alert(..)` trên cửa sổ trình duyệt. `alert(..)` được cung cấp cho JS của bạn thông qua trình duyệt, không phải JS engine. Khi gọi hàm, nó gửi nội dung đến bên trong trình duyệt và nó sẽ vẽ và hiển thị khung nội dung đó.

Tương tự với `console.log(..)`; trình duyệt của bạn cung cấp cơ chế và nối chúng với công cụ phát triển (developer tools).

Cuốn sách này tập trung vào ngôn ngữ JavaScript. Vì vậy bạn sẽ không thấy để cập non-JavaScript nhiều. Tuy nhiên bạn cần phải có nhận thức về chúng, nó ngay trong mọi chương trình JS bạn viết.

## Ôn lại

Bước đầu tiên để học JavaScript là hiểu cơ bản về cơ chế cốt lõi của nó như (không dịch) value, type, closure, `this`, và prototypes.

Đương nhiên, mỗi đề mục được đáng được cụ thể hơn bạn thấy ở đây, đó là l1y do tại sao nó được đề cập suốt trong các phần bộ sách. Sau khi bạn cảm thấy thoải mái với các khái niệm và mẫu code trong chương này, phần còn lại của bộ sách chờ bạn đào sâu vào ngôn ngữ hơn.

Chương cuối của cuốn sách sẽ tóm tắt từng mỗi tiêu đề và các khái niệm khác bên cạnh những gì chúng ta khám phá.
