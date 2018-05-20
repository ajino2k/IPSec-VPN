<img src="https://github.com/ajino2k/IPSec-VPN/blob/master/techspacekh_l2lvpn_1.png" />
# IPSec-VPN
- IPSec – Internet Protocol security: là giao thức cung cấp những kỹ thuật để bảo vệ dữ liệu, sao cho dữ liệu được truyền đi an toàn từ nơi này sang nơi khác. IPSec VPN là sự kết hợp để tạo ra một mạng riêng an toàn phục vụ cho việc truyền dữ liệu bảo mật


- IPSec hoạt động ở lớp Network, nó không phụ thuộc vào lớp Data-Link như các giao thức dùng trong VPN khác như L2TP, PPTP.-

- IPSEC hoạt động lớp network

- IPSec hỗ trợ nhiều thuật toán dùng để để đảm bảo tính toàn vẹn dữ liệu, tính nhất quán,tính bí mật và xác thực của truyền dữ liệu trên một hạ tầng mạng công cộng . Những kỹ thuật mà IPSec dùng cung cấp 4 tính năng phổ biến sau:

- Tính bảo mật dữ liệu – Data confidentiality
- Tính toàn vẹn dữ liệu – Data Integrity
- Tính chứng thực nguồn dữ liệu – Data origin authentication
- Tính tránh trùng lặp gói tin – Anti-replay

# Các giao thức của IPSec

- Để trao đổi và thỏa thuận các thông số để tạo nên một môi trường bảo mật giữa 2 đầu cuối, IPSec dùng 3 giao thức:

- IKE (Internet Key Exchange)
- ESP (Encapsulation Security Payload)
- AH (Authentication Header)

+ IKE là giao thức thực hiện quá trình trao đổi khóa và thỏa thuận các thông số bảo mật với nhau như: mã hóa thế nào, mã hóa bằng thuật toán gì, bau lâu trao đổi khóa 1 lần. Sau khi trao đổi xong thì sẽ có được một “hợp đồng” giữa 2 đầu cuối, khi đó IPSec SA (Security Association) được tạo ra.

SA là những thông số bảo mật đã được thỏa thuận thành công, các thông số SA này sẽ được lưu trong cơ sở dữ liệu của SA

Trong quá trình trao đổi khóa thì IKE dùng thuật toán mã hóa đối xứng, những khóa này sẽ được thay đổi theo thời gian. Đây là đặc tính rất hay của IKE, giúp hạn chế trình trạng bẻ khóa của các attacker.

IKE còn dùng 2 giao thức khác để chứng thực đầu cuối và tạo khóa: ISAKMP (Internet Security Association and Key Management Protocol) và Oakley.

- ISAKMP: là giao thức thực hiện việc thiết lập, thỏa thuận và quản lý chính sách bảo mật SA
- Oakley: là giao thức làm nhiệm vụ chứng thực khóa, bản chất là dùng thuật toán Diffie-Hellman để trao đổi khóa bí mật thông qua môi trường chưa bảo mật.
Giao thức IKE dùng UDP port 500. 
Các giai đoạn (phase) hoạt động của IKE

Giai đoạn hoạt động của IKE cũng được xem tương tự như là quá trình bắt tay trong TCP/IP. Quá trình hoạt động của IKE được chia ra làm 2 phase chính: Phase 1 và Phase 2, cả hai phase này nhằm thiết lập kênh truyền an toàn giữa 2 điểm. Ngoài phase 1 và phase 2 còn có phase 1,5 tùy chọn.



# IKE phase 1:

- Đây là giai đoạn bắt buộc phải có. Pha này thực hiện việc chứng thực và thỏa thuận các thông số bảo mật, nhằm cung cấp một kênh truyền bảo mật giữa hai đầu cuối. Các thông số sau khi đồng ý giữa 2 bên gọi là SA, SA trong pha này gọi là ISAKMP SA hay IKE SA.

Pha này sử dụng một trong 2 mode để thiết lập SA: Main mode và Aggressive mode.

Các thông số bảo mật bắt buộc phải thỏa thuận trong phase 1 này là:

- Thuật toán mã hóa: DES, 3DES, AES
- Thuật toán hash: MD5, SHA
- Phương pháp chứng thực: Preshare-key, RSA
- Nhóm khóa Diffie-Hellman (version của Diffie-Hellman)

Main mode sử dụng 6 message để trao đổi thỏa thuận các thông số với nhau:
- 2 message đầu dùng để thỏa thuận các thộng số của chính sách bảo mật
- 2 message tiếp theo trao đổi khóa Diffire-Hellman
- 2 message cuối cùng thực hiện chứng thực giữa các thiết bị

Aggressive mode: sử dụng 3 message
- Message đầu tiên gồm các thông số của chính sách bảo mật, khóa Diffie-Hellman
- Message thứ 2 sẽ phản hồi lại thông số của chính sách bảo mật được chấp nhận, khóa được chấp nhận và chứng thực bên nhận
- Message cuối cùng sẽ chứng thực bên vửa gửi.


Đây là phase không bắt buộc (optional). Phase 1 cung cấp cơ chế chứng thực giữa 2 đầu cuối tạo nên một kênh truyền bảo mật.

Phase 1.5 sử dụng giao thức Extended Authentication (Xauth). Phase này thường sử dụng trong Remote Access VPN

# IKE phase 2
Đây là phase bắt buộc, đến phase này thì thiết bị đầu cuối đã có đầy đủ các thông số cần thiết cho kênh truyền an toàn. Qua trình thỏa thuận các thông số ở phase 2 là để thiết lập IPSec SA dựa trên những thông số của phase 1. Quick mode là phương thức được sử dụng trong phase 2.

Các thông số mà Quick mode thỏa thuận trong phase 2:

- Giao thức IPSec: ESP hoặc AH
- IPSec mode: Tunnel hoặc transport
- IPSec SA lifetime: dùng để thỏa thuận lại IPSec SA sau một khoảng thời gian mặc định hoặc được chỉ định.
- Trao đổi khóa Diffie-Hellman

IPSec SA của phase 2 hoàn toàn khác với IKE SA ở phase 1, IKE SA chứa các thông số để tạo nên kênh truyền bảo mật, còn IPSec SA chứa các thông số để đóng gói dữ liệu theo ESP hay AH, hoạt động theo tunnel mode hay transport mode 

Các chức năng khác của IKE giúp cho IKE hoạt động tối ưu hơn bao gồm: 

- Dead peer detection ( DPD ) and Cisco IOS keepalives là những chức năng bộ đếm thời gian. Nghĩa là sau khi 2 thiết bị đã tạo được VPN IPsec với nhau rồi thì nó sẽ thường xuyên gửi cho nhau gói keepalives để kiểm tra tình trạng của đối tác. Mục đích chính để phát hiện hỏng hóc của các thiết bị. Thông thường các gói keepalives sẽ gửi mỗi 10s

- Hỗ trợ chức năng NAT-Traversal: Chức năng này có ý nghĩa là nếu trên đường truyền từ A tới B nếu có những thiết bị NAT or PAT đứng giữa thì lúc này IPSec nếu hoạt động ở chế độ tunel mode và enable chức năng NAT- Trasersal sẽ vẫn chuyển gói tin đi được bình thường. 

Lưu ý : Chức năng NAT-T bắt đầu được Cisco hỗ trợ tự phiên bản IOS Release 122.2(13)T 

Nguyên nhân tại sao phải hỗ trợ chức năng NAT-T thì các packet mới tiếp tục đi được? 

Khi thực hiện quá trình mã hóa bằng ESP thì lúc này các source IP, port và destination IP, port đều đã được mã hóa và nằm gọn tron ESP Header. Như vậy khi tất cả các thông tin IP và Port bị mã hóa thì kênh truyền IPSec không thể diễn ra quá trình NAT. 
# Cấu hình IPSEC VPN
#### Connect IPSEC VNM TO ABC Centos 6
ACL Mo port 500 udp va 4500 tcp/udp va protocol 50,51 den server VNM va nguoc lai

#### Connect IPSEC VNM TO ABC Centos 6
 ```yum install libreswan.x86_64```

#### Config IPSEC connect VNM
### 1 . vim /etc/ipsec.conf
config	| note
------------ | -------------
conn VNM-VPN-ABC                  |           ## name connect
left=xxx.xxx.xx.x                 |           ## IP public ABC
leftsubnet=xx.xxx.xx.x/32         |           ## IP private ABC
right=202.172.4.16                |           ## IP Public VNM
rightid=10.32.254.168             |           ## Range IP VNM
rightsubnet=10.8.2.101/32         |   	     ## IP private VNM
authby=secret                     |           ## Giu nguyen
auto=start                        |           ##
pfs=yes                           |           ##   
type=tunnel                       |           ##
priority=1                        |           ##
ike=aes128-sha1;modp1024          |  	     ##
esp=3des;modp1024                 |           ##

### 2 . vim /etc/ipsec.secrets

```xxx.xxx.x.xx %any: PSK "VNM092ABC" | ## IP public VNM + key VNM gui```

### 3 . Route private VNM ve private ABC
```ip r a 10.8.2.101 via xx.x.x.1 dev em2```

#### 4 . Command show status IPSEC
```/etc/init.d/ipsec start ```
</br>
ipsec auto status 
</br>
IPsec connections: loaded 1, active 1
