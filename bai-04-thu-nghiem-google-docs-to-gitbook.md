---
description: $$ax^2 + bx + c = 0$$
---

# Bài 04: Thử nghiệm Google Docs to GitBook



**CHUYÊN ĐỀ \| KỸ THUẬT LẬP TRÌNH CHATBOT**

**PHẦN 01**

**FACEBOOK MESSENGER CHATBOT & GOOGLE APPS SCRIPT**

**MỞ ĐẦU**

**Tôi dành thời gian rảnh rỗi khoảng 01 tuần \(bắt đầu từ hôm nay\), dựa trên cảm hứng nguyên tắc 20% thời gian dành cho các dự án cá nhân của nhân viên tại Google để xây dựng chuyên đề “Kỹ thuật lập trình ChatBot - phần Facebook Messenger ChatBot” với “Google Apps Script”.**

**Tài liệu này cùng với các dự án và mã nguồn minh họa sẽ được chia sẻ miễn phí đến cộng đồng nhằm trợ giúp cho các bạn mới làm quen với Google Apps Script và Lập trình Facebook Messenger ChatBot có thêm tư liệu tham khảo \(bằng tiếng Việt\).**  


**Ghi chú: Tôi cũng dự định mở một lớp học cơ bản về lập trình Facebook Messenger ChatBot với Google Apps Script. Nếu các bạn yêu thích có thể đăng ký tại \[\] hoặc chat trực tiếp với tôi qua Page GoogleDev.Club để có thể trao đổi thêm.**  


**MỘT SỐ LƯU Ý KHI PHÁT TRIỂN CHATBOT**

* **Tài khoản Facebook Developer: Bạn cần phải có tài khoản Facebook Developer. Cái này bạn có thể tìm hiểu nhanh thông qua Google.**
* **Tài khoản Google và biết cách sử dụng Google Drive \(Google Docs, Google Sheets, …\)**
* **Biết cách sử dụng Google Apps Script một cách cơ bản: Tạo code, chạy thử, debug, log và xem log.**

**Một vài lưu ý này là cơ bản, các bạn có thể biết/hoặc chưa biết nhưng hoàn toàn có thể tìm hiểu nhanh trên Internet, tài nguyên tham khảo có khá nhiều trên mạng.**  


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

| **`function log(subject, body){   var file = SpreadsheetApp.openById(logFileId);   file.getSheetByName("Logs").appendRow([subject, body]);   //MailApp.sendEmail("taipm@workcard.vn", "ChatBot", "ChatBot")   if(DEBUG){     //SpreadsheetApp.openById(SPREADSHEET_ID).appendRow([subject, body])   } }  function test_log(){   clearLog();   log("Hello","Hello") }  function clearLog(){   var file = SpreadsheetApp.openById(logFileId).getSheetByName("Logs").clear(); }`** |
| :--- |


**Theo đó, tôi cấu hình tham số logFileId tại Global.gs - file lưu trữ tất cả các tham số mang tính chất global cho ứng dụng.**  


**Lưu ý:**

* **Việc ghi/đọc log đôi khi cũng tốn thời gian, cho nên chúng ta nên log ở những chỗ quan trọng, chỗ nào linh tinh không cần thiết thì thôi.**
* **Nên thiết lập trigger để tự động xóa log, cái này thì tùy bạn.**

**ChatBot.gs**

* **Thực ra thì một dự án ChatBot sẽ có khá nhiều file mã nguồn \(tùy cách bạn tổ chức\), tôi thì theo thói quen sẽ viết tất cả các hàm quan trọng của ChatBot vào file này, ở đây sẽ có mấy hàm chính:**
  * **doGet**
  * **doPost**
  * **CallSendApi**
  * **…**
* **Ngoài ra, các hàm bổ trợ cho ChatBot thì có thể tổ chức gom nhóm và phân loại ở các file phù hợp.**

**DATABASE**  
  


**USERS**

**Phần này khá quan trọng, ứng dụng nào cũng cần phải có người dùng và sheets\[“Users”\] được xây dựng để quản lý người dùng của ChatBot.**  
  


**MESSAGES \| CONVERSATIONS**

**Quá trình trao đổi giữa người dùng và ChatBot nên được lưu lại, có thể sau này còn tổ chức thành các đoạn hội thoại. Chúng ta cũng có thể sử dụng TextMining để sau này khai thác nội dung các cuộc trò chuyện.**  
  
  


**RESTAURANT CHATBOT LÀ GÌ**

**Nói thì hơi khó, nhưng đại loại với nhà hàng như Hoa Sen Restaurant \(tại Cambodia\) mà tôi đang quản lý thì có mấy thứ ChatBot có thể làm được như sau:**

* **Nhắc việc \[Reminders\]: Tôi sẽ đưa vào đây một danh sách các công việc mang tính chất định kỳ, tới thời gian thì nó sẽ nhắc, cho phép người dùng có các tùy chọn để trả lời \[xong/chưa xong\].**

**BOTCOMMAND \| NHẬN DẠNG CÁC CÂU LỆNH CỦA BOT**  
  
  


**CÁC CÂU LỆNH THƯỜNG DÙNG HÀNG NGÀY**  


| **STT** | **Lệnh** | **Ý nghĩa** |  |  |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **\#Today** | **Liệt kê những diễn biến trong ngày** |  |  |
| **2** | **\#Summary** | **Tổng kết tình hình thu/chi/tiền mặt của quán** |  |  |
| **3** | **\#Help \| \#Hi** | **Liệt kê danh sách command mà chatbot có thể thực hiện.** |  |  |
| **4** | **\#Buy** |  |  |  |
| **5** | **\#Cash** |  |  |  |
| **6** | **@\[keyword\]** | **Sẽ trả ra thông tin của người dùng liên quan.** |  |  |
| **7** | **\#Menu** | **Giới thiệu thực đơn cho khách hàng.** |  |  |
| **8** |  |  |  |  |
| **9** | **\#Sell** |  |  |  |

**TÍCH HỢP XỬ LÝ NGÔN NGỮ TỰ NHIÊN \| WIT.AI & MESSENEGER**

**Thực sự hơi khó cho quá trình tương tác với ChatBot nếu như không có cơ chế xử lý và nhận dạng ngôn ngữ tự nhiên, khá may mắn là Messenger ChatBot cho phép chúng ta tích hợp NPL của Wit.ai vào và quá trình sử dụng không khó.**

**Tham khảo:** [**https://developers.facebook.com/docs/messenger-platform/built-in-nlp**](https://developers.facebook.com/docs/messenger-platform/built-in-nlp)  
****

**Nhận dạng tin nhắn đến**  
  


**Tùy chỉnh nhận dạng ngôn ngữ**

**“Ngày mai, mua 100 con cá chép, giá 10$/con”.**  
  


**XỬ LÝ NGOẠI LỆ, KIỆN TOÀN VÀ ĐẢM BẢO TÍNH NHẤT QUÁN CỦA CHATBOT**

* **Thông thường thì người dùng Facebook Messenger có khá nhiều thiết bị, chẳng hạn như tôi có tới 03 thiết bị: Ipad, Điện thoại, Máy tính. Chúng ta cũng nên lưu ý rằng, việc đăng nhập ứng dụng trên nhiều thiết bị khác nhau \(vẫn chung UserId\) sẽ khiến cho quá trình chat bị lặp đi lặp lại, và cần phải có cách xử lý vấn đề này nếu bạn không muốn ChatBot bị treo và hiểu lầm quá trình giao tiếp với người dùng. Giải pháp cho vấn đề này như sau:**
  * **Tạo ra**

**PostBack \| TRẢ LỜI TIN NHẮN**

**ChatBot gửi cho bạn một công việc, nếu bạn hoàn thành rồi thì đánh dấu là hoàn thành, chưa hoàn thành thì đánh dấu chưa hoàn thành. Như vậy là có phản hồi giữa tin nhắn của Bot và người dùng. Ta sử dụng PostBack để làm việc này.**  
  
  
  
  


**HƯỚNG DẪN SỬ DỤNG**  


**Hàng ngày, nhân viên/người dùng phải thực hiện mấy việc sau:**

* **Nhập giá nguyên liệu**

**Nhập giá nguyên liệu đầu vào**

* **\[Tên nguyên liệu\] \[Số lượng\] \[Số tiền mua \(mã tiền tệ\)\]**

**Ví dụ:**

* **Gà, 23$**
* **Bánh mì, 2, 1000R**

**QUẢN LÝ THU / CHI HÀNG NGÀY**

**Quản lý các khoản chi**

* **Hàng ngày ta đều có ghi lại các khoản chi vào trong sổ, tuy nhiên ghi vào sổ thì cuối tháng hoặc định kỳ ta mới cộng sổ được. Để đảm bảo “thời gian thực” hơn thì chúng ta nên đưa ngay vào ChatBot, trong đó xác định cú pháp như sau:**

**\[Chi\]**  


**Ví dụ:**

* **Chi, xăng, 5$, @Thu.**
* **Chi, Gà, 02 con, 20$, @Thơ.**

**DATETIME \| DateTimeHelper**

**Giao tiếp với ChatBot chủ yếu là message, do đó việc gõ làm sao cho thuận lợi là điều quan trọng, bởi thế cần có một vài bổ trợ “nhỏ - có ích” cho người dùng là điều cần thiết. DateTimeHelper giúp ta giải quyết một số vấn đề này.**  


**“Hôm nay, anh @Hùng mượn 1000$”**

**Nếu phân tích ra sẽ là “13/02/219, anh \[Hùng\] \[mượn\] \[1000$\]”**

* **\[Thời gian\] \[danh từ\] \[động từ\] \[danh từ chỉ số lượng\]**

