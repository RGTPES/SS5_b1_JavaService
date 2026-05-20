1. REST dùng danh từ số nhiều cho URL, không dùng động từ

Theo nguyên tắc REST, đường dẫn API nên dùng danh từ số nhiều để biểu diễn tài nguyên, không nên dùng động từ. Lý do là hành động đã được thể hiện qua các phương thức HTTP như GET, POST, PUT, DELETE.

Ví dụ đúng:

GET /students
POST /students

Ví dụ sai:

GET /getStudents
POST /createStudent

Trong ví dụ đúng, /students là tài nguyên sinh viên. Còn việc lấy danh sách hay thêm mới sinh viên sẽ được quyết định bởi method GET hoặc POST.

2. Ví dụ đúng và sai cho API quản lý sinh viên

Hai ví dụ đúng:

GET /students

Dùng để lấy danh sách tất cả sinh viên.

GET /students/1

Dùng để lấy thông tin sinh viên có id là 1.

Hai ví dụ sai:

GET /getAllStudents

Sai vì URL có chứa động từ get.

POST /createStudent

Sai vì URL có chứa động từ create, trong khi REST nên dùng danh từ là /students.

3. Các phương thức HTTP
   Method	Mục đích chính	Có body không?
   GET	Dùng để lấy dữ liệu từ server	Thường không có body
   POST	Dùng để tạo mới một tài nguyên	Thường có body
   PUT	Dùng để cập nhật toàn bộ tài nguyên	Thường có body
   PATCH	Dùng để cập nhật một phần tài nguyên	Thường có body
   DELETE	Dùng để xóa tài nguyên	Thường không có body

Nói ngắn gọn, GET dùng để đọc dữ liệu, POST dùng để thêm mới, PUT dùng để sửa toàn bộ, PATCH dùng để sửa một phần, còn DELETE dùng để xóa dữ liệu.

4. Phân biệt PUT và PATCH

Giả sử ban đầu có một sản phẩm như sau:

{
"id": 1,
"name": "Bút bi",
"price": 5000
}

Nếu dùng:

PUT /products/1

với body:

{
"name": "Bút mực"
}

thì vì PUT là cập nhật toàn bộ tài nguyên, body gửi lên bị thiếu trường price. Khi đó server có thể hiểu rằng sản phẩm mới chỉ còn trường name, làm cho price bị mất hoặc thành null. Trong thực tế, cách xử lý tốt hơn là server nên trả về lỗi 400 Bad Request vì dữ liệu cập nhật chưa đầy đủ.

Nếu dùng:

PATCH /products/1

với body:

{
"name": "Bút mực"
}

thì kết quả sẽ là:

{
"id": 1,
"name": "Bút mực",
"price": 5000
}

Sự khác biệt là PUT dùng để thay thế toàn bộ tài nguyên, nên client cần gửi đầy đủ dữ liệu. Còn PATCH chỉ cập nhật những trường được gửi lên, các trường còn lại vẫn được giữ nguyên.

5. Mã trạng thái HTTP
   Mã	Ý nghĩa	Tình huống ví dụ
   200	OK, yêu cầu thành công	Gọi GET /students/1 và server trả về thông tin sinh viên
   201	Created, tạo mới thành công	Gọi POST /students để thêm sinh viên mới thành công
   204	No Content, xử lý thành công nhưng không trả về dữ liệu	Gọi DELETE /students/1 và xóa thành công
   400	Bad Request, dữ liệu gửi lên không hợp lệ	Thêm sinh viên nhưng thiếu tên hoặc nhập sai định dạng
   404	Not Found, không tìm thấy tài nguyên	Gọi GET /students/999 nhưng không có sinh viên nào có id là 999
   500	Internal Server Error, lỗi bên trong server	Server bị lỗi code hoặc lỗi kết nối database

Tóm lại, status code giúp client biết được kết quả của request. Nếu thành công có thể trả về 200, 201 hoặc 204; nếu lỗi từ phía client thường là 400 hoặc 404; còn lỗi phía server thường là 500.