# You Don't Know JS: Scope & Closures
# Chương 1: Scope (phạm vi) là gì?

Một trong những mô hình cơ bản của các ngôn ngữ lập trình là khả năng lưu trữ giá trị trong biến và sau đó gọi các giá trị đó hoặc thay đổi chúng. Thực tế, khả năng lưu trữ giá trị và lấy giá trị đó ra khỏi biến là cách tạo ra *state* của chương trình.

Không có khái niệm như vậy, một chương trình sẽ cực kỳ giới hạn và thậm chí không thú vị mặc dù nó có thể thực thi một số nhiệm vụ.

Nhưng việc đưa các biến vào trong chương trình của chúng ta đặt ra câu hỏi thú vị nhất mà chúng ta sẽ giải quyết: những biến đó *sống* ở đâu? Hay nói cách khác, nó được lưu ở đâu? Và quan trọng nhất là làm cách nào chương trình có thể tìm thấy nó khi cần?

Những câu hỏi nói lên sự cần thiết của các quy tắc được xác định rõ ràng của việc lưu trữ các biến ở đâu đó, cùng việc tìm các biến sau đó. Chúng ta gọi quy tắc đó là *Scope*

Nhưng, các quy tắc *Scope* được thiết lập ở đâu và như thế nào?

## Lý thuyết trình biên dịch

Tùy thuộc vào mức độ bạn tương tác với các ngôn ngữ khác nhau, nó có thể là hiển nhiên hoặc lạ lẫm, mặc dù thực tế JS là một ngôn ngữ nằm trong hạng mục "động" hoặc "giải nghĩa", thực tế nó là ngôn ngữ được biên dịch. Cũng như nhiều ngôn ngữ được biên dịch truyền thống khác, nó không được biên dịch trước hay cũng không phải là kết quả của sự kết hợp giữa các hệ thống phân phối khác nhau.

Tuy nhiên JS engine thực hiện nhiều bước đồng thời, theo những cách phức tạp hơn mà chúng ta từng biết, kể cả bất trình biên dịch truyền thống nào.

Trong tiến trình biên dịch của ngôn ngữ truyền thống, mã nguồn sẽ thông qua ba bước trước khi thực thi chương trình, gọi là biên dịch:

1. **Tokenizing/Lexing:** Tách các chuỗi ký tự thành các khối có ý nghĩa (đối với ngôn ngữ), được gọi là token (thẻ xác thực). Ví dụ: `var a = 2;`. Chương trình sẽ được phân thành các token sau: `var`, `a`, `=`, `2`, và `;`. Khoảng trắng có thể có hoặc không duy trì như một token, tùy thuộc vào nó có nghĩa hay không.

    **Ghi chú:** Sự khác biệt giữa tokenizing và lexing là tinh tế và học thuật (subtle and academic), nhưng nó tập trung vào việc các token này được xác định theo cách là có *trạng thái* hay *không trạng thái*. Nói đơn giản, nếu tokenizer gọi các quy tắc phân tích trạng thái để xác định liệu `a` có được xem là một token riêng biệt hay chỉ là một phần của token, *đó* chính là **lexing**.

2. **Phân tích cú pháp (parsing):** lấy một mảng (array) token và chuyển nó thành các phần tử cây lồng nhau, đại diện cho cấu trúc ngữ pháp của chương trình. Cây này được gọi là "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree).

    Cây của `var a = 2;` có thể bắt đầu với một nút cao nhất được gọi là `VariableDeclaration`, với một nút con được gọi là `Identifier` (giá trị `a`), và nút con khác được gọi là `AssignmentExpression` với bản thân nó có một con được gọi là `NumericLiteral` (giá trị `2`).

3. **Xử lý mã (Code-Generation):** là quá trình lấy AST và biến nó thành mã thực thi. Phần này khác nhau rất nhiều tùy thuộc vào ngôn ngữ, nền tảng mà nó nhắm đến, ...

	Vì vậy, thay vì đắm chìm vào chi tiết, chúng ta chỉ phẩy tay và nói rằng có cách để lấy AST được mô tả ở trên cho `var a = 2;` và chuyển nó thành một tập mã máy để thực sự *tạo* ra một biến gọi là `a` (bao gồm cả việc lưu trữ bộ nhớ, v.v...), và sao lưu trữ một giá trị vào `a`.

	**Ghi chú:** Chi tiết về quản lý hệ thống của máy sâu hơn những gì chúng ta sẽ tìm hiểu, vì vậy chúng ta chỉ cần biết rằng máy có thể tạo và lưu trữ biến theo mong muốn.

Cũng như những ngôn ngữ biên dịch khác, JavaScript engine phức tạp và mênh mông hơn là chỉ có ba bước trên. Ví dụ, trong tiến trình phân tích và xử lý mã, trong đó đã có các bước tối ưu hiệu suất thực thi, bao gồm gộp các phần tử thừa,...

Vì vậy, tôi chỉ tô vẽ những nét lớn ở đây. Nhưng tôi nghĩ bạn sẽ mau nhận ra vì sao những chi tiết đó chúng ta cần biết, thậm chí là ở mức cao.

Có một điều là JS engine không có nhiều thời gian tối ưu hóa sang chảnh như các trình biên dịch ngôn ngữ khác, bởi việc biên dịch JS không xảy ra trước trong bước xây dựng như những ngôn ngữ khác.

Với JavaScript, trong nhiều trường hợp, sự biên dịch được xảy ra trong vài mili giây (hoặc ít hơn) trước khi code được thực thi. Để đảm bảo hiệu suất nhanh nhất, JS engine sử dụng tất cả các mẹo vượt qua luôn cả phạm vi chúng ta thảo luận ở đây ( như JITs, biên dịch chậm và thậm chí biên dịch lại, ...).

Nói cho đơn giản, bất kỳ đoạn code JavaScript cũng phải được biên dịch trước khi nó thực thi (thường là *ngày trước khi*). Vậy, trình biên dịch JS sẽ lấy `var a = 2;` và biên dịch nó *trước*, sau đó sẽ thực thi nó, thường là ngay tức thì.

## Hiểu Scope

Cách chúng ta tiếp cận học scope là nghĩ về quá trình của một cuộc hội thoại. Nhưng *ai* có cuộc hội thoại đó?

### Vai diễn

Hãy gặp vai diễn của các nhân vật tương tác để tiếp cận chương trình `var a = 2;` để hiểu đoạn hội thoại mà chúng ta sẽ được xem:

1. *Engine*: chịu trách nhiệm biên soạn và thực thi từ đầu đến kết thúc chương trình JavaScript của chúng ta.

2. *Compiler*: Một trong những người bạn của *Engine*; phụ trách tất cả các việc nặng nề của của phân tích cú pháp và xử lý mã (xem phần trên).

3. *Scope*: người bạn khác của *Engine*; tập hợp và duy trì một danh sách các định danh đã khai báo (biến), và thực hiện một tập hợp các quy tắc nghiêm ngặt về cách thức truy cập các mã vừa thực thi.

Để bạn *hiểu đầy đủ* hơn JavaSript làm việc như thế nào, bạn cần bắt đầu *nghĩ* như *Engine* (và bạn bè), đặt những câu hỏi chúng hỏi, đồng thời trả lời những câu hỏi đó.

### Trước và sau

Khi bạn thấy chương trình `var a = 2;` bạn hầu như nghĩ nó như một câu lệnh. Nhưng mà anh bạn mới *Engine* của chúng ta không thấy vậy. Chính xác là *Engine* thấy hai câu lệnh riêng biệt, một do *Compiler* sẽ phụ trách trong quá trình biên dịch, và một do *Engine* sẽ phụ trách trong quá trình thực thi.

Vậy, hãy phân tích cách *Engine* và bạn bè đã tiếp cận `var a = 2;`

Đầu tiên *Compiler* sẽ thực hiện phân tích để tách nó ra thành các token, các token này sẽ tách thành cây. Nhưng khi *Compiler* thực hiện xử lý mã, nó sẽ coi chương trình hơi khác so với giả định.

Một giả định hợp lý là *Compiler* sẽ tạo ra mã có thể được tóm tắt bởi mã giả: "Phân bổ bộ nhớ cho biến, gắn nhãn `a` cho nó, sau đó gán giá trị `2` cho biến đó". Thật không may là nó không hẳn chính xác.

*Compiler* sẽ làm như sau:

1. Gặp `var a`, *Compiler* hỏi *Scope* xem nếu biến `a` có tồn tại trong bộ scope cụ thể đó hay không. Nếu có, *Compiler* bỏ qua khai báo và tiếp tục. Ngược lại, *Compiler* yêu cầu *Scope* khai báo một biến mới tên `a`.

2. *Compiler* sau đó tạo ra code cho *Engine* thực thi sau đó, thực hiện việc gán `a = 2`. Khi *Engine* chạy sẽ hỏi *Scope* rằng có biến nào trong bộ sưu tập tên là `a` có thể truy cập được hay không. Nếu có, *Engine* sử dụng biến 9do1, nếu không thì *Engine* sẽ tìm ở *đâu đó* (xem phần *Scope* lồng nhau bên dưới).

Nếu *Engine* cuối cùng tìm thấy một biến, nó gán giá trị `2` vào đó. Nếu không, *Engine* sẽ hô lên là có lỗi!

Túm lại: Hai hành động riêng biệt được thực hiện cho một phép gán biến: Một là *Compiler* khai báo một biến (nếu chưa có trong scope hiện tại), và thứ hai là khi thực thi, *Engine* tìm biến trong *SCope* và nếu tìm thấy thì sẽ gán nó vào.

### Lời của Compiler

Chúng ta cần một chút thuật ngữ của compiler để tiếp tục hiểu sâu hơn.

Khi *Engine* thực thi code do *Compiler* tạo ra cho bước (2), nó phải tra cứu biến `a` để xem nó đã khai báo hay chưa, và việc tra cứu này là cố vấn *Scope*. Nhưng kiểu tra cứu *Engine* thực hiện ảnh hưởng đến kết quả của việc tra cứu.

Trong trường hợp của chúng ta, *Engine* thực hiện một tra cứu "LHS" cho biến `a`. Một kiểu tra cứu khác là "RHS".

Tôi cá là bạn có thể đoán "L" và "R" nghĩa là gì. Cụm từ để chỉ cho "Left-hand Side (Phía bên trái)" và "Right-hand Side (Phía bên phải)".

Phía... cùa cái gì? **Của một phép gán**

Nói cách khác, tra cứu LHS được hoàn tất khi một biến xuất hiện ở bên trái của phép gán, và một tra cứu RHS hoàn tất khi một biến xuất hiện ở bên phải của phép gán.

Chính xác hơn một chút, tìm kiếm RHS không thể phân biệt được với mục đích của chúng ta, đơn giản chỉ là tra cứu của một giá trị của biến nào đó, trong khi tra cứu LHS để tìm vùng chứa biến để gán. Bằng cách này, RHS không *thực sự* nghĩa là "phía bên phải của một phép gán" cho chính nó, mà chính xác chỉ là "không phải phía bên trái".

Ngắn gọn hơn, bạn có thể nghĩ "RHS" thay vì "lấy đi nguồn (giá trị) của nó" thì là "đi lấy giá trị của..."

Đào sâu hơn chút. Khi tôi nói:

```js
console.log(a);
```

Tham chiếu đến `a` là một tham chiếu RHS, bởi vì không có gì được gán cho `a` cả. Thay vào đó, chúng ta đang tìm kiếm để lấy giá trị của `a` để truyền vào `console.log(..)`.

Ngược lại:

```js
a = 2;
```

Tham chiếu của `a` ở đây là tham chiếu LHS, bởi vị chúng ta không cần quan tâm giá trị gần nhất là gì, chúng ta chỉ đơn giản muốn tìm biến mục tiêu của phép gán `=a`

**Ghi chú:** LHS và RHS có nghĩa "phép gán bên trái/phải" không nhất thiết có phải chính xác là "phía bên trái/phải của phép gán `=`". Có vài cách khác mà phép gán đó có thể xảy ra, do đó tốt hơn là nghĩ khái niệm đó như sau: "cái gì là mục tiêu của phép gán (LHS)" và "cái gì là nguồn của phép gán (RHS)"

Ví dụ chương trình có cả LHS và RHS dưới đây:

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Dòng cuối cùng gọi hàm `foo (..)` yêu cầu một tham chiếu RHS đến `foo`, có nghĩa là,"hãy tra cứu giá trị của `foo`, và đưa nó cho tôi". Hơn nữa, `(..)` có nghĩa là giá trị của `foo` cần được thực hiện, vì vậy nó thực sự là một hàm!

**Bạn có phát hiện ra được** là có một phép gán tinh tế quan trọng ở đây không?

Bạn có thể quên ngụ ý của `a = 2` trong đoạn code này. Nó xảy ra khi giá trị `2` được truyền như một đối số trong hàm `foo(..)`, tức `2` được **gán** cho tham số `a`. Phép tra cứu LHS đã được thực hiện để (ngầm) gán đến tham số `a`.

Đồng thời cũng có một quan hệ RHS của giá trị `a`, và kết quả được chuyển đến `console.log(..)`. `console.log(..)` cần một tham chiếu để thực thi, đó là tra cứu RHS cho object `console`, sau đó xử lý thuộc tính thực hiện để xem có phương thức nào gọi là `log` hay không.

Cuối cùng, chúng ta có thể khái niệm hóa rằng có một trao đổi LHS / RHS chuyển giá trị `2` (bằng cách tra cứu RHS biến` a`) vào `log (..)`. Bên trong việc thực hiện `log (..)`, chúng ta có thể giả định rằng nó có các tham số, phần đầu (chắc gọi là `arg1`) có một tham chiếu LHS trước khi gán` 2` vào nó.

**Ghi chú:** Bạn có thể bị cám dỗ khái niệm hóa khai báo hàm `function foo(a) {...` như một khai báo biến và gán bình thường như `var foo` và `foo = function(a){...`. Làm như vậy thì nó sẽ hướng suy nghĩ rằng việc khai báo hàm này như một tra cứu LHS.

Tuy nhiên, sự khác biệt tinh tế quan trọng ở đây là *Compiler* đảm nhiệm cả việc khai báo và xác định giá trị trong quá trình tạo code. Chẳng hạn như khi *Engine* thực thi code, không cần phải thêm bước "gán" một giá trị hàm cho `foo`.

### Hội thoại Engine/Scope

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Hãy tưởng tượng sự trao đổi xử lý đoạn mã ở trên như một đoạn hội thoại. Đoạn hội thoại sẽ trở nên gì đó giống như vầy:

> ***Engine***: Ê *Scope*, tao có một tham chiếu RHS cho `foo`. Có thấy bao giờ chưa?

> ***Scope***: Có có, có chớ. *Compiler* mới khai báo một giây trước. Nó là hàm, đây nè.

> ***Engine***: Tuyệt, cảm ơn. OK, tao đang xử `foo`

> ***Engine***: Ê *Scope*, tao cũng có tham chiếu LHS cho `a` nè, mày biết ko?

> ***Scope***: Hả, có luôn. Đây nè, *Compiler* mới báo nó như một tham số cho `foo`.

> ***Engine***: Vẫn ngon như mọi khi, *Scope* ạ. Cảm ơn nhiều. Giờ thì gán `2` cho `a` thôi.

> ***Engine***: Ờ... *Scope*, làm phiền mày tiếp. Tao cần tra cứu RHS để kiếm `console`. Có không?

> ***Scope***: Không sao *Engine*, tao làm mấy chuyện này suốt ngày. Tao có `console` chớ, nó là hàm dựng sẵn mà.
> ***Engine***: Perfect. Looking up `log(..)`. OK, great, it's a function.

> ***Engine***: Yo, *Scope*, mày có thể giúp tao một tham chiếu RHS cho `a` không? Tao nghĩ là tao nhớ nó rồi, nhưng mà kiểm lại lần nữa.

> ***Scope***: Ờ đúng rồi, cũng là nó đó, không đổi gì hết.

> ***Engine***: Ngon rồi. Chuyển giá trị `2` cho `a` vào trong `log(..)`.

> ...

### Câu đố

Giờ kiểm tra khả năng hiểu của bạn. Mà nhớ là chơi trò này giống như là một phần của "hội thoại" của hai thằng *Engine* với *Scope*:

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Xác định tất cả tra cứu LHS (có 3!).

2. Xác định các tra cứu RHS ( có 4!).

**Ghi chú:** Xem lời giải ở Ôn tập!

## Scope lồng nhau

Chúng ta đã nói *Scope* là một tập hợp các quy tắc để tìm kiếm biến được nhận diện theo tên. Tuy nhiên thì thường có nhiều hơn một *Scope*.

Cũng như một cụm hàm được lồng vào cụm hàm khác, scope này cũng lồng trong scope kia. Nên nếu một biến không tìm thấy trong scope ngay liền, *Engine* sẽ tham khảo scope bên ngoài, tiếp tục kiếm cho tới khi nào ra tới ngoài cùng nhất (cũng là toàn cục).

Ví dụ:

```js
function foo(a) {
	console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```

Tham chiếu RHS cho `b` khộng thể tìm thấy trong hàm `foo`, nhưng nó có thể được thấy trong *Scope* bao ngoài (trong trường hợp này là toàn cục).

Vì vậy, quay lại đoạn hội thoại giữa *Engine* và *Scope*, chúng ta sẽ nghe:

> ***Engine***: "Uầy, *Scope* của `foo`, có `b` không? Có tham chiếu cho nó nè"

> ***Scope***: "Không thấy, chưa nghe bao giờ, thôi lượn đi"

> ***Engine***: "Này, *Scope* bên ngoài`foo`, à ờ mày là *Scope* toàn cục à, cũng được . Nghe thấy `b` bao giờ chưa? có tham chiếu RHS cho nó nè."

> ***Scope***: "Có, chắc cú. Đầy nè."

Nguyên tắc đơn giản để đi qua *Scope* lồng nhau là: *Engine* bắt đầu từ xử lý *Scope* gần nhất, tìm biến trong đó, nếu không có, lên tầng trên, và cứ tiếp tục như vậy. Tới khi ra tận phạm vi toàn cục, tìm thấy biến hay không thì việc tìm kiếm cũng dừng lại.

### Xây dựng trên phép ẩn dụ.

Để hình dung chi tiết *Scope* lồng nhau, bạn hãy nghĩ nó như một tòa nhà cao tầng.

<img src="fig1.png" width="250">

Tòa nhà đại diện cho một tập hợp *Scope* lồng nhau của chương trình. Tầng trệt của tòa nhà đại diện cho *Scope* thực thi gần nhất. Tầng cao nhất là *Scope* toàn cục.

Chúng ta giải quyết tham chiếu LHS và RHS bằng việc nhìn vào tầng hiện tại của bạn, và nếu bạn không tìm thấy nó, leo lên tầng tiếp theo rồi tìm, rồi tiếp tục như vậy. Cho đến khi bạn đến tầng thượng (*Scope* toàn cục), bạn có thể tìm thấy cái mình cần hoặc không. Bất kể sao thì bạn vẫn dừng lại.

## Lỗi

Tại sao lại quan trọng khi chúng ta gọi nó là LHS hay RHS?

Bởi vì có hai kiểu tra cứu hành xử khác nhau trong trường hợp biến chưa được khai báo (không tìm thấy trong bất *Scope* nào).

Ví dụ:

```js
function foo(a) {
	console.log( a + b );
	b = a;
}

foo( 2 );
```

Khi tra cứu RHS tìm `b` lần đầu xảy ra, nó sẽ không được tìm thấy. Đây được gọi là "chưa khai báo biến" bởi vì nó không tìm thấy trong phạm vi.

Nếu một tra cứu RHS thất bại khi tìm biến, kết quả `ReferenceError` sẽ được đưa ra bởi *Engine*. Việc báo lỗi `ReferenceError` quan trọng.

Ngược lại, nếu *Engine* thực hiện tra cứu LHS và đến tầng trên cùng rồi mà vẫn không thấy nó, và nếu chương trình không đang chạy ở "strict mode", *Scope* toàn cục sẽ tạo ra một biến mới theo tên đó **ở phạm vi toàn cục** và chuyển đến *Engine*

*"Không, chưa bao giờ có biến đó cả, nhưng tao sẽ hỗ trợ tạo một cái cho mày."*

"Strict Mode" được thêm vào từ ES5, có một số hành vi khác nhau từ chế độ bình thường/thoải mái/lười, một trong những hành vi như vậy sẽ không cho phép tự động tạo biến toàn cầu hay ngầm định. Trong trường hợp đó, sẽ không có biến nào của *Scope* toàn cục để trả cho tra cứu LHS, và *Engine*  sẽ đưa ra lỗi `ReferenceError` tương tự trường hợp RHS.

Giờ đây, nếu một biến được tìm thấy cho một tra cứu RHS, nhưng nếu cố làm gì đó với giá trị của nó là không thể, chẳng hạn như cố thực thi hàm cho một giá trị phi hàm hoặc tham chiếu một thuộc tính của giá trị `null` hay `undefined`, *Engine* sẽ đưa ra một loại lỗi khác, gọi là `TypeError`.

`ReferenceError` là lỗi liên quan của chi tiết *Scope* trong khi `TypeError` lại ngụ ý rằng chi tiết *Scope* đúng, nhưng lại có một hành động không hợp lệ/bất khả thi cố gắng đi ngược kết quả.

## Ôn tập (TL;DR)

Phạm vi là một tập hợp các nguyên tắc xác định biến được tìm ở đâu và như thế nào. Việc tra cứu có thể nhằm mục đích gán biến, là tham chiếu từ LHS, hay nhằm mục đích truy xuất dữ liệu của nó dựa trên tham chiếu RHS.

Tham chiếu LHS là kết quả của quá trình gán.Phép gán liên quan *Scope* có thể xảy ra với phép toán `=` hay đưa đối số (gán) đến tham số của hàm.

JavaScript *Engine* biên dịch code trước khi thực thi, và khi làm như vậy, nó chia các lệnh, ví dụ `var a = 2;`, thành hai bước riêng biệt:

1. Đầu tiên, `var a` để khai báo nó trong *Scope* đó. Việc này được thực hiện từ đầu, trước khi code được thực thi.

2. Sau đó, `a = 2` để tìm biến (LHS) và gán cho nó nếu nó được tìm thấy.

Cả tra cứu LHS và RHS đều được bắt đầu ở *Scope* thực thi gần nhất, và nếu cần thiết (tức là nó không tìm thấy cái nó đang tìm trong đó), nó sẽ hoạt động bằng cách đi lên trên *Scope* lồng nhau, một phạm vi (tầng trệt) trong một thời điểm,
Both LHS and RHS reference look-ups start at the currently executing *Scope*, and if need be (that is, they don't find what they're looking for there), they work their way up the nested *Scope*, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in `ReferenceError`s being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a `ReferenceError` (if in "Strict Mode" [^note-strictmode]).

### Quiz Answers

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identify all the LHS look-ups (there are 3!).

	**`c = ..`, `a = 2` (implicit param assignment) and `b = ..`**

2. Identify all the RHS look-ups (there are 4!).

    **`foo(2..`, `= a;`, `a + ..` and `.. + b`**


[^note-strictmode]: MDN: [Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode)
