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

### Function Scopes (Phạm vi chức năng)

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

**Chú ý:** Đây không thường là một ý hay khi dựa vào biến *hoisting* để sử một biến trước đó trong scope của nó hơn là `var` được khai báo bởi chính nó, điều này có thể gây bối rối. Cách thông thường và được chấp nhận để sử dụng *hoisted* function là gọi trước khi nó được khai báo như chúng ta làm với `foo()`.

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

In addition to the `if` statement we introduced briefly in Chapter 1, JavaScript provides a few other conditionals mechanisms that we should take a look at.

Sometimes you may find yourself writing a series of `if..else..if` statements like this:

```js
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```

This structure works, but it's a little verbose because you need to specify the `a` test for each case. Here's another option, the `switch` statement:

```js
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

The `break` is important if you want only the statement(s) in one `case` to run. If you omit `break` from a `case`, and that `case` matches or runs, execution will continue with the next `case`'s statements regardless of that `case` matching. This so called "fall through" is sometimes useful/desired:

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```

Here, if `a` is either `2` or `10`, it will execute the "some cool stuff" code statements.

Another form of conditional in JavaScript is the "conditional operator," often called the "ternary operator." It's like a more concise form of a single `if..else` statement, such as:

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

If the test expression (`a > 41` here) evaluates as `true`, the first clause (`"hello"`) results, otherwise the second clause (`"world"`) results, and whatever the result is then gets assigned to `b`.

The conditional operator doesn't have to be used in an assignment, but that's definitely the most common usage.

**Note:** For more information about testing conditions and other patterns for `switch` and `? :`, see the *Types & Grammar* title of this series.

## Chế độ nghiêm ngặt

ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. Also, adhering to strict mode makes your code generally more optimizable by the engine. Strict mode is a big win for code, and you should use it for all your programs.

You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma:

```js
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

Compare that to:

```js
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```

One key difference (improvement!) with strict mode is disallowing the implicit auto-global variable declaration from omitting the `var`:

```js
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

If you turn on strict mode in your code, and you get errors, or code starts behaving buggy, your temptation might be to avoid strict mode. But that instinct would be a bad idea to indulge. If strict mode causes issues in your program, almost certainly it's a sign that you have things in your program you should fix.

Not only will strict mode keep your code to a safer path, and not only will it make your code more optimizable, but it also represents the future direction of the language. It'd be easier on you to get used to strict mode now than to keep putting it off -- it'll only get harder to convert later!

**Note:** For more information about strict mode, see the Chapter 5 of the *Types & Grammar* title of this series.

## Functions As Values

So far, we've discussed functions as the primary mechanism of *scope* in JavaScript. You recall typical `function` declaration syntax as follows:

```js
function foo() {
	// ..
}
```

Though it may not seem obvious from that syntax, `foo` is basically just a variable in the outer enclosing scope that's given a reference to the `function` being declared. That is, the `function` itself is a value, just like `42` or `[1,2,3]` would be.

This may sound like a strange concept at first, so take a moment to ponder it. Not only can you pass a value (argument) *to* a function, but *a function itself can be a value* that's assigned to variables, or passed to or returned from other functions.

As such, a function value should be thought of as an expression, much like any other value or expression.

Consider:

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

The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that *is* accessible from the outside.

Consider:

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

The `User()` function serves as an outer scope that holds the variables `username` and `password`, as well as the inner `doLogin()` function; these are all private inner details of this `User` module that cannot be accessed from the outside world.

**Warning:** We are not calling `new User()` here, on purpose, despite the fact that probably seems more common to most readers. `User()` is just a function, not a class to be instantiated, so it's just called normally. Using `new` would be inappropriate and actually waste resources.

Executing `User()` creates an *instance* of the `User` module -- a whole new scope is created, and thus a whole new copy of each of these inner variables/functions. We assign this instance to `fred`. If we run `User()` again, we'd get a new instance entirely separate from `fred`.

The inner `doLogin()` function has a closure over `username` and `password`, meaning it will retain its access to them even after the `User()` function finishes running.

`publicAPI` is an object with one property/method on it, `login`, which is a reference to the inner `doLogin()` function. When we return `publicAPI` from `User()`, it becomes the instance we call `fred`.

At this point, the outer `User()` function has finished executing. Normally, you'd think the inner variables like `username` and `password` have gone away. But here they have not, because there's a closure in the `login()` function keeping them alive.

That's why we can call `fred.login(..)` -- the same as calling the inner `doLogin(..)` -- and it can still access `username` and `password` inner variables.

There's a good chance that with just this brief glimpse at closure and the module pattern, some of it is still a bit confusing. That's OK! It takes some work to wrap your brain around it.

From here, go read the *Scope & Closures* title of this series for a much more in-depth exploration.

## `this` Identifier

Another very commonly misunderstood concept in JavaScript is the `this` identifier. Again, there's a couple of chapters on it in the *this & Object Prototypes* title of this series, so here we'll just briefly introduce the concept.

While it may often seem that `this` is related to "object-oriented patterns," in JS `this` is a different mechanism.

If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

It's important to realize that `this` *does not* refer to the function itself, as is the most common misconception.

Here's a quick illustration:

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

There are four rules for how `this` gets set, and they're shown in those last four lines of that snippet.

1. `foo()` ends up setting `this` to the global object in non-strict mode -- in strict mode, `this` would be `undefined` and you'd get an error in accessing the `bar` property -- so `"global"` is the value found for `this.bar`.
2. `obj1.foo()` sets `this` to the `obj1` object.
3. `foo.call(obj2)` sets `this` to the `obj2` object.
4. `new foo()` sets `this` to a brand new empty object.

Bottom line: to understand what `this` points to, you have to examine how the function in question was called. It will be one of those four ways just shown, and that will then answer what `this` is.

**Note:** For more information about `this`, see Chapters 1 and 2 of the *this & Object Prototypes* title of this series.

## Prototypes

The prototype mechanism in JavaScript is quite complicated. We will only glance at it here. You will want to spend plenty of time reviewing Chapters 4-6 of the *this & Object Prototypes* title of this series for all the details.

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called `Object.create(..)`.

Consider:

```js
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

It may help to visualize the `foo` and `bar` objects and their relationship:

<img src="fig6.png">

The `a` property doesn't actually exist on the `bar` object, but because `bar` is prototype-linked to `foo`, JavaScript automatically falls back to looking for `a` on the `foo` object, where it's found.

This linkage may seem like a strange feature of the language. The most common way this feature is used -- and I would argue, abused -- is to try to emulate/fake a "class" mechanism with "inheritance."

But a more natural way of applying prototypes is a pattern called "behavior delegation," where you intentionally design your linked objects to be able to *delegate* from one to the other for parts of the needed behavior.

**Note:** For more information about prototypes and behavior delegation, see Chapters 4-6 of the *this & Object Prototypes* title of this series.

## Old & New

Some of the JS features we've already covered, and certainly many of the features covered in the rest of this series, are newer additions and will not necessarily be available in older browsers. In fact, some of the newest features in the specification aren't even implemented in any stable browsers yet.

So, what do you do with the new stuff? Do you just have to wait around for years or decades for all the old browsers to fade into obscurity?

That's how many people think about the situation, but it's really not a healthy approach to JS.

There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

### Polyfilling

The word "polyfill" is an invented term (by Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

For example, ES6 defines a utility called `Number.isNaN(..)` to provide an accurate non-buggy check for `NaN` values, deprecating the original `isNaN(..)` utility. But it's easy to polyfill that utility so that you can start using it in your code regardless of whether the end user is in an ES6 browser or not.

Consider:

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

The `if` statement guards against applying the polyfill definition in ES6 browsers where it will already exist. If it's not already present, we define `Number.isNaN(..)`.

**Note:** The check we do here takes advantage of a quirk with `NaN` values, which is that they're the only value in the whole language that is not equal to itself. So the `NaN` value is the only one that would make `x !== x` be `true`.

Not all new features are fully polyfillable. Sometimes most of the behavior can be polyfilled, but there are still small deviations. You should be really, really careful in implementing a polyfill yourself, to make sure you are adhering to the specification as strictly as possible.

Or better yet, use an already vetted set of polyfills that you can trust, such as those provided by ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim).

### Transpiling

There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.

So the better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "transpiling," a term for transforming + compiling.

Essentially, your source code is authored in the new syntax form, but what you deploy to the browser is the transpiled code in old syntax form. You typically insert the transpiler into your build process, similar to your code linter or your minifier.

You might wonder why you'd go to the trouble to write new syntax only to have it transpiled away to older code -- why not just write the older code directly?

There are several important reasons you should care about transpiling:

* The new syntax added to the language is designed to make your code more readable and maintainable. The older equivalents are often much more convoluted. You should prefer writing newer and cleaner syntax, not only for yourself but for all other members of the development team.
* If you transpile only for older browsers, but serve the new syntax to the newest browsers, you get to take advantage of browser performance optimizations with the new syntax. This also lets browser makers have more real-world code to test their implementations and optimizations on.
* Using the new syntax earlier allows it to be tested more robustly in the real world, which provides earlier feedback to the JavaScript committee (TC39). If issues are found early enough, they can be changed/fixed before those language design mistakes become permanent.

Here's a quick example of transpiling. ES6 adds a feature called "default parameter values." It looks like this:

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

Simple, right? Helpful, too! But it's new syntax that's invalid in pre-ES6 engines. So what will a transpiler do with that code to make it run in older environments?

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

As you can see, it checks to see if the `arguments[0]` value is `void 0` (aka `undefined`), and if so provides the `2` default value; otherwise, it assigns whatever was passed.

In addition to being able to now use the nicer syntax even in older browsers, looking at the transpiled code actually explains the intended behavior more clearly.

You may not have realized just from looking at the ES6 version that `undefined` is the only value that can't get explicitly passed in for a default-value parameter, but the transpiled code makes that much more clear.

The last important detail to emphasize about transpilers is that they should now be thought of as a standard part of the JS development ecosystem and process. JS is going to continue to evolve, much more quickly than before, so every few months new syntax and new features will be added.

If you use a transpiler by default, you'll always be able to make that switch to newer syntax whenever you find it useful, rather than always waiting for years for today's browsers to phase out.

There are quite a few great transpilers for you to choose from. Here are some good options at the time of this writing:

* Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
* Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5

## Non-JavaScript

So far, the only things we've covered are in the JS language itself. The reality is that most JS is written to run in and interact with environments like browsers. A good chunk of the stuff that you write in your code is, strictly speaking, not directly controlled by JavaScript. That probably sounds a little strange.

The most common non-JavaScript JavaScript you'll encounter is the DOM API. For example:

```js
var el = document.getElementById( "foo" );
```

The `document` variable exists as a global variable when your code is running in a browser. It's not provided by the JS engine, nor is it particularly controlled by the JavaScript specification. It takes the form of something that looks an awful lot like a normal JS `object`, but it's not really exactly that. It's a special `object,` often called a "host object."

Moreover, the `getElementById(..)` method on `document` looks like a normal JS function, but it's just a thinly exposed interface to a built-in method provided by the DOM from your browser. In some (newer-generation) browsers, this layer may also be in JS, but traditionally the DOM and its behavior is implemented in something more like C/C++.

Another example is with input/output (I/O).

Everyone's favorite `alert(..)` pops up a message box in the user's browser window. `alert(..)` is provided to your JS program by the browser, not by the JS engine itself. The call you make sends the message to the browser internals and it handles drawing and displaying the message box.

The same goes with `console.log(..)`; your browser provides such mechanisms and hooks them up to the developer tools.

This book, and this whole series, focuses on JavaScript the language. That's why you don't see any substantial coverage of these non-JavaScript JavaScript mechanisms. Nevertheless, you need to be aware of them, as they'll be in every JS program you write!

## Review

The first step to learning JavaScript's flavor of programming is to get a basic understanding of its core mechanisms like values, types, function closures, `this`, and prototypes.

Of course, each of these topics deserves much greater coverage than you've seen here, but that's why they have chapters and books dedicated to them throughout the rest of this series. After you feel pretty comfortable with the concepts and code samples in this chapter, the rest of the series awaits you to really dig in and get to know the language deeply.

The final chapter of this book will briefly summarize each of the other titles in the series and the other concepts they cover besides what we've already explored.
