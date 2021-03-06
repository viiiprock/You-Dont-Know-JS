# You Don't Know JS: Khởi đầu
# Chương 1: Hiểu về lập trình

Chào mừng bạn đến với seri You Don't Know JS (YDKJS).

Khởi động là lời giới thiệu một số  khái niệm cơ bản của lập trình (dĩ nhiên là chúng ta đặc biệt hướng về phía JavaScript --viết tắt là JS--) và cách để tiếp và hiểu các nội dung trong seri này. Đặc biệt là nếu chúng ta tìm hiểu sâu lập trình và JavaScript, cuốn sách này sẽ khám phá tóm lược những gì bạn cần để khởi đầu và phát triển.

Cuốn sách này bắt đầu ở mức bớt giải thích những nguyên tắc cơ bản của lập trình ở mức độ rất cao. Nó hầu như dự định dành cho người đọc với kiến thức, kinh nghiệm lập trình ít, đang tìm kiếm sách để học và tìm con đường để hiểu lập trình thông qua lăng kính JavaScript.

Chương 1 được tiếp cần như một cái nhìn tổng quát vào những gì bạn sẽ học và thực hành để hiểu sâu hơn về lập trình. Ngoài ra còn có nhiều nguồn tài nguyên giới thiệu lập trình tuyệt vời khác có thể giúp bạn thâm nhập vào các chủ đề này hơn nữa, và tôi khuyến khích bạn để học hỏi từ họ bên cạnh chương này.

Một khi bạn cảm thấy thoải mái với những vấn đề lập trình cơ bản, Chương 2 sẽ hướng dẫn bạn làm quen với hương vị lập trình JavaScript. Chương 2 giới thiệu JavaScript là gì, nhưng một lần nữa, nó cũng không phải là hướng dẫn toàn diện -- phần còn lại của bộ sách sẽ làm chuyện đó.

Nếu bạn đã khá thoải mái với JavaScript rồi thì hãy bắt đầu từ việc nhìn thoáng qua Chương 3 và nhảy tiếp!

## Code

Hãy bắt đầu từ đầu.

Một chương trình, thường được gọi là *source code* hoặc *code*, là một tập hợp các hướng dẫn để yêu cầu máy tính cần xử lý nhiệm vụ gì. Thông thường, code được lưu dưới dạng file văn bản, với js bạn có thể gõ code trực tiếp lên console của trình duyệt, chúng tôi sẽ nói chuyện này sau.

Các nguyên tắc của một định dạng hợp lệ cùng sự kết hợp của các hướng dẫn đó được gọi là ngôn ngữ máy tính, đôi khi còn được gọi là cú pháp, tương tự như ngôn ngữ giao tiếp chỉ cho bạn cách đọc từ và các tạo ra câu đúng bằng cách sử dụng từ và dấu câu.

### Các câu lệnh (Statements)

Trong ngôn ngữ máy tính, một nhóm từ, số, cách thức thực thi từng nhiệm vụ cụ thể được gọi là câu lệnh. Trong JS, câu lệnh có thể như sau:

```js
a = b * 2;
```

Ký tự `a` và `b` là những *biến(variable)* (xem "Các biến"), giống như những cái hộp đơn giản, bạn có thể lưu trữ đồ vật trong đó. Trong lập trình, biến giữ giá trị (ví dụ số `42`) được sử dụng bởi chương trình. Hãy nghĩ nó như chỗ giữ biểu tượng cho chính nó.

Về mặt tương phản, số `2` bản thân nó chỉ là một giá trị, được gọi là giá trị ký tự (literal value), vì nó độc lập và không lưu trữ trong một biến nào cả.

Dấu `=` và `*` là *toán tử (operators)* (xem "Toản tử") -- nó thực thi các hành động với giá trị và biến như sự phân công và phép toán nhân.

Hầu hết câu lệnh trong JS kết thúc bằng dấu chấm phẩy (`;`) ở cuối câu

Lệnh `a = b * 2;` báo cho máy tính giá trị được lưu trữ trong biến `b`, nhân giá trị đó với 2, sau đó lưu kết quả lại vào một biến khác gọi là `a`.

Lập trình tương tự như bộ sưu tập của nhiều câu lệnh cùng nhau mô tả tất cả các bước để thực thi mục đích lập trình của bạn.

### Biểu thức (Expressions)

Các câu lệnh được tạo thành từ một hay nhiều *biểu thức*. Một biểu thức là bất kỳ tham chiếu trên một biến hoặc một giá trị, hoặc tập hợp các giá trị và các biến kết hợp thành toán tử.

Ví dụ:

```js
a = b * 2;
```

Câu lệnh này có 4 biểu thức bên trong nó:

* `2` là *giá trị biểu thức trực kiện*
* `b` là *giá trị biểu thức*, có nghĩa là sẽ lấy giá trị hiện tại của nó
* `b * 2` là *biểu thức toán học*, nghĩa là sẽ làm phép nhân
* `a = b * 2` là một *biểu thức gán*, nghĩa là sẽ gán kết quả của biểu thức `b * 2` cho biến `a` (còn tìm hiểu thêm sau)

Một biểu thức chung đứng một mình còn được gọi là *lệnh biểu thức* (expression statement), như ví dụ sau:

```js
b * 2;
```

Kiểu biểu thức này thường không hữu dụng, bởi nó chẳng có tác dụng nào đối với chương trình đang chạy -- nó sẽ lấy giá trị của `b` và nhân nó với `2`, nhưng sau đó không làm gì với kết quả đó.

Một loại câu lệnh biểu thức nữa là *biểu thức lệnh gọi* (call expression) (Xem "Functions"), khi toàn câu lệnh là một hàm tự gọi biểu thức:

```js
alert( a );
```

### Thực thi chương trình

Làm cách nào mà tập hợp các câu lệnh lập trình có thể yêu cầu máy tính phải làm gì? Chương trình cần được thực thi, hay còn được biết đến với tên *chạy chương trình*.

Các lệnh giống như `a = b * 2` rất hữu dụng cho lập trình viên đọc và viết, nhưng nó không hoàn toàn ở dạng để máy tính hiểu trực tiếp. Vì vậy, một trình tiện ích đặc biệt trong máy tính (hoặc là *thông dịch-interpreter* hoặc là *biên dịch-compiler* ) được sử dụng để dịch code bạn viết thành các lệnh mà máy tính có thể hiểu.

Đối với một số ngôn ngữ máy tính, mỗi khi chương trình chạy thì bản dịch của các câu lệnh thường được hoàn thành từ trên xuống dưới, từng dòng một, nó thường được gọi là thông dịch mã.

Một số ngôn ngữ khác, bản dịch được hoàn thiện trước, gọi là biên dịch mã, rồi sau đó  chương trình mới chạy, những gì đang chạy đã được biên dịch xong để cho máy sẵn sàng chạy.

JavaScript thường được khẳng định nó là ngôn ngữ được *thông dịch*, bởi vì mã nguồn JavaScript được xử lý mỗi lần chạy. Nhưng điều đó không hoàn toàn chính xác, thực ra là JavaScript engine  biên dịch chương trình và sau đó chạy ngay mã đã được biên dịch.

**Ghi chú:** thông tin thêm về biên dịch JavaScript, xem 2 chương đầu của Scope & Closures trong seri này.


## Tự bạn thử

Chương này sẽ giới thiệu mỗi khái niệm lập trình với những mẫu code đơn giản, tất cả đều viết bằng JavaScript (đương nhiên!!!)

Khi bạn đi qua chương này thì cũng không phải là đủ -- và bạn cũng có thể bỏ thời gian xem nó vài lần -- bạn cần luyện tập mỗi khái niệm bằng cách tự gõ code. Các dễ nhất là mở developer tool console trên trình duyệt.

**Mẹo:** Thường thì bạn có thể mở developer console với phím tắt hoặc từ menu. Thông tin chi tiết về việc mở và sử dụng console trong trình duyệt ưa thích của bạn, xem ["Mastering The Developer Tools Console"](http://blog.teamtreehouse.com/mastering-developer-tools-console). Để gõ nhiều dòng trên console cùng lúc, sử dụng `<shift> + <enter>` để chuyển sang dòng mới. Khi  bạn nhấn `<enter>`, console sẽ chạy tất cả những gì bạn vừa viết.

Hãy làm quen với việc chạy code trên console. Trước tiên, tôi đề nghị bạn mở 1 tab trống trên trình duyệt. Tôi thích làm cách này bằng việc gõ `about:blank` trên thanh địa chỉ. Sau đó chỉ cần chắc chắn developer console được mở như chúng ta đã đề cập.

Và giờ hãy gõ code này và xem chúng chạy:

```js
a = 21;

b = a * 2;

console.log( b );
```

Gõ những đoạn code trước trên Chrome console có thể tạo ra gì đó như:

<img src="fig1.png" width="500">

Thử đi. Cách học code hay nhất để bắt đầu học lập trình đó! 

### Output

Trong mẫu code ở trên, chúng ta sử dụng `console.log(..)`. Tóm lại, hãy nhìn vào những dòng code và xem nó là gì.

Bạn có thể đoán, nhưng chính xác làm sao chúng ta có thẻ print text (tức *ouput* cho user) trên dev console. Có hai đặc tính mà chúng ta cần giải thích.

Đầu tiên, phần `log( b )` được đại diện như việc gọi funciton (xem "Functions"). Những  gì xảy ra là chúng ta đang giao biến `b` cho function đó, function sẽ yêu cầu lấy giá trị của `b` và print nó lên console.

Thứ hai, phần `console.` làm một object tham chiếu nơi có `log(...)` function. Chúng ta sẽ xem object và các thuộc tính của nó chi tiết hơn ở Chương 2.

Cách khác để tạo ra output để bạn có thể nhìn nó chạy là `alert(...)`. Ví dụ:


```js
alert( b );
```

Nếu bạn  chạy nó, bạn sẽ thấy rằng thay vì print output lên console, nó sẽ hiển thị một popup "OK" với nội dung của biến `b`. Tuy nhiên, sử dụng `console.log(..)` giúp cho việc học của bạn thuận tiện hơn nhiều so với `alert(..)`, bởi vì bạn có thể output nhiều giá trị cùng lúc mà không bị phiền hà bởi trình duyệt.

Với sách này thì chúng ta sẽ dùng `console.log(..)`

### Input

Khi chúng ta thảo luận về output, bạn cũng sẽ thắc mắc về *input* (ví dụ như nhận thông tin từ user)

Cách thông thường nhất là trang HTML hiển thị form cho user có thể gõ vào, sau đó sử dụng js để đọc các giá trị vào các biến của chương trình.

Nhưng có một cách đơn giản để lấy input để học và trình bày như bạn học theo sách này, đó là sử dụng function `prompt(...)`


```js
age = prompt( "Please tell me your age:" );

console.log( age );
```

Như bạn có thể đoán, mẩu tin bạn đưa vào `prompt(..)` -- trong trường hợp này, `"Please tell me your age:"` -- được in lên popup.

Điều này cũng tương tự

<img src="fig2.png" width="500">

Khi bạn nhập input text bằng cách nhấp "OK", bạn sẽ tuân theo giá trị mà bạn đã gõ được lưu trong biến `age`, cái mà chúng ta *output* với `console.log(..)`:

<img src="fig3.png" width="500">

Để giữ cho mọi thứ đơn giản trong quá trình chúng ta học lý thuyết lập trình cơ bản, các ví dụ trong sách này sẽ không cần input. Nhưng giờ bạn đã biết cách sử dụng `prompt(..)`, nếu bạn muốn thử thách bản thân, bạn có thể thử sử dụng input qua khám phá các ví dụ.


## Các biểu thức  

Toán tử là những gì chúng ta thực thi hành động trên các biến và các giá trị. Chúng ta đã thấy hai kiểu toán tử của JavaScript là `=` và `*`

Toán tử `*` để  thực hiện phép toán nhân. Dễ heng.

Dấu `=` được sử  dụng để *gán* -- đầu tiên là ta tính toán giá trị ở *phí bên phải* (giá trị nguồn) dấu `=` và sau nó đặt nó vào biến(variable) mà chúng ta xác định ở *phía bên trái* (variable mục tiêu).


**Chú ý:** Có thể cảm giác hơi bị ngược bằng cách đảo trình tự gán. Thay vì `a = 42`, một số thích lật ngược trình tự giá trị nguồn ở bên trái và biến mục tiêu ở bên phải, kiểu  như `42 -> a` (nó không phải JavaScript!). Không may,`a = 42` là định  dạng được sắp xếp, và tương tự các biến, nó khá phổ  biến trong các ngôn ngữ lập trình hiện đại. Nếu có cảm giác không tự nhiên, hãy  bỏ chút thời gian để làm quen và tâm trí bạn sẽ tự sắp xếp nó.

Hãy xem:

```js
a = 2;
b = a + 1;
```

Tại đây, chúng ta gán giá trị`2` cho biến `a`. Sau đó chúng ta có giá trị của biến `a` (vẫn là  `2`), thêm `1` vào nó sẽ có kết quả giá trị `3`, sau đó lưu giá trị đó vào biến `b`.

Trong khi biểu thức kỹ thuật,  bạn cũng cần từ khoá `var` trong mọi thể loại lập trình, như cách đầu tiên bạn *khai báo* (aka *khởi tạo*) *các biến* (Xem "Variables")

Bạn có  thể luôn khai báo biến bằng tên trước khi sử dụng nó. Nhưng bạn cũng chỉ khai báo một biến một lần trong mỗi *scope* (xem "Scope"): nó có thể được sử dụng nhiều lần khi cần. Ví dụ:

```js
var a = 20;

a = a + 1;
a = a * 2;

console.log( a );	// 42
```

Sau đây là một số biểu thức trong JavaScript:

* Gán: `=` như `a = 2`.
* Toán: `+` (cộng), `-` (trừ), `*` (nhân), và `/` (chia),  `a * 3`.
* Tổ hợp gán: `+=`, `-=`, `*=`, và `/=` là các biểu thức tổ hợp mà nó kết hợp giữa toán với các giá trị gán, ví dụ như `a += 2` (tương tự `a = a + 2`).
* Tăng/Giảm: `++` (tăng), `--` (giảm), như `a++` (tương tự  `a = a + 1`).
* Tiếp cận object: `.` như `console.log()`.
  
  Object là các giá trị chứa các giá trị khác tại các vị trí có tên cụ thể gọi là thuộc tính. `obj.a` nghĩa là một giá trị object gọi `obj` với một đặc tính có tên `a`. Các thuộc tính có thể chuyển đổi cách tiếp cận ví dụ `obj["a"]`. Xem chương 2.

* Tương đương: `==` (bằng tương đối), `===` (bằng tuyệt đối), `!=` (khác tương đối), `!==` (khác tuyệt đối), ví dụ `a == b`.
   See "Values & Types" and Chapter 2.

* So sánh: `<` (nhỏ hơn), `>` (lớn hơn), `<=` (nhỏ hơn hoặc bằng tương đối), `>=` (lớn hơn hoặc bằng tương đối), như `a <= b`.

  Xem "Values & Types" ở  2.
* Tính logic: `&&` (và), `||` (hoặc), ví dụ `a || b` chọn  luôn `a` *hoặc* `b`.

   Các biểu thức này được sử dụng để diễn tả các điều kiện phức tạp (xem "Điều kiện"), ví như cả `a` *hoặc* `b` đều đúng.


**Ghi chú:** Để biết chi tiết nhiều hơn, phạm vi của các biểu thức không đề cập ở đây, bạn có thể xem thêm Mozilla Developer Network (MDN)'s "Expressions and Operators" (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators).

## Giá trị (Values) & loại (types)

Nếu bạn hỏi nhân viên tại một cửa hàng điện thoại giá của một cái điện thoại gì đó, và họ trả lời "chín chín, chín chín"
(ví dụ $99.99), tức họ đã cho bạn một hình dung giá trị tiền mà  bạn cần phải trả (bao gồm thuế) để mua nó. Nếu bạn muốn mua 2 cái điện  thoại, bạn có thể dễ dàng làm phép toán để gấp đôi giá trị thành $199.98 từ giá gốc.

Nếu người nhân viên đó lấy hai cái điện thoại tương tự và nói "miễn phí", tức là họ không cho bạn một con số nào, nhưng nó cũng là một dạng đại diện cho mức giá ($0.00) -- cho từ "miễn phí".

Sau đó bạn lại hỏi điện thoại có đồ sạc hay không, câu trả lời có thể là "có" hoặc "không".

Bằng cách tương tự, khi bạn diễn tả những giá trị trong một chương trình, bạn chon các kiểu đại diện khác nhau cho các giá trị đó dựa trên những gì bạn kế hoạch với chúng.

Những kiểu đại diện khác nhau cho ác giá trị được gọi là *types* trong thuật ngữ lập trình. JavaScript có sẵn các type được gọi là giá trị *nguyên thuỷ* :

* Khi bạn muốn làm toán, bạn muốn `number`.
* Khi bạn muốn in gái trị trên màn hình, bạn cần `string` (một hay nhiều ký tự, từ, câu).
* Khi bạn muốn tạo một guyết định trên chương trình, bạn cần `boolean` (`true` hoặc `false`).

Các giá trị được thêm trực tiếp vào trong source code được gọi là *literals*. `string` literals được bao bằng dấu ngoặc kép  `"..."` hoặc dấu ngoặc đơn (`'...'`) -- chỉ là phong cách khác nhau. `number` và `boolean` literals được đại diện như là ví dụ ( `42`, `true`, etc.).

Hãy xem:

```js
"I am a string";
'I am also a string';

42;

true;
false;
```

Ngoài kiểu  giá trị `string`/`number`/`boolean`, các ngôn ngữ lập trình còn thường cung cấp kiểu *arrays*, *objects*, *functions*, và hơn nữa. Chúng ta sẽ tìm hiểu thêm về giá trị và kiểu suốt chương này và chương kế tiếp.

### Chuyển đổi giữa các kiểu

Nếu bạn có một `number` nhưng muốn hiển thị trên màn hình, bạn cần chuyển giá trị thành `string`, và trong JavaScript gọi việc chuyển đổi này là "cưỡng bức". Tương tự, nếu ai đó cho một loạt ký tự số vào trong form của trang thương mại điện tử, đó là `string`, nhưng nếu bạn muốn sử dụng giá trị để thực hiện phép toán, bạn cần *ép* nó thành `number`

JavaScript cung cấp một vài cơ sở khác nhau để ép buộc chuyển đổi các *kiểu*. Ví dụ:

```js
var a = "42";
var b = Number( a );

console.log( a );	// "42"
console.log( b );	// 42
```

Sử dụng `Number(..)` (một function có sẵn) như đã thấy là một sự cưỡng bức *minh bạch* từ bất kỳ kiểu nào sang `number`. Nó hơi rõ ràng.

Nhưng có một chủ đề gây tranh cãi rằng những gì xảy ra khi bạn muốn so sánh hai giá trị của cùng một kiểu, nó cần một cưỡng bức *ngầm*.

Khi so sánh chuỗi `"99.99"` với số `99.99`, hầu hết mọi người đồng ý rằng nó bằng nhau. Nhưng nó không chính xác, đúng không? Nó có chung giá trị trong hai kiểu đại diện khác nhau, hai *kiểu* khác nhau. Bạn có thể nói là "bằng tương đối", đúng không?

Để giúp bạn thoát khỏi hoàn cảnh như vậy, JavaScript đôi khi đưa vào một sự cưỡng bức giá  trị *ngầm* đến kiểu phù hợp.

Vì vậy nếu bạn sử dụng dấu bằng tương đối `==` để so sánh `"99.99" == 99.99`, JavaScript sẽ chuyển phía bên trái `"99.99"` thành `number` tương đương `99.99`. Sự so sánh trở thành `99.99 == 99.99`, nghĩa là đương nhiên `true`.

Khi được thiết kế để giúp bạn, cưỡng bức ngầm có thể mang đến sự hoang mang nếu bạn không có thời gian để học luật quản lý hành vi của nó. Hầu hết JS dev chưa bao giờ tìm hiểu, vì vậy cảm giác thường tình là sự cưỡng bức ngầm này gây hoang mang và tạo ra bug ngoài ý muốn, vậy nên phải tránh. Nó đôi khi được gọi là lỗ hổng trong thiết kế ngôn ngữ.

Tuy nhiên, cưỡng bức ngầm là một cơ chế *có thể học được*, và hơn thế nữa là *cần được học* bởi bất kỳ ai muốn học lập trình JavaScript một cách nghiêm túc. Nó không chỉ làm bạn hết hoang mang khi bạn học luật này, nó còn làm cho bạn lập trình tốt hơn. Hiệu quả xứng đáng.

**Ghi chú:** Để biết thêm về sự cưỡng bức, xem Chương 2 của tiêu đề này và Chương 4 của "Kiểu & Ngữ pháp".

## Code comment

Nhân viên cửa hàng điện thoại có thể liệt kê vài tính năng của loại điện thoại mới hoặc một số kế hoạch của công ty cô ta đưa ra. Những ghi chú đó chỉ dành cho nhân vân -- nó không phải dành cho khách hàng coi. Tuy nhiên, những ghi chú đó đó giúp cô ta làm việc tốt hơn bằng cách tài liệu hoá những gì cô có thể nói với khách hàng.

Một trong những bài học quan trọng là bạn có thể học bằng cách viết code không chỉ cho máy tính. Code is every bit as much, if not more, for the developer as it is for the compiler.

Máy tính của bạn chỉ quan tâm đến mã máy, một chuỗi nhị phân 0s và 1s đến *biên dịch*. Có vô số chương trình mà bạn có thể viết và mang lại kết quả tương tự với chuỗi 0 - 1. Sự lựa chọn là bạn viết chương trình tế nào -- không chỉ riêng bạn, mà còn là thành viên khác trong nhóm và kể cả tương lai của chính bạn.

Bạn có thể phấn đâu không chỉ viết chương trình cho nó chạy đúng, mà còn dễ dàng kiểm định. Bạn có thể mất nhiều thời gian cho việc chọn một cái tên tốt cho biến, cho function.

Nhưng phần quan trọng khác là code comment. Có một số văn bản trong chương trình của bạn được đưa vào thuần tuý là để giải thích cho người. Trình thông dịch/ biên dịch sẽ bỏ qua những comment đó.

Có rất nhiều quan điểm về việc điều gì làm nên một comment tốt; chúng ta thực sự không thể xác định một quy tắc bao trùm. Nhưng có một số nhận xét và hướng dẫn có ích:

* Code không có comment là chưa đủ tối ưu.
* Quá nhiều comment là dấu hiệu của code tồi.
* Comment nên giải thích *vì sao* thay vì *cái gì*. Nó có thể giải thích *vì sao* nếu có chút phức tạp.

Trong JavaScript, có hai kiểu comment khả dụng: là comment dòng đơn và comment đa dòng.

Nhận biết:

```js
// Đây là comment dòng đơn

/* Đây là
      comment
             đa dòng
                      */
```

`//` Comment dòng đơn thích hợp với việc bạn đặt comment ngay trên một biểu thức hoặc cuối dòng. Những gì trên dòng có dấu `//` đều được coi như là comment (được bỏ qua bởi biên dịch) từ đầu cho đến cuối dòng đó. Không có bất kỳ cá biệt nào có thể xuất hiện trên comment dòn đơn.

Xem:

```js
var a = 42;		// 42 là ý nghĩa cuộc đời
```

`/* .. */` Comment đa dòng thích hợp với việc nếu bạn có một vài dòng cần giải thích.

Đây là cách sử dụng comment đa dòng thông thường:

```js
/* Giá trị sau đây được sử dụng bởi vì
	nó đã thể hiện rằng nó chính là câu
	trả lời mọi câu hỏi trong vũ trụ  */
var a = 42;
```
Nó có thể xuất hiện trong một dòng, thậm chí giữa dòng, bởi vì có `*/` là kết thúc. Ví dụ:


```js
var a = /* arbitrary value */ 42;

console.log( a );	// 42
```

Thứ duy nhất không thể xuất hiện trong comment đa dòng là `*/`, bởi vì nó có thể tạo ra kết comment.

Bạn sẽ muốn bắt đầu học lập trình bằng cách bắt đầu bằng thói quen comment code. Trong suốt phần còn lại của chương này, bạn sẽ thấy bạn muốn dùng comment để giải thích các thứ, hãy làm tương tự với sự luyện tập của bạn. Tin tôi đi, mọi người đọc code của bạn sẽ cảm ơn bạn!

## Biến

Hầu hết chương trình hữu ích đều muốn theo dõi một giá trị và sự thay đổi của chúng suốt chương trình, trải qua những biểu thức được gọi theo dự tính của chương trình.

Cách đơn giản nhất để thực hiện điều này là gán một giá trị cho một biểu tượng chứa nó, nó gọi là *biến* -- bởi vì giá trị trong vật chứa này có thể *biến đổi* trong suốt quá trình như mong muốn.

Vài ngôn ngữ lập trình, bạn khai báo một biến để chứa một giá trị có kiểu riêng biệt, ví dụ `number` hoặc `string`. `Static typing` hay còn gọi là *kiểu thực thi* thường được dẫn chứng là một lợi ích cho sự chính xác của chương trình bằng cách bảo vệ nó khỏi sự chuyển đổi giá trị không mong muốn.


Một số ngôn ngữ khác nhấn mạnh kiểu cho giá trị thay bì biến. *Weak typing*, hay còn gọi là *dynamic typing*, cho phép một biến có thể giữ bất kỳ kiểu giá trị tại bất kỳ thời điểm nào. Nó thường được dẫn chứng về lợi ích linh động của chương trình bằng cách cho phép một biến đại diện cho một giá trị cho dù nó là kiểu giá trị gì tại bất kỳ thời điểm nào theo chu trình logic của chương trình.

JavaScript sử dụng cách tiếp cận thứ hai, *dynamic typing*, nghĩa là biến có thể giữ bất kỳ giá trị của bất kỳ *kiểu* nào mà không bắt buộc kiểu thực thi.

Như đã giới thiệu, chúng ta khai báo một biến sử dụng biểu thức `var` -- chú ý rằng không có thông tin *kiểu* trong khai báo. Xem một chương trình đơn giản sau:

```js
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// chuyển `amount` sang một string, và thêm "$"
// ở đầu.
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```

Biến  `amount` ban đầu giữ một number `99.99`, và sau đó giữ kết quả `number` kết quả của `amount * 2` tức là `199.98`.

Lệnh `console.log(..)` đầu tiên *ngầm* buộc giá trị `number` thành `string` để in ra.

Tiếp đó lệnh `amount = "$" + String(amount)` *rõ ràng* ép giá trị `199.98` thành một `string` và thêm ký tự `"$"` đằng trước. Tại đây, `amount` có `string` giá trị `"$199.98"`, vì thế `console.log(..)` thứ hai không cần phải cưỡng ép gì cả để in nó ra.

Lập trình viên JavaScript sẽ lưu ý tính linh hoạt của việc sử dụng biến `amount` cho các giá trị `99.99`. `199.98`, và `"$199.98"`. Những người thích static-typing sẽ thích tách biến kiểu như `amountStr` để giữ gía trị `"$199.98"` cuối cùng, bởi vì nó là một kiểu khác.

Dù thế nào, bạn cũng để ý rằng `amount` giữ một giá trị đang chạy mà thay đổi trong quá trình hoạt động của chương trình, minh họa cho mục đích chính của các biến là: quản lý *state* của chương trình.

Nói theo cách khác, *state* là theo dõi các thay đổi của giá trị khi chương trình hoạt động.

Một cách sử dụng phổ biến khác của các biến là tập trung hóa thiết lập giá trị. Thường được gọi là *constants*, khi bạn khai báo một biết với một giá trị xác định *không thay đổi* suốt chương trình.

Bạn khai báo *constants*, thường là ở đầu chương trình, tiện để bạn có một chỗ để thay đổi giá trịnh khi cần. Theo quy ước, các biến JS hằng số thường được viết hoa, hay có gạch dưới `_` giữa các liên từ.

Dưới đây là ví dụ:

```js
var TAX_RATE = 0.08;	// 8% sales tax

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log( amount );				// 215.9784
console.log( amount.toFixed( 2 ) );	// "215.98"
```

**Chú ý:** Tương tự như làm thế nào`console.log(..)` là một function `log(..)` truy cập như một thuộc tính object trong giá trị `console`, `toFixed(..)` ở đây là một function có thể truy cập các giá trị `number`. Các `number` JavaScript không tự động định dạng cho dollars -- cơ chế không thể biết được ý định của bạn và không có kiểu tiền tệ. `toFixed(..)` cho phép chúng ta chỉ định làm tròn số thập phân `number` như mong muốn, và nó tạo ra `string` như ý.

Biến `TAX_RATE` là một *constant* theo quy ước -- không có gì đặc biệt trong chương trình ngăn cho nó không thay đổi. nhưng nếu thành phố tăng thuế bán lên 9%, chúng ta có thể dễ dàng cập nhật chương trình bằng cách cấu hình lại `TAX_RATE` bằng giá trị `0.09` cùng một nơi, thay vì phải tìm nhiều nhiều giá trị `0.08` rải rác khắp chương trình và cập nhật tất cả.

Phiên bản mới nhất của JS tại thời điểm biết bài này (thường gọi là "ES6") đã có cách mới để khai báo *constants*, bằng cách sửa dụng `const` thay cho `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ..
```
Const rất hữu dụng giống như var với giá trị không đổi, khác biệt là nó ngăn ngừa sự thay đổi giá trị vô tình xảy ra ở đâu đó sau giá trị khởi tạo. Nếu bạn thử gán giá trị khác cho `TAX_RATE` sau lần khai báo đầu tiên, chương trình của bạn sẽ từ chối thay đổi (trong strict mode, sẽ thất bại với lỗi -- xem "Strict Mode" trong Chương 2).

Tuy nhiên, cách "bảo vệ" khỏi các sai sót này cũng tương tự như các ngôn ngữ static-typing, nên bạn có thể thấy tại sao static types trong ngôn ngữ khác hấp dẫn.

**Lưu ý:** Để có thêm thông tin về sự khác biệt của các biến có thể sử dụng trong chương trình, xem  *Kiểu & Ngữ pháp* trong serie này.

## Blocks

Nhân viên cửa hàng điện thoại phải đi qua tất cả các khâu để hoàn tất việc thanh toán khi bạn mua điện thoại.

Tương tự, trong code chúng ta thường nhóm các biểu thức với nhau, thường được gọi l2 *block*. Trong JavaScript, một block được xác định bằng các bao một hoặc nhiều lệnh trong dấu ngoặc `{...}`. Ví dụ:

```js
var amount = 99.99;

// a general block
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```
Kiểu block `{...}` chung này hợp lệ, nhưng không thường thấy trong các chương trình JS. Thông thường, block được gắn liền với một lệnh điều khiển, chẳng hạn như biểu thức `if` (xem "Điều kiện") hoặc một vòng lặp (xem "Vòng lặp"). Ví dụ:

```js
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```
Chúng ta sẽ giải thích `if` trong phần tiếp theo, nhưng như bạn thấy, `{...}` block có 2 câu lệnh được gắn với `if (amount > 10)`; câu lệnh bên trong block sẽ chỉ thực hiện khi điều kiện đáp ứng.

**Chú ý:** Không giống hầu hết các lệnh khác như `console.log(amount);`, một block lệnh không cần dấu chấm phẩy (`;`) để kết thúc.

## Điều kiện

"Bạn có muốn thêm bảo vệ màn hình với chỉ $9.99 không?" Nhân viên hỗ trợ cửa hàng điện thoại đã tạo một quyết định cho bạn. Nhưng rõ ràng, đây chỉ là một câu hỏi "yes hoặc no".

Có nhiều cách để bạn diễn đạt *điều kiện* trong chương trình.

Loại thường thấy là lệnh `if`. Về cơ bản, khi bạn nói "*Nếu* điều kiện này đúng, hãy làm theo...". Ví dụ:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```
Lệnh `if` cần một diễn đạt trong dấu ngoặc đơn `()` được thể hiện như `true` hoặc `false`. Trong ví dụ, chúng ta có biểu thức `amount < bank_balance`, nó sẽ xác định `true` hoặc `false` tùy thuộc vào giá trị của biến `bank_balance`.

Bạn cũng có thể cung cấp một sự thay thế điều kiện nếu nó không thỏa, gọi mệnh đề `else`:

```js
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// can we afford the extra purchase?
if ( amount < bank_balance ) {
	console.log( "I'll take the accessory!" );
	amount = amount + ACCESSORY_PRICE;
}
// otherwise:
else {
	console.log( "No, thanks." );
}
```
Tại đây, nếu `amount < bank_balance` là `true`, chúng ta sẽ in ra `"I'll take the accessory!"` và thêm `9.99` vào biến `amount`. Nếu không, mệnh đề `else` nói ra chúng ta sẽ trả lời lịch sự `"No, thanks."` và `amount` không thay đổi.

Như chúng ta đã thảo luận trong "Giá trị & Loại" ở trên, các giá trị không chuẩn bị sẵn cho kiểu mong muốn mà thường bị ép theo kiểu đó, lệnh `if` muốn kiểu `boolean`, nhưng nếu bạn chèn nó gì đó không phải `boolean`, sự cưỡng ép sẽ xảy ra.

JavaScript xác định một danh sách các giá trị đặc trưng được coi là "falsy" bởi vì khi bị ép là `boolean`, nó sẽ trở thành `false` -- nó bao gồm các giá trị như `0` và `""`. Bất kỳ giá trị nào không trong danh sách "falsy", sẽ tự động "truthy" -- khi bị ép sang `boolean` nó sẽ trở thành `true`. Các giá trị truthy bao gồm kiểu như `99.99` và `"free"`. Xem "Truthy & Falsy" trong Chương 2.

Các *Điều kiện* hiện hữu trong các dạng khác ngoài `if`. Ví dụ, lệnh `switch` có thể được sử dụng như như là viết tắt của một loạt lệnh `if..else` (xem Chương 2). Vòng lặp sử một *điều kiện* để diễn đạt nếu vòng lặp diễn ra tiếp hay dừng lại.

**Chú ý:** Thông tin sâu hơn về sự ép buộc có thể xảy ra ngầm trong các biểu thức kiểm tra của các *điều kiện*, xem Chương 4 đề mục *Kiểu & Ngữ pháp* của serie này.

## Vòng lặp

Trong suốt quá trình bận rộn, có một danh sách khách hàng cần nói chuyện với nhân viên cửa hàng. Trong khi vẫn còn người trong danh sách đó, cô ta chỉ cần tiếp tục phục vụ khách hàng tiếp theo.

Lập lại một tập hợp hành động cho đến khi có một điều kiện nhất định thất bại -- nói cách khác, chỉ lặp lại khi điều kiện thỏa mãn -- là công việc của vòng lặp chương trình; vòng lặp có thể có nhiều hình thức khác nhau, nhưng tất cả đều đáp ứng hành vi cơ bản này.

Một vòng lặp bao gồm điều kiện kiểm tra cũng như một block (thường là `{ .. }`). Mỗi lần block vòng lặp được thực hiện, nó được gọi là *sự lặp lại*.

Ví dụ, vòng lặp  `while` và dạng `do..while` minh họa khái niệm lặp lại một block câu lệnh cho đến khi điều kiện không còn được đánh giá là `true`:

```js
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

Sự khác biệt thực tế duy nhất giữa các vòng lặp này là liệu điều kiện được kiểm tra trước lần lặp đầu tiên (`while`) hay sau lần lặp đầu tiên (`do..while`).

Ở một trong hai dạng, nếu điều kiện kiểm tra là `false`, lần lặp kế tiếp sẽ không chạy. Có nghĩa là điều kiện khởi tạo là `false`, vòng lặp `while` sẽ không bao giờ chạy, nhưng một vòng lặp `do..while` sẽ chạy lần đầu.

Đôi lúc bạn thực hiện lặp với mục đính là đếp một nhóm số, như là từ `0` đến `9` (mười số). Bạn có thể thực hiện bằng cách thiết lập một biến lặp `i` ở giá trị `0` và tăng lên `1` ở mỗi lần lặp.

**Cảnh báo:**  Với vài lý do lịch sử, ngôn ngữ lập trình hầu hết luôn đếm mọi thứ từ không có gì, tức là `0` thay vì `1`. Nếu bạn chưa quen với cách tư duy đó, nó có thể hơi khó chịu lúc ban đầu. Dành thời gian để luyện việc đếm từ `0` cho đến khi cảm thấy thoải mái với nó.

Điều kiện được kiểm tra cho mỗi lần lặp, giống như nó có lệnh `if` bên trong vòng lặp.

Chúng ta có thể sử dụng lệnh `break` của JS để ngừng một vòng lặp. Đồng thời, chúng ta có thể thấy rằng thật dễ dàng một cách kinh khủng để tạo ra một vòng lặp chạy liên tục mà không có cơ chế `break`.

Hãy hình dung:

```js
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

**Cảnh báo:** Đây không phải là hình thức thực tế mà bạn muốn sử dụng vòng lặp của mình. Nó chỉ được giới thiệu với mục đích minh họa mà thôi.

Trong khi `while` (hay `do..while`) có thể hoàn thành nhiệm vụ thủ công, có một dạng cú pháp khác được gọi là vòng lặp `for`
cho dành mục đích đó.

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```
Như bạn thấy, trong cả hai trường hợp điều kiện `i <= 9` là `true` trong 10 lần lặp đầu tiên của một dạng vòng lặp (các giá trị `i` từ `0` đến `9`) trở thành `false` khi `i` đạt giá trị `10`.

Vòng lặp `for` có ba mệnh đề: mệnh đề khởi tạo (`var i=0`), mệnh đề kiểm tra điều kiện (`i <= 9`), và mệnh đề cập nhật (`i = i + 1`). Vì vậy nếu bạn định đếm với vòng lặp lặp đi lặp lại, `for` là hỉnh thức gọn gàng và dễ hiểu hơn để hiểu và viết.

Có một dạng vòng lặp đặc biệt khác nhằm mục đích lặp các giá trị cụ thể, chẳng hạn như các thuộc tính của một đối tượng (xem Chương 2) trong đó kiểm tra điều kiện nghĩa là tất cả các thuộc tính đã được xử lý. Nguyên lý "lặp cho đến khi điều kiện sai" giữ nguyên bất kể dạng lặp nào.

## Hàm (function)

Nhân viên cửa hàng điện thoại có thể không mang theo bàn tính để tính ra số thuế và số lượng mua cuối cùng. Đó là hạng mục cô ta cần xác định một lần và sử dụng nhiều lần. Ngược lại, công ty có phương tiện tính toán thanh toán (máy tính, tablet,...) với các chức năng có sẵn.

Tương tự, chương trình của bạn hầu hết cần tách code thành nhiều phần để sử dụng lại, thay vì lặp đi lặp lại bản thân. Cách làm điều này là xác định một `function`.

Một hàm thường là một phần code được đặt tên và có thể được gọi bằng tên, code bên trong nó sẽ chạy cho mỗi lần gọi.

```js
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```
Các hàm có thể tùy ý lấy đối số (aka tham số) -- giá trị mà bạn truyền vào. Và chúng có thể tùy ý trả lại giá trị.

```js
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
```
Hàm `printAmount(..)` lấy một tham số gọi là `amt`. Hàm `formatAmount()` trả một giá trị. Dĩ nhiên, bạn có thể kết hợp hai kỹ thuật đó trong một hàm.

Các hàm thường được sử dụng với mục đích gọi ra nhiều lần, nhưng nó cũng có thể hữu ích trong việc tổ chức code, kể cả khi bạn chỉ gọi nó một lần.

Ví dụ:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

Mặc dù `calculateFinalPurchaseAmount(..)` chỉ được gọi một lần, tổ chức hành vi của nó thành những function tách biệt làm cho code trở nên logic rõ ràng hơn (lệnh  `amount = calculateFinal...`). Nếu function có nhiều lệnh bên trong nó, lợi ích thậm chí còn rõ ràng hơn.

### Scope

Nếu bạn hỏi nhân viên cửa hàng về một mẫu điện thoại mà cô ấy không có, cô ta sẽ không thể bán chiếc điện thoại bạn muốn. Cô ta chỉ có thể bán những chiếc điện thoại có trong kho. Bạn sẽ phải thử ở cửa hiệu khác để tìm chiếc điện thoại bạn muốn.

Lập trình có một thuật ngữ cho khái niệm này: *scope* ( kỹ thuật gọi là *lexical scope*). Trong JS, mỗi hàm đều có scope của nó. Scope cơ bản là một bộ tập hợp của các biến cũng như quy tắc cho các biến đó được gọi theo tên. Chỉ có code trong hàm mới có thể tiếp cận được với các biến trong *scope* của hàm đó.

Tên biến bên trong cùng scope phải là duy nhất -- nó không thể có hai biến `a` khác nhau tồn tại kế bên. Nhưng biến `a` trùng nhau có thể tồn tại trong các scope khác nhau.

```js
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```

Ngoài ra, một scope có thể lồng bên trong scope khác, giống như chú hề trong tiệc sinh nhật thổi quả bóng lồng trong quả bóng khác. Nếu một scope được lồng trong scope khác, code bên trong scope sâu nhất có thể tiếp cận với mọi biến ở các phạm vi.

Xem:

```js
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```

Các nguyên tắc của lexical scope cho phép code có thể truy cập các biến của phạm vi bên trong hay bên ngoài scope.

Do đó, code bên trong hàm `inner()` có thể truy cập cả hai biến `a` và `b`, nhưng code trong `outer()` chỉ có thể truy cập `a` -- nó không thể truy cập `b` bởi vì biến đó bên trong `inner()`

Nhắc lại đoạn mã trên:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}
```

Hằng (biến) `TAX_RATE` có thể truy cập từ bên trong function `calculateFinalPurchaseAmount(..)`, vì là lexical scope ta không cần gán nó vào trong.

**Ghi chú:** Nội dung về lexical scope, xem 3 chương đầu của *Scope & Closures*.

## Luyện tập

Không có sự thay thế nào bằng việc thực hành trong lập trình. Cũng không có nội dung rõ ràng nào trong bài viết của tôi biến bạn trở thành lập trình viên.

Bạn hãy ghi nhớ, hãy luyên tập các nguyên lý mà chúng ta đã học trong chương này. Tôi sẽ cho bạn vài "yêu cầu" và bạn thử trước. Sau đó tìm hiểu danh sách code dưới đây để biết cách mà tôi đã tiếp cận chúng.

* Viết một chương trình để tính toán tổng giá điện thoại bạn đã mua. Bạn sẽ tiếp tục mua điện thoại (gợi ý: vòng lặp!) cho đến khi bạn hết tiền trong tài khoản. Bạn đồng thời cũng sẽ mua phụ kiện cho mỗi cái điện thoại miễn là số tiền dưới ngưỡng chi tiêu.
* Sau khi bạn tính toán tổng tiền, thêm thuế, sau đó in ra tổng cuối cùng với định dạng hoàn chỉnh.
* Cuối cùng, kiểm tra số tiền đối với số dư trong tài khoản ngân hàng để xem bạn có đủ khả năng hay không.
* Bạn nên lập một vài hằng số cho "tax rate," "phone price," "accessory price," and "spending threshold," cũng như biến cho "bank account balance.""
* Bạn nên xác định các hàm cho việc tính toán thuế và định dạng giá tiền hoàn chỉnh với "$" và làm trong thành hai số thập phân.
* **Thách thức tăng cường:** Thử kết hợp đầu vào chương trình, có thể là `prompt(..)` được đề cập trong "Input" ở trên. Bạn có thể nhắc người dùng về số dư trong tài khoản, ví dụ vậy. Chúc vui và sáng tạo.

OK, triển thôi. Đừng nhìn code của tôi cho đến khi bạn có gì đó!

**Ghi chú:** Bởi đây là cuốn sách JavaScript, Tôi sẽ giải bài tập bằng JavaScript. Nhưng bạn có thể thực hiện nó với ngôn ngữ khác nếu bạn cảm thấy thoải mái hơn.

Đây là giải pháp của tôi:

```js
const SPENDING_THRESHOLD = 200;
const TAX_RATE = 0.08;
const PHONE_PRICE = 99.99;
const ACCESSORY_PRICE = 9.99;

var bank_balance = 303.91;
var amount = 0;

function calculateTax(amount) {
	return amount * TAX_RATE;
}

function formatAmount(amount) {
	return "$" + amount.toFixed( 2 );
}

// còn tiền thì mua tiếp
while (amount < bank_balance) {
	// mua điện thoại mới!
	amount = amount + PHONE_PRICE;

	// chúng ta có thể mua phụ kiện được không?
	if (amount < SPENDING_THRESHOLD) {
		amount = amount + ACCESSORY_PRICE;
	}
}

// đừng quên trả tiền cho chính phủ
amount = amount + calculateTax( amount );

console.log(
	"Your purchase: " + formatAmount( amount )
);
// Bạn dùng hết: $334.76

// bạn trả tiền nổi không?
if (amount > bank_balance) {
	console.log(
		"You can't afford this purchase. :("
	);
}
// Bạn không trả nổi. :(
```

**Ghi chú:** Cách đơn giản nhất để chạy chương trình JavaScript là gõ trên console trình duyệt mà bạn quen thuộc.

Bạn đã làm như thế nào? Cũng không khổ sở gì để thử lại nếu bạn đã thấy code của tôi. Có thể chơi một chút bằng cách thay đổi vài hằng số và xem chương trình nó chạy với những giá trị khác nhau.

## Ôn lại

Học lập trình không cần thiết phải theo tiến trình phức tạp và nặng nề. Nó chỉ có vài nguyên lý cơ bản bạn cần ghi nhớ trong đầu.

Việc học cũng như xây nhà. Để tạo ra một tòa tháp cao, bạn phải bắt đầu đặt những viên gạch này lên trên những viên gạch khác. Đây là vài chương trình cần thiết để xây tháp:

* Bạn cần *toán tử* để thực hiện hành động trên các giá trị.
* Bạn cần các giá trị và *kiểu* để thực hiện vài kiểu tác vụ khác nhau như toán với `number` hoặc xuất đầu ra với `string`.
* Bạn cần *biến* để lưu trữ dữ liệu (cũng như *state*) trong suốt quá trình thực thi chương trình.
* Bạn cần *điều kiện* như câu lệnh `if` để ra quyết định.
* Bạn cần *vòng lặp* để lặp lại tác vụ cho đến khi điều kiện ngừng xác thực.
* Bạn cần *hàm* để tổ chức code của bạn thành những thành phần logic và tái sử dụng.

Bình luận code là một trong những các hiệu quả để giúp cho code dễ đọc, làm cho chương trình của bạn dể hiểu, dễ bảo trì và sửa chữa sau đó nếu có vấn đề.

Cuối cùng, đừng bỏ qua sức mạnh của thực hành. Cách tốt nhất để học lập trình là lập trình.

