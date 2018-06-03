# You Don't Know JS: _this_ & Object Prototypes

# Chương 2: Giờ mọi thứ đều hợp lý với `this` !

Trong Chương 1, chúng ta bỏ qua những quan niệm sai lầm về `this` và đã học được rằng `this` là ràng buộc được thực hiện cho mỗi lần gọi hàm và được dựa hoàn toàn vào **call-site** (cách gọi hàm) của nó.

## Call-site

Để hiểu ràng buộc `this`, chúng ta phải hiểu call-site: vị trí hàm được gọi trong code (**không phải nơi nó được khai báo**). Chúng ta cần quan sát call-site để trả lời câu hỏi: `this` này tham chiếu vào cái gì?

Đại khái tìm call-site là: "Xác định vị trí hàm được gọi", nhưng không phải lúc nào cũng dễ vậy, vì một số mô hình code có thể che khuất call-site _thực_.

Việc quan trọng là nghĩ về **call-stack** (cụm hàm đang được gọi ngay tại thời điểm thực thi). Call-site mà chúng ta quan tâm ở nơi gọi hàm _trước_ hàm thực thi gần nhất.

Ví dụ về call-stack và call-site:

```js
function baz() {
  // call-stack là: `baz`
  // vì vậy, call-site là scope toàn cục

  console.log("baz");
  bar(); // <-- call-site của `bar`
}

function bar() {
  // call-stack là: `baz` -> `bar`
  // vì vậy, call-site ở trong `baz`

  console.log("bar");
  foo(); // <-- call-site cho `foo`
}

function foo() {
  // call-stack là: `baz` -> `bar` -> `foo`
  // vì vậy, call-site ở trong `bar`

  console.log("foo");
}

baz(); // <-- call-site cho `baz`
```

Cẩn trọng khi phân tích code để tìm call-site thực sự (từ call-stack), bởi vì nó là thứ duy nhất ảnh hưởng đến ràng buộc `this`.

**Ghi chú:** Bạn có thể hình dung call-stack trong đầu bằng cách nhìn vào việc gọi hàm theo trình tự giống như ta đã làm với đoạn code ở trên. Nhưng cách này cần sự cần mẫn và dễ sai. Cách khác để biết call-stack là sử dụng công cụ debugger của trình duyệt. Hầu hết các trình duyệt ngày nay đều tích hợp công cụ hỗ trợ lập trình, bao gồm JS debugger. Trong đoạn code trên bạn có thể đặt một breakpoint trong công cụ cho dòng đầu của hàm `foo()`, hoặc đơn giản chèn lệnh `debugger;` vào dòng đầu đó. Khi bạn chạy trang, debugger sẽ tạm dừng tại điểm này và nó sẽ hiện một danh sách hàm được gọi để đi đến dòng đó, tức đó là call-stact. Vì vậy, nếu bạn muốn xác định đúng ràng buộc `this`, sử dụng công cụ hỗ trợ (developer tools) để lấy call-stack, sau đó tìm mục thứ hai từ trên xuống, nó sẽ cho bạn thấy call-site thực.

## Không có gì ngoài quy tắc

Giờ chúng ta chuyển sự chú ý đến việc call-site xác định nơi `this` sẽ trỏ đến suốt quá trình thực thi của một hàm _như thế nào_.

Bạn phải kiểm tra call-site và xác định cái nào trong 4 quy tắc được áp dụng. Trước tiên chúng ta sẽ giải thích tường điều trong 4 quy tắc một cách độc lập, và sau đó ta sẽ minh họa thứ tự ưu tiên của chúng nếu nhiều quy tắc _được_ áp dụng ở call-site.

### Ràng buộc mặc định

Quy tắc đầu tiên ta mà chúng ta xem xét đều có trong hầu hết trường hợp gọi hàm thông thường: Gọi hàm độc lập. Quy tắc này được xem là quy tắc mặc định khi không có quy tắc nào khác áp dụng.

Xem đoạn code sau:

```js
function foo() {
  console.log(this.a);
}

var a = 2;

foo(); // 2
```

Nếu bạn không chú ý thì trước hết phải nhắc rằng biến `var a = 2` được khai báo ở toàn cục, đồng nghĩa với thuộc tính object toàn cục chung một tên. Chúng không sao chép lẫn nhau, mà là riêng biệt, kiểu như hai mặt của một đồng xu.

Tiếp theo, ta thấy khi `foo()` được gọi, `this.a` xử lý biến toàn cục `a`. Vì sao? Vì trong trường hợp này _ràng buộc mặc định_ của `this` áp dụng vào việc gọi hàm và trỏ `this` tại object toàn cục.

Làm sao ta biết rằng quy tắc _ràng buộc mặc định_ được xác định ở đây? Ta kiểm call-site để thấy `foo()` được gọi như thế nào. Trong đoạn code, `foo()` được gọi với một tham chiếu hàm đơn giản, Không có nguyên tắc nào được áp dụng, vì vậy _ràng buộc mặc định_ được áp dụng.

Nếu để chế độ `strict mode`, object toàn cục không được phép thực hiện _ràng buộc mặc định_, vì vậy `this` sẽ là `undefined`.

```js
function foo() {
  "use strict";
  console.log(this.a);
}

var a = 2;

foo(); // TypeError: `this` bị `undefined`
```

Một chi tiết nhỏ quan trọng là: mặc dù toàn bộ các nguyên tắc ràng buộc `this` hoàn toàn dựa trên call-site, object toàn cục **chỉ** đủ điều kiện cho _ràng buộc mặc định_ nếu **nội dung** của `foo()` không chạy ở `strict mode`; trạng thái `strict mode` của call-site của `foo()` không thích hợp.

```js
function foo() {
  console.log(this.a);
}

var a = 2;

(function() {
  "use strict";
  foo(); // 2
})();
```

**Ghi chú:** Cố ý pha trộn `strict mode` và không `strict mode` với nhau sẽ rất lộn xộn. Chương trình của bạn có thể là **Nghiêm ngặt** hoặc **không Nghiêm ngặt**. Nhưng đôi khi bạn nhúng thư viện bên thứ ba vào mà nó có chế độ khác với code của bạn, vì vậy cần phải chú ý những chi tiết tương thích tinh vi này.

### Ngầm ràng buộc

Quy tắc khác là: call-site có một context object, đồng thời được coi là chủ hoặc chứa đối tượng, mặc dù các thuật ngữ thay thế có thể gây hiểu nhầm.

Ví dụ:

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

obj.foo(); // 2
```

Trước tiên, chú ý cách thức `foo()` được khai báo và sau đó được thêm vào như một thuộc tính tham chiếu vào `obj`. Bất kể `foo()` được khai báo ban đầu _trong_ `obj`, hoặc được thêm vào dưới dạng một tham chiếu sau đó (như đoạn code ở trên), trong cả hai trường hợp là **hàm** thực sự "sở hữu" hoặc "chứa" bởi object `obj`.

Tuy nhiên, call-site sử dụng `obj` context để **tham chiếu** hàm, vì vậy bạn có thể nói rằng `obj` "sở hữu" hay "chứa" **tham chiếu hàm** tại thời điểm hàm được gọi.

Dù bạn gọi mẫu này bằng cách nào, tại điểm `foo()` được gọi, nó đến trước bằng một tham chiếu đối tượng đến `obj`. Khi có một context object cho tham chiếm hàm, nguyên tắc _ngầm ràng buộc_ sẽ chỉ ra rằng object _đó_ chính là cái cần được dùng cho ràng buộc `this` của việc gọi hàm.

Vì `obj` là `this` của `foo()`, `this.a` đồng nghĩa với `obj.a`.

Chỉ có cấp cuối cùng của thuộc tính tham chiếu object là mắt xích của call-site. Ví dụ:

```js
function foo() {
  console.log(this.a);
}

var obj2 = {
  a: 42,
  foo: foo
};

var obj1 = {
  a: 2,
  obj2: obj2
};

obj1.obj2.foo(); // 42
```

#### Mất tính ngầm

Một trong những thất vọng thông thường do ràng buộc `this` tạo ra là khi một hàm _bị ràng buộc ngầm_ mất ràng buộc, thường dẫn đến kết quả _ràng buộc mặc định_, hoặc `undefined` tùy vào `strict mode`.

Xem:

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

var bar = obj.foo; // tham chiếu hàm!

var a = "oops, global"; // `a` cũng là thuộc tính của object toàn cục

bar(); // "oops, global"
```

Mặc dù `bar` hiện diện là một tham chiếu đến `obj.foo`, thực ra thì nó chỉ là một tham chiếu khác đến bản thân `foo`. Hơn nữa, call-site mới là quan trọng, `bar()` là call-site theo cách rõ rệt, nên ràng buộc mặc định được áp dụng.

Khi chúng ta xác định truyền một hàm callback thì lại càng tinh tế, rất phổ biến và khác thường xảy ra:

```js
function foo() {
  console.log(this.a);
}

function doFoo(fn) {
  // `fn` chỉ là một tham chiếu khác đến `foo`
  fn(); // <-- call-site!
}

var obj = {
  a: 2,
  foo: foo
};

var a = "oops, global"; // `a` cũng là thuộc tính của object toàn cục

doFoo(obj.foo); // "oops, global"
```

Tham số truyền vào chỉ là phép gán ngầm. Khi ta đang truyền một hàm, nó ngầm gán nên kết quả cuối cùng cũng giống đoạn code trước.

Điều gì xảy ra nếu hàm bạn truyền vào hàm callback lại không phải do bạn viết mà là một hàm định sẵn? Không có gì khác, kết quả y chang.

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

var a = "oops, global"; // `a` cũng là thuộc tính của object toàn cục

setTimeout(obj.foo, 100); // "oops, global"
```

Hãy giả sử rằng hàm định sẵn `setTimeout()` của JavaScript theo cách mộc mạc nhất như sau:

```js
function setTimeout(fn, delay) {
  // chờ `delay` theo milli giây
  fn(); // <-- call-site!
}
```

Như chúng ta vừa thấy, việc hàm callback _mất_ tính ràng buộc `this` là rất bình thường. Nhưng mà `this` có thể làm ta bất ngờ theo cách khác là khi chúng ta đã truyền hàm qua callback để cố ý làm thay đổi `this` khi gọi. Trình xử lý sự kiện trong các thư viện JavaScript phổ biến hơi muốn buộc callback của bạn có một `this` để trỏ đến, chẳng hạn như, thành phần DOM kích hoạt sự kiện. Mặc dù điều này đôi lúc hữu ích, nhưng cũng có lúc nó làm khó chịu. Không may, những công cụ này hiếm khi cho phép bạn chọn khác.

Dù cho `this` thay đổi bất ngờ bằng cách nào, bạn cũng không thực kiểm soát tham chiếu hàm callback được thực thi như thế nào, vậy nên bạn không (chưa) có cách nào điều khiển được call-site để có ràng buộc theo chủ ý. Ta có cách khác để "khắc phục" vấn đề này bằng cách _chỉ định_ `this`.

### Ràng buộc minh bạch

Với _ngầm ràng buộc_ như ta đã thấy, chúng ta phải thay đổi object để bao gồm một tham chiếu vào chính nó đến một hàm, và sử dụng tham chiếu thuộc tính hàm để chuyển (ngầm) ràng buộc `this` vào object.

Nhưng, nếu bạn muốn buộc một thực thi hàm để sử dụng một object riêng biệt cho ràng buộc `this` mà không đặt một thuộc tính tham chiếu vào object?

"Toàn bộ" các hàm có vài tiện ích trong ngôn ngữ của chúng (thông qua `[[Prototype]]` -- càng ngày sẽ càng nhiều hơn) có thể hữu dụng trong trường hợp này. Đặc biệt, hàm có phương thức `call(..)` và `apply(...)`. Về mặt kỹ thuật, môi trường chủ của JavaScript đôi khi cung cấp các hàm đặc biệt mà nó không mang tính chất hàm, nhưng cũng ít thôi. Phần lớn các hàm được cung cấp và chắc chắn là hàm bạn sẽ tạo ra có thể truy cập được `call(..)` và `apply(..)`.

Cách nó hoạt động là lấy một object để dùng cho `this` làm tham số đầu tiên cho nó, và sau đó gọi hàm với `this` cụ thể đó. Vì bạn trực tiếp chỉ ra `this` như bạn muốn nên ta gọi là _ràng buộc minh bạch_.

Xem ví dụ:

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2
};

foo.call(obj); // 2
```

Thực thi `foo` với _ràng buộc minh bạch_ bằng `foo.call(...)` cho phép ta buộc `this` của nó thành `obj`.

Nếu bạn truyền một giá trị nguyên thủy đơn giản (của dạng `string`, `boolean`, hay `number`) như là một ràng buộc `this`, giá trị đó được bao trong dạng object tương ứng (`new String(..)`, `new Boolean(..)`, hoặc `new Number(..)` của nó. Điều này thường được gọi là "đóng hộp".

**Ghi chú:** `call(..)` và `apply(...)` giống nhau trong việc chú trọng vào ràng buộc `this`. Nó có hành vi khác nhau với tham số đưa vào, nhưng không phải là cái ta quan tâm bây giờ.

Không may, bản thân _ràng buộc minh bạch_ vẫn không đưa ra giải pháp nào cho vấn đề đã nêu từ trước về việc một hàm "mất" ràng buộc `this` theo chủ định.

#### Ràng buộc cứng (Hard Binding)

Nhưng thực ra có một mẫu biến thể chung quanh _ràng buộc minh bạch_ có thể làm được:

```js
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2
};

var bar = function() {
  foo.call(obj);
};

bar(); // 2
setTimeout(bar, 100); // 2

// `bar` ràng buộc cứng `this` của `foo` vào `obj`
// vậy nên không thể ghi đè
bar.call(window); // 2
```

Hãy kiểm tra xem biến thể này hoạt động ra sao. Ta tạo một hàm `bar()` gọi thủ công `foo.call(obj)` bên trong nó, do đó nó buộc phải gọi `foo` với ràng buộc `obj` cho `this`. Cho dù sau này bạn gọi hàm `bar` như thế nào, nó vẫn ràng buộc `foo` với `obj`. Cách ràng buộc này vừa rõ ràng vừa mạnh, vậy nên ta gọi là _ràng buộc cứng_.

Cách điển hình nhất để bao một hàm với một _ràng buộc cứng_ tạo ra một đường dẫn (pass-thru) cho bất kỳ đối số nào truyền qua và bất kỳ giá trị nào nhận được:

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}

var obj = {
  a: 2
};

var bar = function() {
  return foo.apply(obj, arguments);
};

var b = bar(3); // 2 3
console.log(b); // 5
```

Cách khác để thể hiện mẫu này là tạo ra một hàm hỗ trợ tái sử dụng:

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}

// hàm hỗ trợ `bind` đơn giản
function bind(fn, obj) {
  return function() {
    return fn.apply(obj, arguments);
  };
}

var obj = {
  a: 2
};

var bar = bind(foo, obj);

var b = bar(3); // 2 3
console.log(b); // 5
```

Vì _ràng buộc cứng_ là một mẫu thông thường, nó được cung cấp bởi tiện ích dựng sẵn từ bản ES5: `Function.prototype.bind`, sử dụng như sau

```js
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}

var obj = {
  a: 2
};

var bar = foo.bind(obj);

var b = bar(3); // 2 3
console.log(b); // 5
```

`bind(..)` trả về một hàm mới đã được hard-code để gọi hàm với `this` context như bạn đã chỉ định.

**Ghi chú:** Theo ES6, hàm ràng buộc cứng được tạo bởi `bind(...)` có một thuộc tính `.name` xuất phát từ _hàm mục tiêu_ gốc. Ví dụ: `bar = foo.bind(..)` sẽ có một `bar.name` giá trị `"bound foo"`, đây là tên gọi hàm sẽ hiển thị trong stack trace.

#### API gọi "Ngữ cảnh" (context)

Nhiều thư viện hàm, và thực tế nhiều hàm dựng sẵn của JavaScript và môi trường chủ cung cấp một tham số tùy chọn, thường được gọi là "ngữ cảnh", nó được thiết kế như là một hàm phụ trợ để bạn không cần thiết phải dùng `bind(...)` để đảm bảo hàm callback sử dụng một `this` cụ thể nào đó.

Ví dụ:

```js
function foo(el) {
  console.log(el, this.id);
}

var obj = {
  id: "awesome"
};

// sử dụng `obj` như `this` của `foo(..)` khi gọi hàm này.
[1, 2, 3].forEach(foo, obj); // 1 awesome  2 awesome  3 awesome
```

Cục bộ thì các hàm này hầu như sử dụng _ràng buộc minh bạch_ thông qua `call(..)` hoặc `apply(..)`, để giúp bạn khỏi rắc rối.

### `new` Binding

Nguyên tắc thứ 4 cuối cùng của ràng buộc `this` cần chúng ta nhắc lại những hiểu lầm thông thường về hàm và onject trong JavaScript.

Trong các ngôn ngữ hướng class, "contructor" là những phương thức đặc biệt gắn liền với class, khi class được tạo với lệnh `new`, contructor của class đó được gọi. Thường thấy như sau:

```js
something = new MyClass(..);
```

JavaScript có lệnh `new`, và mẫu code dùng nó cơ bản trông giống như những ngôn ngữ hướng class; hầu hết các lập trình viên giả định cơ chế JavaScript làm gì đó tương tự. Tuy nhiên, thực ra là cách dùng `new` _không có mối quan hệ_ nào với hàm định hướng class trong JS.

Trước tiên, hãy xác định lại "constructor" trong JavaScript là gì. Trong JS, constructor **chỉ là những hàm** xảy ra để được gọi với lệnh `new` phía trước chúng. Nó không được gắn liền với class hay tạo ra một class. Chúng cũng không phải là hàm đặc biệt nào cả. Về bản chất, nó chỉ là những hàm thông thường, hijacked by the use of `new` in their invocation.

Ví dụ, hàm `Number(..)` (được trích dẫn từ hồi ES5.1) hoạt động như một constructor:

> 15.7.2 The Number Constructor
>
> When Number is called as part of a new expression it is a constructor: it initialises the newly created object.

So, pretty much any ol' function, including the built-in object functions like `Number(..)` (see Chapter 3) can be called with `new` in front of it, and that makes that function call a _constructor call_. This is an important but subtle distinction: there's really no such thing as "constructor functions", but rather construction calls _of_ functions.

Khi một hàm được thực thi bằng `new` trước nó, nếu không phải là contructor call, những điều sau sẽ tự động xảy ra:

1.  một object mới tinh được tạo ra (hay còn gọi là constructed)
2.  _một constructed object mới là `[[Prototype]]`-được liên kết_
3.  the newly constructed object is set as the `this` binding for that function call
4.  unless the function returns its own alternate **object**, the `new`-invoked function call will _automatically_ return the newly constructed object.

Steps 1, 3, and 4 apply to our current discussion. We'll skip over step 2 for now and come back to it in Chapter 5.

Consider this code:

```js
function foo(a) {
  this.a = a;
}

var bar = new foo(2);
console.log(bar.a); // 2
```

By calling `foo(..)` with `new` in front of it, we've constructed a new object and set that new object as the `this` for the call of `foo(..)`. **So `new` is the final way that a function call's `this` can be bound.** We'll call this _new binding_.

## Mọi thứ đều theo trình tự

So, now we've uncovered the 4 rules for binding `this` in function calls. _All_ you need to do is find the call-site and inspect it to see which rule applies. But, what if the call-site has multiple eligible rules? There must be an order of precedence to these rules, and so we will next demonstrate what order to apply the rules.

It should be clear that the _default binding_ is the lowest priority rule of the 4. So we'll just set that one aside.

Which is more precedent, _implicit binding_ or _explicit binding_? Let's test it:

```js
function foo() {
  console.log(this.a);
}

var obj1 = {
  a: 2,
  foo: foo
};

var obj2 = {
  a: 3,
  foo: foo
};

obj1.foo(); // 2
obj2.foo(); // 3

obj1.foo.call(obj2); // 3
obj2.foo.call(obj1); // 2
```

So, _explicit binding_ takes precedence over _implicit binding_, which means you should ask **first** if _explicit binding_ applies before checking for _implicit binding_.

Now, we just need to figure out where _new binding_ fits in the precedence.

```js
function foo(something) {
  this.a = something;
}

var obj1 = {
  foo: foo
};

var obj2 = {};

obj1.foo(2);
console.log(obj1.a); // 2

obj1.foo.call(obj2, 3);
console.log(obj2.a); // 3

var bar = new obj1.foo(4);
console.log(obj1.a); // 2
console.log(bar.a); // 4
```

OK, _new binding_ được ưu tiên hơn _implicit binding_. Nhưng bạn có nghĩ _new binding_ ít ưu tiên hơn _explicit binding_ không?

**Ghi chú:** `new` và `call`/`apply` không thể dùng cùng nhau, so `new foo.call(obj1)` is not allowed, to test _new binding_ directly against _explicit binding_. But we can still use a _hard binding_ to test the precedence of the two rules.

Before we explore that in a code listing, think back to how _hard binding_ physically works, which is that `Function.prototype.bind(..)` creates a new wrapper function that is hard-coded to ignore its own `this` binding (whatever it may be), and use a manual one we provide.

By that reasoning, it would seem obvious to assume that _hard binding_ (which is a form of _explicit binding_) is more precedent than _new binding_, and thus cannot be overridden with `new`.

Let's check:

```js
function foo(something) {
  this.a = something;
}

var obj1 = {};

var bar = foo.bind(obj1);
bar(2);
console.log(obj1.a); // 2

var baz = new bar(3);
console.log(obj1.a); // 2
console.log(baz.a); // 3
```

Whoa! `bar` is hard-bound against `obj1`, but `new bar(3)` did **not** change `obj1.a` to be `3` as we would have expected. Instead, the _hard bound_ (to `obj1`) call to `bar(..)` **_is_** able to be overridden with `new`. Since `new` was applied, we got the newly created object back, which we named `baz`, and we see in fact that `baz.a` has the value `3`.

This should be surprising if you go back to our "fake" bind helper:

```js
function bind(fn, obj) {
  return function() {
    fn.apply(obj, arguments);
  };
}
```

If you reason about how the helper's code works, it does not have a way for a `new` operator call to override the hard-binding to `obj` as we just observed.

But the built-in `Function.prototype.bind(..)` as of ES5 is more sophisticated, quite a bit so in fact. Here is the (slightly reformatted) polyfill provided by the MDN page for `bind(..)`:

```js
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== "function") {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError(
        "Function.prototype.bind - what " +
          "is trying to be bound is not callable"
      );
    }

    var aArgs = Array.prototype.slice.call(arguments, 1),
      fToBind = this,
      fNOP = function() {},
      fBound = function() {
        return fToBind.apply(
          this instanceof fNOP && oThis ? this : oThis,
          aArgs.concat(Array.prototype.slice.call(arguments))
        );
      };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```

**Note:** The `bind(..)` polyfill shown above differs from the built-in `bind(..)` in ES5 with respect to hard-bound functions that will be used with `new` (see below for why that's useful). Because the polyfill cannot create a function without a `.prototype` as the built-in utility does, there's some nuanced indirection to approximate the same behavior. Tread carefully if you plan to use `new` with a hard-bound function and you rely on this polyfill.

The part that's allowing `new` overriding is:

```js
this instanceof fNOP && oThis ? this : oThis;

// ... and:

fNOP.prototype = this.prototype;
fBound.prototype = new fNOP();
```

We won't actually dive into explaining how this trickery works (it's complicated and beyond our scope here), but essentially the utility determines whether or not the hard-bound function has been called with `new` (resulting in a newly constructed object being its `this`), and if so, it uses _that_ newly created `this` rather than the previously specified _hard binding_ for `this`.

Why is `new` being able to override _hard binding_ useful?

The primary reason for this behavior is to create a function (that can be used with `new` for constructing objects) that essentially ignores the `this` _hard binding_ but which presets some or all of the function's arguments. One of the capabilities of `bind(..)` is that any arguments passed after the first `this` binding argument are defaulted as standard arguments to the underlying function (technically called "partial application", which is a subset of "currying").

For example:

```js
function foo(p1, p2) {
  this.val = p1 + p2;
}

// using `null` here because we don't care about
// the `this` hard-binding in this scenario, and
// it will be overridden by the `new` call anyway!
var bar = foo.bind(null, "p1");

var baz = new bar("p2");

baz.val; // p1p2
```

### Mô tả `this`

Now, we can summarize the rules for determining `this` from a function call's call-site, in their order of precedence. Ask these questions in this order, and stop when the first rule applies.

1.  Hàm có được gọi với `new` (**new binding**) không? Nếu có, `this` là constructed object mới.

    `var bar = new foo()`

2.  Is the function called with `call` or `apply` (**explicit binding**), even hidden inside a `bind` _hard binding_? If so, `this` is the explicitly specified object.

    `var bar = foo.call( obj2 )`

3.  Is the function called with a context (**implicit binding**), otherwise known as an owning or containing object? If so, `this` is _that_ context object.

    `var bar = obj1.foo()`

4.  Otherwise, default the `this` (**default binding**). If in `strict mode`, pick `undefined`, otherwise pick the `global` object.

    `var bar = foo()`

That's it. That's _all it takes_ to understand the rules of `this` binding for normal function calls. Well... almost.

## Binding Exceptions

As usual, there are some _exceptions_ to the "rules".

The `this`-binding behavior can in some scenarios be surprising, where you intended a different binding but you end up with binding behavior from the _default binding_ rule (see previous).

### Ignored `this`

If you pass `null` or `undefined` as a `this` binding parameter to `call`, `apply`, or `bind`, those values are effectively ignored, and instead the _default binding_ rule applies to the invocation.

```js
function foo() {
  console.log(this.a);
}

var a = 2;

foo.call(null); // 2
```

Why would you intentionally pass something like `null` for a `this` binding?

It's quite common to use `apply(..)` for spreading out arrays of values as parameters to a function call. Similarly, `bind(..)` can curry parameters (pre-set values), which can be very helpful.

```js
function foo(a, b) {
  console.log("a:" + a + ", b:" + b);
}

// spreading out array as parameters
foo.apply(null, [2, 3]); // a:2, b:3

// currying with `bind(..)`
var bar = foo.bind(null, 2);
bar(3); // a:2, b:3
```

Both these utilities require a `this` binding for the first parameter. If the functions in question don't care about `this`, you need a placeholder value, and `null` might seem like a reasonable choice as shown in this snippet.

**Note:** We don't cover it in this book, but ES6 has the `...` spread operator which will let you syntactically "spread out" an array as parameters without needing `apply(..)`, such as `foo(...[1,2])`, which amounts to `foo(1,2)` -- syntactically avoiding a `this` binding if it's unnecessary. Unfortunately, there's no ES6 syntactic substitute for currying, so the `this` parameter of the `bind(..)` call still needs attention.

However, there's a slight hidden "danger" in always using `null` when you don't care about the `this` binding. If you ever use that against a function call (for instance, a third-party library function that you don't control), and that function _does_ make a `this` reference, the _default binding_ rule means it might inadvertently reference (or worse, mutate!) the `global` object (`window` in the browser).

Obviously, such a pitfall can lead to a variety of _very difficult_ to diagnose/track-down bugs.

#### Safer `this`

Perhaps a somewhat "safer" practice is to pass a specifically set up object for `this` which is guaranteed not to be an object that can create problematic side effects in your program. Borrowing terminology from networking (and the military), we can create a "DMZ" (de-militarized zone) object -- nothing more special than a completely empty, non-delegated (see Chapters 5 and 6) object.

If we always pass a DMZ object for ignored `this` bindings we don't think we need to care about, we're sure any hidden/unexpected usage of `this` will be restricted to the empty object, which insulates our program's `global` object from side-effects.

Since this object is totally empty, I personally like to give it the variable name `ø` (the lowercase mathematical symbol for the empty set). On many keyboards (like US-layout on Mac), this symbol is easily typed with `⌥`+`o` (option+`o`). Some systems also let you set up hotkeys for specific symbols. If you don't like the `ø` symbol, or your keyboard doesn't make that as easy to type, you can of course call it whatever you want.

Whatever you call it, the easiest way to set it up as **totally empty** is `Object.create(null)` (see Chapter 5). `Object.create(null)` is similar to `{ }`, but without the delegation to `Object.prototype`, so it's "more empty" than just `{ }`.

```js
function foo(a, b) {
  console.log("a:" + a + ", b:" + b);
}

// our DMZ empty object
var ø = Object.create(null);

// spreading out array as parameters
foo.apply(ø, [2, 3]); // a:2, b:3

// currying with `bind(..)`
var bar = foo.bind(ø, 2);
bar(3); // a:2, b:3
```

Not only functionally "safer", there's a sort of stylistic benefit to `ø`, in that it semantically conveys "I want the `this` to be empty" a little more clearly than `null` might. But again, name your DMZ object whatever you prefer.

### Indirection

Another thing to be aware of is you can (intentionally or not!) create "indirect references" to functions, and in those cases, when that function reference is invoked, the _default binding_ rule also applies.

One of the most common ways that _indirect references_ occur is from an assignment:

```js
function foo() {
  console.log(this.a);
}

var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };

o.foo(); // 3
(p.foo = o.foo)(); // 2
```

The _result value_ of the assignment expression `p.foo = o.foo` is a reference to just the underlying function object. As such, the effective call-site is just `foo()`, not `p.foo()` or `o.foo()` as you might expect. Per the rules above, the _default binding_ rule applies.

Reminder: regardless of how you get to a function invocation using the _default binding_ rule, the `strict mode` status of the **contents** of the invoked function making the `this` reference -- not the function call-site -- determines the _default binding_ value: either the `global` object if in non-`strict mode` or `undefined` if in `strict mode`.

### Softening Binding

We saw earlier that _hard binding_ was one strategy for preventing a function call falling back to the _default binding_ rule inadvertently, by forcing it to be bound to a specific `this` (unless you use `new` to override it!). The problem is, _hard-binding_ greatly reduces the flexibility of a function, preventing manual `this` override with either the _implicit binding_ or even subsequent _explicit binding_ attempts.

It would be nice if there was a way to provide a different default for _default binding_ (not `global` or `undefined`), while still leaving the function able to be manually `this` bound via _implicit binding_ or _explicit binding_ techniques.

We can construct a so-called _soft binding_ utility which emulates our desired behavior.

```js
if (!Function.prototype.softBind) {
  Function.prototype.softBind = function(obj) {
    var fn = this,
      curried = [].slice.call(arguments, 1),
      bound = function bound() {
        return fn.apply(
          !this ||
          (typeof window !== "undefined" && this === window) ||
          (typeof global !== "undefined" && this === global)
            ? obj
            : this,
          curried.concat.apply(curried, arguments)
        );
      };
    bound.prototype = Object.create(fn.prototype);
    return bound;
  };
}
```

The `softBind(..)` utility provided here works similarly to the built-in ES5 `bind(..)` utility, except with our _soft binding_ behavior. It wraps the specified function in logic that checks the `this` at call-time and if it's `global` or `undefined`, uses a pre-specified alternate _default_ (`obj`). Otherwise the `this` is left untouched. It also provides optional currying (see the `bind(..)` discussion earlier).

Let's demonstrate its usage:

```js
function foo() {
  console.log("name: " + this.name);
}

var obj = { name: "obj" },
  obj2 = { name: "obj2" },
  obj3 = { name: "obj3" };

var fooOBJ = foo.softBind(obj);

fooOBJ(); // name: obj

obj2.foo = foo.softBind(obj);
obj2.foo(); // name: obj2   <---- look!!!

fooOBJ.call(obj3); // name: obj3   <---- look!

setTimeout(obj2.foo, 10); // name: obj   <---- falls back to soft-binding
```

The soft-bound version of the `foo()` function can be manually `this`-bound to `obj2` or `obj3` as shown, but it falls back to `obj` if the _default binding_ would otherwise apply.

## Lexical `this`

Normal functions abide by the 4 rules we just covered. But ES6 introduces a special kind of function that does not use these rules: arrow-function.

Arrow-functions are signified not by the `function` keyword, but by the `=>` so called "fat arrow" operator. Instead of using the four standard `this` rules, arrow-functions adopt the `this` binding from the enclosing (function or global) scope.

Let's illustrate arrow-function lexical scope:

```js
function foo() {
  // return an arrow function
  return a => {
    // `this` here is lexically adopted from `foo()`
    console.log(this.a);
  };
}

var obj1 = {
  a: 2
};

var obj2 = {
  a: 3
};

var bar = foo.call(obj1);
bar.call(obj2); // 2, not 3!
```

The arrow-function created in `foo()` lexically captures whatever `foo()`s `this` is at its call-time. Since `foo()` was `this`-bound to `obj1`, `bar` (a reference to the returned arrow-function) will also be `this`-bound to `obj1`. The lexical binding of an arrow-function cannot be overridden (even with `new`!).

The most common use-case will likely be in the use of callbacks, such as event handlers or timers:

```js
function foo() {
  setTimeout(() => {
    // `this` here is lexically adopted from `foo()`
    console.log(this.a);
  }, 100);
}

var obj = {
  a: 2
};

foo.call(obj); // 2
```

While arrow-functions provide an alternative to using `bind(..)` on a function to ensure its `this`, which can seem attractive, it's important to note that they essentially are disabling the traditional `this` mechanism in favor of more widely-understood lexical scoping. Pre-ES6, we already have a fairly common pattern for doing so, which is basically almost indistinguishable from the spirit of ES6 arrow-functions:

```js
function foo() {
  var self = this; // lexical capture of `this`
  setTimeout(function() {
    console.log(self.a);
  }, 100);
}

var obj = {
  a: 2
};

foo.call(obj); // 2
```

While `self = this` and arrow-functions both seem like good "solutions" to not wanting to use `bind(..)`, they are essentially fleeing from `this` instead of understanding and embracing it.

If you find yourself writing `this`-style code, but most or all the time, you defeat the `this` mechanism with lexical `self = this` or arrow-function "tricks", perhaps you should either:

1.  Use only lexical scope and forget the false pretense of `this`-style code.

2.  Embrace `this`-style mechanisms completely, including using `bind(..)` where necessary, and try to avoid `self = this` and arrow-function "lexical this" tricks.

A program can effectively use both styles of code (lexical and `this`), but inside of the same function, and indeed for the same sorts of look-ups, mixing the two mechanisms is usually asking for harder-to-maintain code, and probably working too hard to be clever.

## Review (TL;DR)

Determining the `this` binding for an executing function requires finding the direct call-site of that function. Once examined, four rules can be applied to the call-site, in _this_ order of precedence:

1.  Called with `new`? Use the newly constructed object.

2.  Called with `call` or `apply` (or `bind`)? Use the specified object.

3.  Called with a context object owning the call? Use that context object.

4.  Default: `undefined` in `strict mode`, global object otherwise.

Be careful of accidental/unintentional invoking of the _default binding_ rule. In cases where you want to "safely" ignore a `this` binding, a "DMZ" object like `ø = Object.create(null)` is a good placeholder value that protects the `global` object from unintended side-effects.

Instead of the four standard binding rules, ES6 arrow-functions use lexical scoping for `this` binding, which means they adopt the `this` binding (whatever it is) from its enclosing function call. They are essentially a syntactic replacement of `self = this` in pre-ES6 coding.
