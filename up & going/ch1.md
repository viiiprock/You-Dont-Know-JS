# You Don't Know JS: Khởi động 
# Chương 1: Hiểu về lập trình

Chào mừng bạn đến với seri You Don't Know JS (YDKJS).

Khởi động là lời giới thiệu một số  khái niệm cơ bản của lập trình (dĩ nhiên là chúng ta đặc biệt hướng về phía JavaScript --viết tắt là JS--) và cách để tiếp và hiểu các nội dung trong seri này. Đặc biệt là nếu chúng ta tìm hiểu sâu lập trình và JavaScript, cuốn sách này sẽ khám phá tóm lược những gì bạn cần để up & going. 

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

Lệnh `console.log(..)`  đầu tiên command has to *implicitly* coerce that `number` value to a `string` to print it out.

Then the statement `amount = "$" + String(amount)` *explicitly* coerces the `199.98` value to a `string` and adds a `"$"` character to the beginning. At this point, `amount` now holds the `string` value `"$199.98"`, so the second `console.log(..)` statement doesn't need to do any coercion to print it out.

JavaScript developers will note the flexibility of using the `amount` variable for each of the `99.99`, `199.98`, and the `"$199.98"` values. Static-typing enthusiasts would prefer a separate variable like `amountStr` to hold the final `"$199.98"` representation of the value, because it's a different type.

Either way, you'll note that `amount` holds a running value that changes over the course of the program, illustrating the primary purpose of variables: managing program *state*.

In other words, *state* is tracking the changes to values as your program runs.

Another common usage of variables is for centralizing value setting. This is more typically called *constants*, when you declare a variable with a value and intend for that value to *not change* throughout the program.

You declare these *constants*, often at the top of a program, so that it's convenient for you to have one place to go to alter a value if you need to. By convention, JavaScript variables as constants are usually capitalized, with underscores `_` between multiple words.

Here's a silly example:

```js
var TAX_RATE = 0.08;	// 8% sales tax

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log( amount );				// 215.9784
console.log( amount.toFixed( 2 ) );	// "215.98"
```

**Note:** Similar to how `console.log(..)` is a function `log(..)` accessed as an object property on the `console` value, `toFixed(..)` here is a function that can be accessed on `number` values. JavaScript `number`s aren't automatically formatted for dollars -- the engine doesn't know what your intent is and there's no type for currency. `toFixed(..)` lets us specify how many decimal places we'd like the `number` rounded to, and it produces the `string` as necessary.

The `TAX_RATE` variable is only *constant* by convention -- there's nothing special in this program that prevents it from being changed. But if the city raises the sales tax rate to 9%, we can still easily update our program by setting the `TAX_RATE` assigned value to `0.09` in one place, instead of finding many occurrences of the value `0.08` strewn throughout the program and updating all of them.

The newest version of JavaScript at the time of this writing (commonly called "ES6") includes a new way to declare *constants*, by using `const` instead of `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ..
```

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting. If you tried to assign any different value to `TAX_RATE` after that first declaration, your program would reject the change (and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2).

By the way, that kind of "protection" against mistakes is similar to the static-typing type enforcement, so you can see why static types in other languages can be attractive!

**Note:** For more information about how different values in variables can be used in your programs, see the *Types & Grammar* title of this series.

## Blocks

The phone store employee must go through a series of steps to complete the checkout as you buy your new phone.

Similarly, in code we often need to group a series of statements together, which we often call a *block*. In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair `{ .. }`. Consider:

```js
var amount = 99.99;

// a general block
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

This kind of standalone `{ .. }` general block is valid, but isn't as commonly seen in JS programs. Typically, blocks are attached to some other control statement, such as an `if` statement (see "Conditionals") or a loop (see "Loops"). For example:

```js
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

We'll explain `if` statements in the next section, but as you can see, the `{ .. }` block with its two statements is attached to `if (amount > 10)`; the statements inside the block will only be processed if the conditional passes.

**Note:** Unlike most other statements like `console.log(amount);`, a block statement does not need a semicolon (`;`) to conclude it.

## Conditionals

"Do you want to add on the extra screen protectors to your purchase, for $9.99?" The helpful phone store employee has asked you to make a decision. And you may need to first consult the current *state* of your wallet or bank account to answer that question. But obviously, this is just a simple "yes or no" question.

There are quite a few ways we can express *conditionals* (aka decisions) in our programs.

The most common one is the `if` statement. Essentially, you're saying, "*If* this condition is true, do the following...". For example:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```

The `if` statement requires an expression in between the parentheses `( )` that can be treated as either `true` or `false`. In this program, we provided the expression `amount < bank_balance`, which indeed will either evaluate to `true` or `false` depending on the amount in the `bank_balance` variable.

You can even provide an alternative if the condition isn't true, called an `else` clause. Consider:

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

Here, if `amount < bank_balance` is `true`, we'll print out `"I'll take the accessory!"` and add the `9.99` to our `amount` variable. Otherwise, the `else` clause says we'll just politely respond with `"No, thanks."` and leave `amount` unchanged.

As we discussed in "Values & Types" earlier, values that aren't already of an expected type are often coerced to that type. The `if` statement expects a `boolean`, but if you pass it something that's not already `boolean`, coercion will occur.

JavaScript defines a list of specific values that are considered "falsy" because when coerced to a `boolean`, they become `false` -- these include values like `0` and `""`. Any other value not on the "falsy" list is automatically "truthy" -- when coerced to a `boolean` they become `true`. Truthy values include things like `99.99` and `"free"`. See "Truthy & Falsy" in Chapter 2 for more information.

*Conditionals* exist in other forms besides the `if`. For example, the `switch` statement can be used as a shorthand for a series of `if..else` statements (see Chapter 2). Loops (see "Loops") use a *conditional* to determine if the loop should keep going or stop.

**Note:** For deeper information about the coercions that can occur implicitly in the test expressions of *conditionals*, see Chapter 4 of the *Types & Grammar* title of this series.

## Loops

During busy times, there's a waiting list for customers who need to speak to the phone store employee. While there's still people on that list, she just needs to keep serving the next customer.

Repeating a set of actions until a certain condition fails -- in other words, repeating only while the condition holds -- is the job of programming loops; loops can take different forms, but they all satisfy this basic behavior.

A loop includes the test condition as well as a block (typically as `{ .. }`). Each time the loop block executes, that's called an *iteration*.

For example, the `while` loop and the `do..while` loop forms illustrate the concept of repeating a block of statements until a condition no longer evaluates to `true`:

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

The only practical difference between these loops is whether the conditional is tested before the first iteration (`while`) or after the first iteration (`do..while`).

In either form, if the conditional tests as `false`, the next iteration will not run. That means if the condition is initially `false`, a `while` loop will never run, but a `do..while` loop will run just the first time.

Sometimes you are looping for the intended purpose of counting a certain set of numbers, like from `0` to `9` (ten numbers). You can do that by setting a loop iteration variable like `i` at value `0` and incrementing it by `1` each iteration.

**Warning:** For a variety of historical reasons, programming languages almost always count things in a zero-based fashion, meaning starting with `0` instead of `1`. If you're not familiar with that mode of thinking, it can be quite confusing at first. Take some time to practice counting starting with `0` to become more comfortable with it!

The conditional is tested on each iteration, much as if there is an implied `if` statement inside the loop.

We can use JavaScript's `break` statement to stop a loop. Also, we can observe that it's awfully easy to create a loop that would otherwise run forever without a `break`ing mechanism.

Let's illustrate:

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

**Warning:** This is not necessarily a practical form you'd want to use for your loops. It's presented here for illustration purposes only.

While a `while` (or `do..while`) can accomplish the task manually, there's another syntactic form called a `for` loop for just that purpose:

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```

As you can see, in both cases the conditional `i <= 9` is `true` for the first 10 iterations (`i` of values `0` through `9`) of either loop form, but becomes `false` once `i` is value `10`.

The `for` loop has three clauses: the initialization clause (`var i=0`), the conditional test clause (`i <= 9`), and the update clause (`i = i + 1`). So if you're going to do counting with your loop iterations, `for` is a more compact and often easier form to understand and write.

There are other specialized loop forms that are intended to iterate over specific values, such as the properties of an object (see Chapter 2) where the implied conditional test is just whether all the properties have been processed. The "loop until a condition fails" concept holds no matter what the form of the loop.

## Functions

The phone store employee probably doesn't carry around a calculator to figure out the taxes and final purchase amount. That's a task she needs to define once and reuse over and over again. Odds are, the company has a checkout register (computer, tablet, etc.) with those "functions" built in.

Similarly, your program will almost certainly want to break up the code's tasks into reusable pieces, instead of repeatedly repeating yourself repetitiously (pun intended!). The way to do this is to define a `function`.

A function is generally a named section of code that can be "called" by name, and the code inside it will be run each time. Consider:

```js
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```

Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.

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

The function `printAmount(..)` takes a parameter that we call `amt`. The function `formatAmount()` returns a value. Of course, you can also combine those two techniques in the same function.

Functions are often used for code that you plan to call multiple times, but they can also be useful just to organize related bits of code into named collections, even if you only plan to call them once.

Consider:

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

Although `calculateFinalPurchaseAmount(..)` is only called once, organizing its behavior into a separate named function makes the code that uses its logic (the `amount = calculateFinal...` statement) cleaner. If the function had more statements in it, the benefits would be even more pronounced.

### Scope

If you ask the phone store employee for a phone model that her store doesn't carry, she will not be able to sell you the phone you want. She only has access to the phones in her store's inventory. You'll have to try another store to see if you can find the phone you're looking for.

Programming has a term for this concept: *scope* (technically called *lexical scope*). In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's *scoped* variables.

A variable name has to be unique within the same scope -- there can't be two different `a` variables sitting right next to each other. But the same variable name `a` could appear in different scopes.

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

Also, a scope can be nested inside another scope, just like if a clown at a birthday party blows up one balloon inside another balloon. If one scope is nested inside another, code inside the innermost scope can access variables from either scope.

Consider:

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

Lexical scope rules say that code in one scope can access variables of either that scope or any scope outside of it.

So, code inside the `inner()` function has access to both variables `a` and `b`, but code in `outer()` has access only to `a` -- it cannot access `b` because that variable is only inside `inner()`.

Recall this code snippet from earlier:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}
```

The `TAX_RATE` constant (variable) is accessible from inside the `calculateFinalPurchaseAmount(..)` function, even though we didn't pass it in, because of lexical scope.

**Note:** For more information about lexical scope, see the first three chapters of the *Scope & Closures* title of this series.

## Practice

There is absolutely no substitute for practice in learning programming. No amount of articulate writing on my part is alone going to make you a programmer.

With that in mind, let's try practicing some of the concepts we learned here in this chapter. I'll give the "requirements," and you try it first. Then consult the code listing below to see how I approached it.

* Write a program to calculate the total price of your phone purchase. You will keep purchasing phones (hint: loop!) until you run out of money in your bank account. You'll also buy accessories for each phone as long as your purchase amount is below your mental spending threshold.
* After you've calculated your purchase amount, add in the tax, then print out the calculated purchase amount, properly formatted.
* Finally, check the amount against your bank account balance to see if you can afford it or not.
* You should set up some constants for the "tax rate," "phone price," "accessory price," and "spending threshold," as well as a variable for your "bank account balance.""
* You should define functions for calculating the tax and for formatting the price with a "$" and rounding to two decimal places.
* **Bonus Challenge:** Try to incorporate input into this program, perhaps with the `prompt(..)` covered in "Input" earlier. You may prompt the user for their bank account balance, for example. Have fun and be creative!

OK, go ahead. Try it. Don't peek at my code listing until you've given it a shot yourself!

**Note:** Because this is a JavaScript book, I'm obviously going to solve the practice exercise in JavaScript. But you can do it in another language for now if you feel more comfortable.

Here's my JavaScript solution for this exercise:

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

// keep buying phones while you still have money
while (amount < bank_balance) {
	// buy a new phone!
	amount = amount + PHONE_PRICE;

	// can we afford the accessory?
	if (amount < SPENDING_THRESHOLD) {
		amount = amount + ACCESSORY_PRICE;
	}
}

// don't forget to pay the government, too
amount = amount + calculateTax( amount );

console.log(
	"Your purchase: " + formatAmount( amount )
);
// Your purchase: $334.76

// can you actually afford this purchase?
if (amount > bank_balance) {
	console.log(
		"You can't afford this purchase. :("
	);
}
// You can't afford this purchase. :(
```

**Note:** The simplest way to run this JavaScript program is to type it into the developer console of your nearest browser.

How did you do? It wouldn't hurt to try it again now that you've seen my code. And play around with changing some of the constants to see how the program runs with different values.

## Review

Learning programming doesn't have to be a complex and overwhelming process. There are just a few basic concepts you need to wrap your head around.

These act like building blocks. To build a tall tower, you start first by putting block on top of block on top of block. The same goes with programming. Here are some of the essential programming building blocks:

* You need *operators* to perform actions on values.
* You need values and *types* to perform different kinds of actions like math on `number`s or output with `string`s.
* You need *variables* to store data (aka *state*) during your program's execution.
* You need *conditionals* like `if` statements to make decisions.
* You need *loops* to repeat tasks until a condition stops being true.
* You need *functions* to organize your code into logical and reusable chunks.

Code comments are one effective way to write more readable code, which makes your program easier to understand, maintain, and fix later if there are problems.

Finally, don't neglect the power of practice. The best way to learn how to write code is to write code.

I'm excited you're well on your way to learning how to code, now! Keep it up. Don't forget to check out other beginner programming resources (books, blogs, online training, etc.). This chapter and this book are a great start, but they're just a brief introduction.

The next chapter will review many of the concepts from this chapter, but from a more JavaScript-specific perspective, which will highlight most of the major topics that are addressed in deeper detail throughout the rest of the series.
