# IPv6:
### Prerequisite: 
- Kiến thức cơ bản về IP, IPv4
## Thông tin chung về địa chỉ IPv6
- Dùng 128 bit thay vì 32 bit như IPv4. -> Lượng địa chỉ khả dụng là lớn hơn rất nhiều so với IPv4
- Biểu diễn bằng 8 bộ số thập lục phân 4 chữ số ngăn cách bởi dấu ":". Các chữ số 0 ở đầu mỗi bộ số có thể được bỏ đi (ABF thay vì 0ABF hay 0012 -> 12). Một chuỗi liên tiếp các bộ số chỉ chứa 0 (0000 hay 0) có thể được rút gọn thành "::" (Chỉ dùng 1 lần trong một địa chỉ IPv6 để tránh gây nhầm lẫn).
  - Ví dụ: 2001:0DB8:0000:0010:FEDC:0000:0000:3210 -> 2001:DB8:0:10:FEDC:0:0:3210 -> 2001:DB8:0:10:FEDC::3210 
- Địa chỉ IPv6 được chia thành 2 cụm 64 bit, 64 bit đầu tiên là phần địa chỉ mạng, 64 bit tiếp theo là phần interface ID 
- 64 bit mạng có thể được chia nhỏ thành các phần đầu tố nhỏ hơn để phục vụ việc routing
- CIDR notation giống như IPv4
 
## Một số loại địa chỉ IPv6 đặc biệt và các prefix của chúng:
  - Địa chỉ không xác định (Dùng cho các host mới tham gia mạng chưa có địa chỉ IP) ::/128 
  - Địa chỉ loopback (localhost) ::1/128
  - Địa chỉ multicast (Dùng khi muốn gửi các packet cùng một lúc đến nhiều host) ff00::/8
    - ff02::1/128 gửi đến tất cả node trong cùng mạng nội bộ
    - ff02::2/128 gửi đến tất cả router trong cùng mạng 
    - Các địa chỉ khác có thể xem thêm ở https://www.iana.org/assignments/ipv6-multicast-addresses/ipv6-multicast-addresses.xhtml	
  - Địa chỉ unicast (dùng khi muốn gửi đến 1 host duy nhất)
    - Địa chỉ link-local unicast (Dùng khi muốn kết nối đến những host thuộc cùng một phần mạng không cần thông qua router) fe80::/10
    - Địa chỉ unique-local unicast (Giống private address trong IPv4) fc00::/7
    - Địa chỉ unique-global unicast (Địa chỉ host) 2000::/3 (Các địa chỉ đã được IANA assign chủ yếu nằm trong dải 2001::/16 nên các IPv6 thường gặp sẽ hay bắt đầu bằng 2001, nhưng ngoài ra cũng có các dải khác cho được cấp có thể được tìm thấy ở đây https://www.cidr.eu/en/ipv6#C-c67e73)
  
## Ưu điểm của địa chỉ IPv6
- Vì lượng địa chỉ khả dụng của IPv6 là lớn hơn rất nhiều so với IPv4, IPv6 có một số ưu điểm:
  - Assign IP có thể được thực hiện trực tiếp bởi host với SLAAC mà không cần thông qua DHCP server(Host có thể chỉnh sửa mã MAC để tạo thành môt interface ID 64 bit và nối vào sau phần subnet prefix của mạng mình kết nối). Host vẫn có thể dùng DHCP nếu trong mạng có một DHCP server và host muốn dùng DHCP.
  - Không cần dùng NAT (lượng địa chỉ IP đủ lớn để có thể thoải mái cho mỗi thiết bị một IP duy nhất). Việc này đảm bảo rằng source IP và destination IP trong IPv6 header không cần phải được thay đổi trong quá trình vận chuyển
  - Một host có thể cùng lúc có nhiều địa chỉ IP khác nhau dùng cho nhiều mục đích khác nhau để làm cho việc trao đổi thông tin hiệu quả hơn (Ví dụ vừa có unique-local IPv6 để giao tiếp với các máy trong mạng nội bộ, vừa có một unique-global address để giao tiếp với internet, và các máy được khuyến nghị dùng địa chỉ IP phù hợp nhỏ nhất để thực hiện giao tiếp)
  - Phần subnet của địa chỉ IPv6 có thể được dùng để tối ưu việc routing bằng cách phân cáp đầu tố. 
- Một số khuyến nghị của IETF để đơn giản hóa việc quản trị mạng:
  - Mỗi subnet luôn là một block /64
  - Mỗi endsite sẽ được assign ít nhất một block từ /48 đến /56 (từ 256 đến 65536 subnet)
  - Các ISP thường được assign các block /32

## Một số cải tiến khác về mặt protocol
- IPsec là bắt buộc trong IPv6
- Header của IPv6 sẽ không có check sum mà sẽ phụ thuộc vào kiểm tra check sum ở tầng trên và tầng dưới trong mô hình IP (Tầng link và tầng transport). Vì vậy, trong IPv6, UDP cũng bắt buộc phải có checksum (checksum cho UDP là optional ở v4)
- IPv6 sẽ bỏ phần OPTION trong header và thay bằng header extension nằm trong payload. Bằng cách này, header của gói tin IPv6 sẽ có độ dài cố định.
- IPv6 không cho phép các router fragment các gói tin trong trường hợp gói tin vượt mức MTU mà thay vào đó, router sẽ trả lỗi Packet too big về cho source host. Như vậy, việc fragmenting trong IPv6 sẽ chỉ diễn ra ở điểm đầu trong quá trình truyền tin.
	
