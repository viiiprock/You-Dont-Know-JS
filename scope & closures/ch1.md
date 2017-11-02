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

**Ghi chú:** You might be tempted to conceptualize the function declaration `function foo(a) {...` as a normal variable declaration and assignment, such as `var foo` and `foo = function(a){...`. In so doing, it would be tempting to think of this function declaration as involving an LHS look-up.

However, the subtle but important difference is that *Compiler* handles both the declaration and the value definition during code-generation, such that when *Engine* is executing code, there's no processing necessary to "assign" a function value to `foo`. Thus, it's not really appropriate to think of a function declaration as an LHS look-up assignment in the way we're discussing them here.

### Engine/Scope Conversation

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Let's imagine the above exchange (which processes this code snippet) as a conversation. The conversation would go a little something like this:

> ***Engine***: Ê *Scope*, I have an RHS reference for `foo`. Ever heard of it?

> ***Scope***: Why yes, I have. *Compiler* declared it just a second ago. He's a function. Here you go.

> ***Engine***: Great, thanks! OK, I'm executing `foo`.

> ***Engine***: Hey, *Scope*, I've got an LHS reference for `a`, ever heard of it?

> ***Scope***: Why yes, I have. *Compiler* declared it as a formal parameter to `foo` just recently. Here you go.

> ***Engine***: Helpful as always, *Scope*. Thanks again. Now, time to assign `2` to `a`.

> ***Engine***: Hey, *Scope*, sorry to bother you again. I need an RHS look-up for `console`. Ever heard of it?

> ***Scope***: No problem, *Engine*, this is what I do all day. Yes, I've got `console`. He's built-in. Here ya go.

> ***Engine***: Perfect. Looking up `log(..)`. OK, great, it's a function.

> ***Engine***: Yo, *Scope*. Can you help me out with an RHS reference to `a`. I think I remember it, but just want to double-check.

> ***Scope***: You're right, *Engine*. Same guy, hasn't changed. Here ya go.

> ***Engine***: Cool. Passing the value of `a`, which is `2`, into `log(..)`.

> ...

### Quiz

Check your understanding so far. Make sure to play the part of *Engine* and have a "conversation" with the *Scope*:

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identify all the LHS look-ups (there are 3!).

2. Identify all the RHS look-ups (there are 4!).

**Note:** See the chapter review for the quiz answers!

## Nested Scope

We said that *Scope* is a set of rules for looking up variables by their identifier name. There's usually more than one *Scope* to consider, however.

Just as a block or function is nested inside another block or function, scopes are nested inside other scopes. So, if a variable cannot be found in the immediate scope, *Engine* consults the next outer containing scope, continuing until found or until the outermost (aka, global) scope has been reached.

Consider:

```js
function foo(a) {
	console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```

The RHS reference for `b` cannot be resolved inside the function `foo`, but it can be resolved in the *Scope* surrounding it (in this case, the global).

So, revisiting the conversations between *Engine* and *Scope*, we'd overhear:

> ***Engine***: "Hey, *Scope* of `foo`, ever heard of `b`? Got an RHS reference for it."

> ***Scope***: "Nope, never heard of it. Go fish."

> ***Engine***: "Hey, *Scope* outside of `foo`, oh you're the global *Scope*, ok cool. Ever heard of `b`? Got an RHS reference for it."

> ***Scope***: "Yep, sure have. Here ya go."

The simple rules for traversing nested *Scope*: *Engine* starts at the currently executing *Scope*, looks for the variable there, then if not found, keeps going up one level, and so on. If the outermost global scope is reached, the search stops, whether it finds the variable or not.

### Building on Metaphors

To visualize the process of nested *Scope* resolution, I want you to think of this tall building.

<img src="fig1.png" width="250">

The building represents our program's nested *Scope* rule set. The first floor of the building represents your currently executing *Scope*, wherever you are. The top level of the building is the global *Scope*.

You resolve LHS and RHS references by looking on your current floor, and if you don't find it, taking the elevator to the next floor, looking there, then the next, and so on. Once you get to the top floor (the global *Scope*), you either find what you're looking for, or you don't. But you have to stop regardless.

## Errors

Why does it matter whether we call it LHS or RHS?

Because these two types of look-ups behave differently in the circumstance where the variable has not yet been declared (is not found in any consulted *Scope*).

Consider:

```js
function foo(a) {
	console.log( a + b );
	b = a;
}

foo( 2 );
```

When the RHS look-up occurs for `b` the first time, it will not be found. This is said to be an "undeclared" variable, because it is not found in the scope.

If an RHS look-up fails to ever find a variable, anywhere in the nested *Scope*s, this results in a `ReferenceError` being thrown by the *Engine*. It's important to note that the error is of the type `ReferenceError`.

By contrast, if the *Engine* is performing an LHS look-up and arrives at the top floor (global *Scope*) without finding it, and if the program is not running in "Strict Mode" [^note-strictmode], then the global *Scope* will create a new variable of that name **in the global scope**, and hand it back to *Engine*.

*"No, there wasn't one before, but I was helpful and created one for you."*

"Strict Mode" [^note-strictmode], which was added in ES5, has a number of different behaviors from normal/relaxed/lazy mode. One such behavior is that it disallows the automatic/implicit global variable creation. In that case, there would be no global *Scope*'d variable to hand back from an LHS look-up, and *Engine* would throw a `ReferenceError` similarly to the RHS case.

Now, if a variable is found for an RHS look-up, but you try to do something with its value that is impossible, such as trying to execute-as-function a non-function value, or reference a property on a `null` or `undefined` value, then *Engine* throws a different kind of error, called a `TypeError`.

`ReferenceError` is *Scope* resolution-failure related, whereas `TypeError` implies that *Scope* resolution was successful, but that there was an illegal/impossible action attempted against the result.

## Review (TL;DR)

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. *Scope*-related assignments can occur either with the `=` operator or by passing arguments to (assign to) function parameters.

The JavaScript *Engine* first compiles code before it executes, and in so doing, it splits up statements like `var a = 2;` into two separate steps:

1. First, `var a` to declare it in that *Scope*. This is performed at the beginning, before code execution.

2. Later, `a = 2` to look up the variable (LHS reference) and assign to it if found.

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
