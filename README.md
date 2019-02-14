---
description: >-
  Đây là bài viết đầu tiên trong chuỗi chuyên đề "ChatBot & Google Apps Script",
  tôi lấy nền tảng Facebook Messenger làm ví dụ minh họa do có tính phổ biến
  cao.
---

# ChatBot \| Bài 01: Khởi tạo, thử nghiệm Facebook Messenger ChatBot với Google Apps Script

## CHUẨN BỊ

$$ax^2 + bx + c = 0$$

Bạn cần phải chuẩn bị một số bước sau đây trước khi tiến hành đọc tiếp bài viết:

1\) Tài khoản Google, với tài khoản \(miễn phí\) này bạn đã có thể sử dụng Google Drives, Google Apps Script \(để lập trình\).

2\) Tài khoản Facebook \(để sử dụng\) và Facebook Developer \(để phát triển ứng dụng trên Facebook\).

3\) Tài khoản GitHub \(để sau này bạn lưu trữ và quản lý mã nguồn của mình\).

Ngoài ra, bạn còn nên chuẩn bị cho mình một số nguyên tắc làm việc và kỹ năng, chẳng hạn:

* Tạo một thư mục "Projects/ChatBots/&lt;project name&gt;" - để chứa các tài nguyên liên quan đến từng dự án \(chẳng hạn như ChatBots trong chuỗi chuyên đề này\).
* Tạo một thư mục Projects/Google Apps Script/Libraries - để lưu trữ các thư viện mà bạn đã viết/sưu tầm \(bằng Google Apps Script\) để sau này tiện sử dụng.
* Cũng nên tìm hiểu nhanh một số thao tác cơ bản và thử nghiệm vài chương trình nho nhỏ trên Google Apps Script để hoàn thiện những kỹ năng cần thiết khác.

## FACEBOOK DEVELOPER \| MỘT SỐ KỸ NĂNG QUAN TRỌNG

Trước tiên bạn cần phải có một vài kỹ năng nhất định khi làm việc với Facebook Developer. Tôi điểm qua một số kỹ năng thường gặp và bạn có thể tìm hiểu thêm trên Internet.

1. Tạo mới một ứng dụng trên facebook.
2. Khai báo các thông tin cho ứng dụng trên facebook.
3. Tích hợp các sản phẩm của facebook vào ứng dụng mà bạn vừa tạo.
4. Hiểu thêm về Webhook:
5. * Địa chỉ Webhook.
   * Mã xác thực.
   * Kiểm tra kết nối đến Webhook
6. Cách đăng ký App Review để ứng dụng có thể phổ biến công khai đến mọi người.





**ĐĂNG KÝ Facebook Messenger ChatBot**  
  


**STEP 01: KHỞI TẠO CHATBOT, ĐĂNG KÝ, ĐẢM BẢO THÔNG LUỒNG ỨNG DỤNG**

* **ChatBot.gs: Là mã nguồn của ChatBot, tại đây có mấy hàm chính doGet, doPost, để nhận/gửi thông tin từ ChatBot đến người dùng.**

**Bước 01: Xuất bản ChatBot**

* **Bạn vào Publish, sau đó xuất bản ứng dụng dưới dạng Web apps.**
* **Lấy link của ứng dụng**
* **Đăng ký WebHook trên Facebook Developer \| iRestaurant**
* **Nhớ mã xác thực**

![](https://lh3.googleusercontent.com/sWbPznKiYyLKQheZuflTWzgv3b-UzB7MSh9Bw4ph1HL8DCCyXlpATfG036azpC1_Y-9PEVqBLFIRjkHxteeoEhUBUaMV6Hlek3vbi0hS8r_VDSMp5NHBgJ4xevE0vUa6xwe6Qf5m)

![](https://lh3.googleusercontent.com/6Z0sGUGhbSljaRhuwyVRSKMMyYUGMkuya6qJz8pGq0utOmLTNp22FdYgCwBVD1C_tVMnrptNNtGYnVIlydj_ZcZr53NLNAFSCC_M3CVuvCo99d_sTg6yD3eLGH8GLn3Bz0T9Xipe)

**LOGS**

**Logs là điều cơ bản nhất mà mọi lập trình viên đều phải biết, với Google Apps Script, việc ghi Logs có khá nhiều cách, chẳng hạn trực tiếp thì dùng Logger \(đã được tích hợp sẵn\), tôi thì theo thói quen hay dùng 02 dạng logs.**

* **Logger: Dùng để log nhanh trong quá trình test các hàm.**
* **Logs.log\(\)/clear\(\): Là hàm tự xây dựng, tổ chức Logs dưới dạng file Google Sheets để thuận tiện tra cứu và xử lý sau này. Dưới đây là mã nguồn của Logs.gs cơ bản mà tôi hay sử dụng.**

| **function log\(subject, body\){   var file = SpreadsheetApp.openById\(logFileId\);   file.getSheetByName\("Logs"\).appendRow\(\[subject, body\]\);   //MailApp.sendEmail\("taipm@workcard.vn", "ChatBot", "ChatBot"\)   if\(DEBUG\){     //SpreadsheetApp.openById\(SPREADSHEET\_ID\).appendRow\(\[subject, body\]\)   } }  function test\_log\(\){   clearLog\(\);   log\("Hello","Hello"\) }  function clearLog\(\){   var file = SpreadsheetApp.openById\(logFileId\).getSheetByName\("Logs"\).clear\(\); }** |
| :--- |


**Theo đó, tôi cấu hình tham số logFileId tại Global.gs - file lưu trữ tất cả các tham số mang tính chất global cho ứng dụng.**  


**Lưu ý:**

* **Việc ghi/đọc log đôi khi cũng tốn thời gian, cho nên chúng ta nên log ở những chỗ quan trọng, chỗ nào linh tinh không cần thiết thì thôi.**
* **Nên thiết lập trigger để tự động xóa log, cái này thì tùy bạn.**

**ChatBot.gs**

* **Thực ra thì một dự án ChatBot sẽ có khá nhiều file mã nguồn \(tùy cách bạn tổ chức\), tôi thì theo thói quen sẽ viết tất cả các hàm quan trọng của ChatBot vào file này, ở đây sẽ có mấy hàm chính:**
  * **doGet**
  * **doPost**



