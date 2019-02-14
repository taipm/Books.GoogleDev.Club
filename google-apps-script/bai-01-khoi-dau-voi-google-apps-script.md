---
description: >-
  Bài này được biên tập với mục tiêu giúp các bạn (lần đầu) làm quen với Google
  Apps Script (GAS) nhanh chóng nắm bắt một số kỹ năng quan trọng khi làm việc
  với GAS,
---

# Bài 01: Khởi đầu với Google Apps Script

## CHUẨN BỊ \| LÀM VIỆC VỚI GOOGLE APPS SCRIPT \(GAS\)

{% hint style="info" %}
GAS: Kể từ đây về sau tôi sẽ sử dụng GAS thay cho cụm từ Google Apps Script.
{% endhint %}

### Bạn phải có

* Tài khoản Google hợp lệ, tôi nghĩ rằng hầu hết mọi người đều đã có và cũng mặc định luôn rằng các bạn có đủ kỹ năng cần thiết để làm việc và hiểu được cách thức làm việc cơ bản của các sản phẩm liên quan trong bộ công cụ Google Drives.
* Có khả năng truy cập [http://script.google.com](http://script.google.com) bằng tài khoản của chính bạn, tôi cũng mặc định rằng bạn có thể hiểu một số thứ cơ bản trong quá trình làm việc với GAS tại đây, chẳng hạn như:
  * Tạo script
  * Thực thi script
  * Hiểu được và thực hành được Trigger
  * ...
* Javascript: Thực sự thì nó rất cần thiết, tuy không phải là điều gì đó quá khó khăn, tôi nghĩ bạn cần nên tìm hiểu thêm một chút để làm quen với cú pháp, ngôn ngữ, hiểu được cách thức thực thi các hàm trong Javascript và từ đó có thể sử dụng GAS tốt hơn.

## MỘT SỐ VÍ DỤ THỰC HÀNH

#### Ví dụ 01: Gửi email từ GAS

1. Đăng nhập vào [http://script.google.com](http://script.google.com) bằng tài khoản của bạn.
2. Tạo mới script, đặt tên nó là SendEmail.
3. Viết mã nguồn

```javascript
function sendEmails(){
  MailApp.sendEmail(recipient, subject, body);
}
```

Thực thi code và kiểm tra email xem điều gì xảy ra.

