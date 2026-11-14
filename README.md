# Vietnamese Agent Skill

Bộ hướng dẫn giúp agent giao tiếp và làm việc tự nhiên bằng tiếng Việt: hiểu không dấu, lỗi gõ, viết tắt, phương ngữ và câu trộn Anh–Việt; phản hồi rõ ràng, đúng ngữ cảnh và giữ chính xác nội dung kỹ thuật.

## Mục tiêu

Người dùng có thể viết “ê fix giùm tui cái login, bấm hoài ko vô”. Agent cần nhận ra yêu cầu hỗ trợ sửa lỗi đăng nhập, tận dụng ngữ cảnh có sẵn và tìm nguyên nhân bằng bằng chứng. Agent không nên bắt người dùng viết lại câu cho đúng chính tả hoặc tự kết luận lỗi nằm ở token.

Skill là hướng dẫn hành vi, không phải mô hình ngôn ngữ mới. Hiệu quả phụ thuộc vào năng lực nền của agent, cách nền tảng nạp skill và công cụ được cấp.

## Cấu trúc

File skill chính: [skills/vietnamese-agent/SKILL.md](skills/vietnamese-agent/SKILL.md).

README này trình bày cách tích hợp, phạm vi và các tình huống kiểm tra thủ công. Skill có YAML frontmatter với name và description để nền tảng hỗ trợ SKILL.md nhận diện.

## Cài đặt

### Nền tảng hỗ trợ SKILL.md

Chép toàn bộ thư mục skills/vietnamese-agent vào thư mục skills mà nền tảng của bạn quy định. Giữ tên file SKILL.md. Đường dẫn cài đặt và cách kích hoạt khác nhau giữa các nền tảng; làm theo tài liệu của agent đang dùng.

Nếu nền tảng cho phép chọn skill thủ công, bật vietnamese-agent. Nếu nền tảng tự chọn theo mô tả, thử yêu cầu tiếng Việt trong bảng kiểm tra bên dưới và xác nhận skill đã được nạp qua giao diện hoặc log mà nền tảng cung cấp.

### Agent chỉ hỗ trợ custom instructions

Dán phần nội dung sau YAML frontmatter của SKILL.md vào custom instructions, developer instructions hoặc phần cấu hình hướng dẫn tương đương. Giữ các quy định về quyền hạn và công cụ của agent chủ quản.

Chỉ chép file vào một thư mục bất kỳ hoặc gửi đường dẫn trong chat không bảo đảm agent sẽ tự nạp skill. Kiểm tra cơ chế nạp của nền tảng trước khi đánh giá.

Skill không yêu cầu thư viện, API key hay dịch vụ bên ngoài.

## Những hành vi được hướng dẫn

### Hiểu ý định trước khi sửa câu chữ

Chấp nhận văn bản có dấu, không dấu, thiếu dấu, lỗi gõ và khẩu ngữ phổ biến. Dùng hội thoại và tài liệu liên quan để giải nghĩa; không áp dụng bảng thay từ máy móc. Khi có nhiều cách hiểu dẫn đến kết quả khác nhau, hỏi một câu ngắn, có lựa chọn cụ thể.

### Tiếng Việt rõ ràng và đúng sắc thái

Mặc định xưng “mình – bạn”, viết tự nhiên, tôn trọng và dễ đọc. Với tài liệu, báo cáo hoặc giao tiếp công việc, dùng giọng văn phù hợp đối tượng đọc. Không bắt chước quá mức tiếng lóng, vùng miền hoặc lời thô tục.

### Bảo toàn dữ liệu và cú pháp

Giữ nguyên code, tên biến, API, package, URL, đường dẫn, mã lỗi và chuỗi được trích dẫn. Giải thích bằng tiếng Việt; chỉ sửa nội dung kỹ thuật khi nhiệm vụ yêu cầu và có căn cứ. Không đoán tài khoản, số tiền, ngày tháng, môi trường hoặc file cần xóa.

### Làm việc có bằng chứng

Với yêu cầu thao tác, dùng công cụ trong quyền hạn đã được cấp, kiểm chứng kết quả và báo rõ phần đã làm. Nếu chỉ có khả năng tư vấn, đưa hướng dẫn hoặc bản nháp và nói rõ giới hạn; không khẳng định đã chạy lệnh hay sửa file khi chưa làm.

## Tình huống kiểm tra thủ công

| Đầu vào | Hành vi mong đợi |
| --- | --- |
| ê fix giùm tui cái login, bấm hoài ko vô | Hiểu lỗi đăng nhập; xem thông tin có sẵn, tìm bằng chứng; không tự quy lỗi cho token. |
| build lai app roi test nha | Hiểu yêu cầu build và test; dùng quy trình của dự án nếu có công cụ, báo kết quả thật. |
| ktra API /v1/orders sao trả 500 | Giữ nguyên /v1/orders và 500; kiểm tra log hoặc hỏi dữ liệu còn thiếu. |
| xóa bản cũ đi | Nếu chưa rõ bản nào và việc xóa có rủi ro, xác định đúng đối tượng trước khi xóa. |
| sao kê tháng 3 của tk đó | Không diễn giải “sao” thành “tại sao”; không tự chọn tài khoản hay năm còn thiếu. |
| hẹn 03/04 lúc 8h | Khi cần tạo lịch, làm rõ ngày/tháng, sáng/tối và múi giờ nếu ngữ cảnh chưa đủ. |
| dịch “permission denied” cho người mới | Giải thích “không đủ quyền truy cập”; giữ nguyên chuỗi gốc khi cần đối chiếu lỗi. |
| viết email xin dời lịch họp gửi khách hàng | Viết bản nháp lịch sự; không tự gửi email nếu chưa được yêu cầu. |
| trả lời bằng tiếng Anh từ đây | Chuyển sang tiếng Anh theo yêu cầu rõ ràng của người dùng. |
| đưa output đúng JSON, không thêm chữ | Trả đúng định dạng yêu cầu, không thêm lời dẫn tiếng Việt ngoài JSON. |

Chạy cùng tình huống trên agent trước và sau khi nạp skill. Kiểm tra mức độ đúng ý định, số câu hỏi không cần thiết, độ chính xác của dữ liệu kỹ thuật, tính tự nhiên và sự trung thực về hành động đã làm. Bảng trên là tiêu chí đề xuất, chưa phải kết quả benchmark hoặc bộ test tự động đã chạy.

## Đóng góp

Mở issue hoặc pull request kèm câu đầu vào, ngữ cảnh tối thiểu, hành vi mong đợi và ví dụ phản hồi. Ưu tiên tình huống thật đã ẩn thông tin cá nhân; giải thích vì sao quy tắc mới hữu ích và không làm sai các trường hợp khác.

## Giấy phép

Repository hiện chưa có LICENSE. Không mặc định dự án được cấp phép MIT hoặc một giấy phép mã nguồn mở khác.
