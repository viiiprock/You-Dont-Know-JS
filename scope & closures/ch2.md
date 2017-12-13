# You Don't Know JS: Scope & Closures
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

Trên các dòng mã tiếp theo sau khi `eval(..)` được thực thi, *Engine* sẽ không biết hoặc quan tâm rằng code trước đó
On subsequent lines of code after an `eval(..)` has executed, the *Engine* will not "know" or "care" that the previous code in question was dynamically interpreted and thus modified the lexical scope environment. The *Engine* will simply perform its lexical scope look-ups as it always does.

Xem ví dụ sau:

```js
function foo(str, a) {
	eval( str ); // cheating!
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1, 3
```

The string `"var b = 3;"` is treated, at the point of the `eval(..)` call, as code that was there all along. Because that code happens to declare a new variable `b`, it modifies the existing lexical scope of `foo(..)`. In fact, as mentioned above, this code actually creates variable `b` inside of `foo(..)` that shadows the `b` that was declared in the outer (global) scope.

When the `console.log(..)` call occurs, it finds both `a` and `b` in the scope of `foo(..)`, and never finds the outer `b`. Thus, we print out "1, 3" instead of "1, 2" as would have normally been the case.

**Note:** In this example, for simplicity's sake, the string of "code" we pass in was a fixed literal. But it could easily have been programmatically created by adding characters together based on your program's logic. `eval(..)` is usually used to execute dynamically created code, as dynamically evaluating essentially static code from a string literal would provide no real benefit to just authoring the code directly.

By default, if a string of code that `eval(..)` executes contains one or more declarations (either variables or functions), this action modifies the existing lexical scope in which the `eval(..)` resides. Technically, `eval(..)` can be invoked "indirectly", through various tricks (beyond our discussion here), which causes it to instead execute in the context of the global scope, thus modifying it. But in either case, `eval(..)` can at runtime modify an author-time lexical scope.

**Note:** `eval(..)` when used in a strict-mode program operates in its own lexical scope, which means declarations made inside of the `eval()` do not actually modify the enclosing scope.

```js
function foo(str) {
   "use strict";
   eval( str );
   console.log( a ); // ReferenceError: a is not defined
}

foo( "var a = 2" );
```

There are other facilities in JavaScript which amount to a very similar effect to `eval(..)`. `setTimeout(..)` and `setInterval(..)` *can* take a string for their respective first argument, the contents of which are `eval`uated as the code of a dynamically-generated function. This is old, legacy behavior and long-since deprecated. Don't do it!

The `new Function(..)` function constructor similarly takes a string of code in its **last** argument to turn into a dynamically-generated function (the first argument(s), if any, are the named parameters for the new function). This function-constructor syntax is slightly safer than `eval(..)`, but it should still be avoided in your code.

The use-cases for dynamically generating code inside your program are incredibly rare, as the performance degradations are almost never worth the capability.

### `with`

The other frowned-upon (and now deprecated!) feature in JavaScript which cheats lexical scope is the `with` keyword. There are multiple valid ways that `with` can be explained, but I will choose here to explain it from the perspective of how it interacts with and affects lexical scope.

`with` is typically explained as a short-hand for making multiple property references against an object *without* repeating the object reference itself each time.

For example:

```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// more "tedious" to repeat "obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// "easier" short-hand
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}
```

However, there's much more going on here than just a convenient short-hand for object property access. Consider:

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
console.log( o2.a ); // undefined
console.log( a ); // 2 -- Oops, leaked global!
```

In this code example, two objects `o1` and `o2` are created. One has an `a` property, and the other does not. The `foo(..)` function takes an object reference `obj` as an argument, and calls `with (obj) { .. }` on the reference. Inside the `with` block, we make what appears to be a normal lexical reference to a variable `a`, an LHS reference in fact (see Chapter 1), to assign to it the value of `2`.

When we pass in `o1`, the `a = 2` assignment finds the property `o1.a` and assigns it the value `2`, as reflected in the subsequent `console.log(o1.a)` statement. However, when we pass in `o2`, since it does not have an `a` property, no such property is created, and `o2.a` remains `undefined`.

But then we note a peculiar side-effect, the fact that a global variable `a` was created by the `a = 2` assignment. How can this be?

The `with` statement takes an object, one which has zero or more properties, and **treats that object as if *it* is a wholly separate lexical scope**, and thus the object's properties are treated as lexically defined identifiers in that "scope".

**Note:** Even though a `with` block treats an object like a lexical scope, a normal `var` declaration inside that `with` block will not be scoped to that `with` block, but instead the containing function scope.

While the `eval(..)` function can modify existing lexical scope if it takes a string of code with one or more declarations in it, the `with` statement actually creates a **whole new lexical scope** out of thin air, from the object you pass to it.

Understood in this way, the "scope" declared by the `with` statement when we passed in `o1` was `o1`, and that "scope" had an "identifier" in it which corresponds to the `o1.a` property. But when we used `o2` as the "scope", it had no such `a` "identifier" in it, and so the normal rules of LHS identifier look-up (see Chapter 1) occurred.

Neither the "scope" of `o2`, nor the scope of `foo(..)`, nor the global scope even, has an `a` identifier to be found, so when `a = 2` is executed, it results in the automatic-global being created (since we're in non-strict mode).

It is a strange sort of mind-bending thought to see `with` turning, at runtime, an object and its properties into a "scope" *with* "identifiers". But that is the clearest explanation I can give for the results we see.

**Note:** In addition to being a bad idea to use, both `eval(..)` and `with` are affected (restricted) by Strict Mode. `with` is outright disallowed, whereas various forms of indirect or unsafe `eval(..)` are disallowed while retaining the core functionality.

### Performance

Both `eval(..)` and `with` cheat the otherwise author-time defined lexical scope by modifying or creating new lexical scope at runtime.

So, what's the big deal, you ask? If they offer more sophisticated functionality and coding flexibility, aren't these *good* features? **No.**

The JavaScript *Engine* has a number of performance optimizations that it performs during the compilation phase. Some of these boil down to being able to essentially statically analyze the code as it lexes, and pre-determine where all the variable and function declarations are, so that it takes less effort to resolve identifiers during execution.

But if the *Engine* finds an `eval(..)` or `with` in the code, it essentially has to *assume* that all its awareness of identifier location may be invalid, because it cannot know at lexing time exactly what code you may pass to `eval(..)` to modify the lexical scope, or the contents of the object you may pass to `with` to create a new lexical scope to be consulted.

In other words, in the pessimistic sense, most of those optimizations it *would* make are pointless if `eval(..)` or `with` are present, so it simply doesn't perform the optimizations *at all*.

Your code will almost certainly tend to run slower simply by the fact that you include an `eval(..)` or `with` anywhere in the code. No matter how smart the *Engine* may be about trying to limit the side-effects of these pessimistic assumptions, **there's no getting around the fact that without the optimizations, code runs slower.**

## Review (TL;DR)

Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is essentially able to know where and how all identifiers are declared, and thus predict how they will be looked-up during execution.

Two mechanisms in JavaScript can "cheat" lexical scope: `eval(..)` and `with`. The former can modify existing lexical scope (at runtime) by evaluating a string of "code" which has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at runtime) by treating an object reference *as* a "scope" and that object's properties as scoped identifiers.

The downside to these mechanisms is that it defeats the *Engine*'s ability to perform compile-time optimizations regarding scope look-up, because the *Engine* has to assume pessimistically that such optimizations will be invalid. Code *will* run slower as a result of using either feature. **Don't use them.**
