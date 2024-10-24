# mid-project-140260583-1
mid-project-140260583-1 created by GitHub Classroom

**Thành viên:** 
  - Đặng Minh Anh
  - Vũ Hoàng Phi
  - Nguyễn Thuỳ Dung

**I.Topic**
(*)P2P-File-Sharing
Phần mềm chia sẻ tệp P2P bằng Java tương tự BitTorrent

Thông điệp bắt tay (Handshake message) :
  - Thông điệp bắt tay bao gồm ba phần: tiêu đề bắt tay, các bit 0 và ID của peer. Độ dài của thông điệp bắt tay là 32 byte. Tiêu đề bắt tay là một chuỗi ký tự 18 byte ‘P2PFILESHARINGPROJ’, tiếp theo là 10 byte các bit 0, và sau đó là 4 byte ID của peer, đây là dạng số nguyên đại diện cho ID của peer.
  - Thông điệp thực tế (Actual messages)
Sau khi bắt tay, mỗi peer có thể gửi một luồng thông điệp thực tế. Một thông điệp thực tế bao gồm trường độ dài thông điệp 4 byte, trường loại thông điệp 1 byte, và một phần tải thông điệp với kích thước biến đổi. Trường độ dài thông điệp 4 byte chỉ định độ dài của thông điệp tính theo byte, không bao gồm độ dài của chính trường độ dài thông điệp. Trường loại thông điệp 1 byte chỉ định loại của thông điệp.

Các loại thông điệp (Message Types)
  - choke, unchoke, interested, not interested . Các thông điệp ‘choke’, ‘unchoke’, ‘interested’ và ‘not interested’ không có tải dữ liệu.

have
  - Thông điệp ‘have’ có phần tải chứa một trường chỉ số mảnh 4 byte.

bitfield
  - Thông điệp ‘bitfield’ chỉ được gửi như thông điệp đầu tiên ngay sau khi bắt tay xong khi kết nối được thiết lập. Thông điệp ‘bitfield’ có một trường tải là bitfield. Mỗi bit trong phần tải bitfield đại diện cho việc peer có sở hữu mảnh tương ứng hay không. Byte đầu tiên của bitfield tương ứng với các chỉ số mảnh từ 0 đến 7, từ bit cao đến bit thấp. Byte tiếp theo tương ứng với các chỉ số mảnh từ 8 đến 15, v.v. Các bit thừa ở cuối được đặt thành 0. Các peer chưa sở hữu bất cứ mảnh nào có thể bỏ qua thông điệp ‘bitfield’.

request
  - Thông điệp ‘request’ có phần tải bao gồm một trường chỉ số mảnh 4 byte. Lưu ý rằng tải của thông điệp ‘request’ được định nghĩa ở đây khác với BitTorrent. Chúng tôi không chia nhỏ một mảnh thành các mảnh nhỏ hơn.

piece
  - Thông điệp ‘piece’ có phần tải bao gồm một trường chỉ số mảnh 4 byte và nội dung của mảnh.

**II.Công nghệ**

Công nghệ của phần mềm chia sẻ tệp P2P (Peer-to-Peer)  thường bao gồm các thành phần và giao thức sau:

  - Mạng P2P (Peer-to-Peer)
  - Java Socket : Sockets cung cấp cơ chế giao tiếp thông qua mạng, cho phép gửi và nhận dữ liệu giữa các thiết bị.
  - Giao thức truyền thông TCP/IP: TCP (Transmission Control Protocol) bảo đảm rằng dữ liệu được truyền một cách tin cậy và theo đúng thứ tự, rất quan trọng trong chia sẻ file lớn.
  - Message Protocol (Giao thức thông điệp): Sau khi thiết lập kết nối, các peer trao đổi thông tin qua một giao thức thông điệp.
  - Quản lý file chia sẻ: File được chia nhỏ thành các mảnh (pieces), và mỗi peer có thể tải xuống hoặc tải lên từng mảnh của file đó.
  - Băm dữ liệu (Data Hashing): Để đảm bảo tính toàn vẹn của dữ liệu, các mảnh file thường được băm bằng các hàm băm (hashing algorithms) như SHA-1 hoặc MD5

**III.Các bước hoạt động**

  (*) Tổng quan quá trình giao tiếp giữa client và server:
  - Thiết lập kết nối TCP giữa client và server.
  - Gửi và nhận thông điệp handshake để xác nhận danh tính của peer.
  - Gửi thông điệp bitfield để chia sẻ trạng thái file của mình.
  - Client gửi request để yêu cầu mảnh file từ server.
  - Server gửi piece chứa dữ liệu của mảnh được yêu cầu.
  - Client ghép file lại từ các mảnh đã nhận được từ nhiều server/peer.
  - Thông điệp choke/unchoke được sử dụng để điều chỉnh kết nối nếu cần.
Hệ thống này hoạt động liên tục cho đến khi mọi peer đều có đủ các mảnh của file hoàn chỉnh.
