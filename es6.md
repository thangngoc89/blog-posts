---
layout: Post
title: "Tổng quan ES6 qua 350 điểm"
date: 2016-02-21 20:20:00
tags: [js]
draft: true
translate:
  url: "https://ponyfoo.com/articles/es6"
  author: "Pony Foo"
---

@[toc]

# Giới thiệu

- ES6, còn được biết với tên Harmony, `es-next`, ES2015 là bản mô tả mới nhất của Javascript
- Bản mô tả ES6 hoàn thành vào tháng 6 năm 2015 (vì vậy mới gọi là ES2015)
- Các bản mô tả tiếp theo của Javascript sẽ được đặt tên theo dạng ES[YYYY], ví dụ ES2016 cho ES7
  - Sẽ cập nhật mỗi năm, tính năng nào không hoàn thành sẽ bị hoãn lại tới năm tiếp theo
  - Vì ES6 được bắt đầu trước khi cách gọi mới này ra đời nên chúng ta vẫn thường gọi là ES6
  - Bắt đầu từ ES2016 (ES7), chúng ta nên bắt đầu dùng dạng ES[YYYY] để gọi tên phiên bản Javascript.
  - Lý do quan trọng nhất khi đặt tên như vậy là để gây áp lực, và buộc các trình duyệt phải thêm tính năng mới vào nhanh hơn.

# Công cụ

- Để làm việc với ES6 ngay từ bây giờ, bạn cần transpiler **Javascript-to-Javascript**
- Transpiler có chức năng
  - Biên dịch code của bạn từ phiên bản mới nhất (của Javascript) về phiên bản cũ hơn
  - Khi mà trình duyệt đã tương thích với phiên bản mới, chúng ta sẽ biên dịch từ ES2016 và ES2017 về ES6, ... và tiếp tục như vậy.
  - Hỗ trợ source map
  - Bạn có thể tự tin viết code ES6 và dùng trong thực tế ngay từ bây giờ (tuy nhiên, trình duyệt sẽ nhận được code ES5)

- Babel (một transpiler) có một tính năng mà không có đối thủ nào cạnh tranh được: xuất ra code mà bạn **có thể đọc được**

- Dùng `babel` để biên dịch ES6 xuống ES5
- Dùng `babelify` kết hợp với `babel` cùng với Gulp, Grunt, hoặc npm run trong khi build.
- Dùng Node.js v4.x.x sẽ có native support cho ES6 (:+1: cho V8)
- Dùng `babel-node` với bất kì phiên bản node nào, sẽ biên dịch module xuống ES5
- Babel có một kho plugins và hệ sinh thái phong phú.

# Destructuring

- `var {foo} = pony` tương đượng với `var foo = pony.foo`
- `var {foo: baz} = pony` tương đương với `var baz = pony.foo`
- Bạn có thể cung cấp giá trị mặc định , `var {foo='bar'} = baz` cho ra `foo: 'bar'` nếu `baz.foo === undefined`
- Bạn có thể lấy bao nhiêu giá trị tùy thích `var {foo, bar: baz} = {foo: 0, bar: 1}` cho ra `foo: 0` và `baz: 1`
- Bạn có thể lấy giá trị lồng vào nhau. `var {foo: {bar}} = { foo: { bar: 'baz' } }` cho ra  `bar: 'baz'`
- Bạn cũng có thể đặt tên cho nó. `var {foo: {bar: deep}} = { foo: { bar: 'baz' } }` cho ra `deep: 'baz'`
- Thuộc tính không tình thấy sẽ cho ra `undefined` như bình thường. Ví dụ `var {foo} = {}`
- Giá trị lồng vào nhau không tồn tại sẽ cho ra lỗi, ví dụ: `var {foo: {bar}} = {}`
- Mảng cũng tương tự, `[a, b] = [0, 1]` cho ra `a: 0` và `b: 1`
- Bạn có thể bỏ qua các giá trị trong mảng, `[a, , b] = [0, 1, 2]`, cho ra `a: 0` và `b: 2`
- Bạn có thể thay đổi vị trí mà không cần biết trung gian `[a, b] = [b, a]`
- Bạn cũng có thể destruct các tham số của hàm
  - Gán giá trị mặc định cho hàm `function foo (bar=2) {}`
  - Giá trị mặc định cũng có thể là object `function foo (bar={ a: 1, b: 2 }) {}`
  - Chúng ta có thể destruct biến `bar` ở trên hoàn toàn thế này `function foo ({ a=1, b=2 }) {}`
  - Hoặc có thể đặt giá trị mặt định là một object rỗng nếu như không có bất kì tham số nào `function foo ({ a=1, b=2 } = {}) {}`

# Spread Operator and Rest Parameters

- Rest Parameters (tạm dịch là các tham số còn lại) sẽ `arguments` tốt hơn
  - Bạn có thể định nghĩa nó trong hàm thế này `function foo (...everything) {}`
  - `everything` là một mảng gồm tất cả tham số được gửi đến hàm `foo`
  - Bạn có thể đặt tên cho vài tham số trước `...everything` thế này `function foo (bar, ...rest) {}`
  - Các tham số đã được đặt tên sẽ được loại ra khỏi `...rest`
  - `...rest` phải là tham số cuối cùng trong danh sách
