Câu 1: <a href="https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload">Remote code execution via web shell upload</a>

Problem: 
<ul>
  <li>Chức năng up load ảnh không thực hiện bất kỳ 1 xác thực nào</li>
  <li>Vậy ta có thể upload các file thực thi như PHP</li>
  <li>Flag sẽ nằm trong thư mục: <i>/home/carlos/secret</i></li>
  <li>Account: wiener:peter</li>
</ul>

Solution:
<ul>
  <li>
    Sử dụng Burpsuite để bắt request gửi ảnh, và response render ảnh:<br>
    <ul>
      <li><image src="./images/request.png"></li>
      <li>Màu vàng là gửi ảnh, xanh là render ảnh</li>
    </ul>
  </li>
  <li>Sau đó đưa 2 request đó vào Repeater</li>
  <li>
    Với Request POST ảnh<br>
    <ul>
      <li><image src="./images/sendImage.png"></li>
      <li>Sửa nội dung file ảnh thành <i>"<?php echo file_get_contents('/home/carlos/secret')?>"</i></li>
      <li>Đặt file name là payload.php</li>
      <image src="./images/payload.png">
    </ul>
  </li>
  <li>
    Với Request render ảnh:<br>
    <ul>
      <li><image src="./images/renderImage.png"></li>
      <li>Sửa tên file thành payload.php</li>
      <li>Sau đó ấn send, ta được kết quả: <i>tjemWrTjKZC2mH9Jd1O25l0l3wRtJc2A</i></li>
      <image src="./images/result.png">
    </ul>
  </li>

</ul>

Câu 2: <a href="https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-content-type-restriction-bypass">Web shell upload via Content-Type restriction bypass</a>


Problem: 
<ul>
  <li>Có thể khai thác giống như câu1 để tìm được flag. </li>
  <li>Hoặc dùng  payload <i>system($_GET['cmd'])</i> dùng cat để đọc file</li>
  <li>Flag sẽ nằm trong thư mục: <i>/home/carlos/secret</i></li>
  <li>Account: wiener:peter</li>
</ul>

Solution:

<ul>
  Làm y hệt câu 1, ta có kết quả
  <li><image src="./images/resultCau2.png"></li>
</ul>

Câu 3: <a href="https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-path-traversal">Web shell upload via path traversal</a>


Problem: 
<ul>
  <li>Quan sát response trả ảnh, ta thấy thư mục chứa file tải lên đã nằm trong 1 thư mục <i>files</i>, có nghĩa file tải lên cách thư mục gốc 1 cấp</li>
  <li>Sử dụng Path Traversal, để đi qua thư mục <i>avatar</i></li>
  <li>Thứ tự các bước như các câu trên</li>
  <li><image src="./images/reqCau3.png"></li>
</ul>

Solution:

<ul>
  <li><image src="./images/sendRqCau3.png">Đưa 2 request vào Repeater</image></li>
  <li>
    Với payload của request POST:
    <ul>
      <li><image src="./images/payloadPostCau3.png"></li>
      <li>Nội dung payload: sử dụng hàm đọc nội dung file</li>
      <li>Tên file gửi đi: sử dụng "../" để vượt cấp thư mục <i>avatar</i>, dấu "/" được encode url trước khi gửi</li>
      <li>Sau đó nhần send, và tiếp tục đến với Response GET</li>
    </ul>
  </li>
  <li>
    Với payload của Response GET:
    <ul>
      <li>Đổi tên file thành:</li>
      <li><image src="./images/payloadGetcau3.png"></li>
      <li>Sau đó nhấn send, ta nhận được kết quả:</li>
      <li><image src="./images/resultCau3.png"></li>
    </ul>
  </li>
</ul>


Câu 4: <a href="https://portswigger.net/web-security/file-path-traversal/lab-simple">File path traversal, simple case</a>


Problem: 
<ul>
  <li>Chỉ cần thực thi được file /etc/passwd là solve lab</li>
  <li>Chọn vào 1 bức ảnh, vì đó là Request yêu cầu server tìm và hiển thị bức ảnh đó</li>
  <li>Truy vấn có đối số là filename</li>
</ul>

Solution:

<ul>
  <li>Đưa Request chọn ảnh vào Repeater</li>
    <ul><li><image src="./images/Cau4_1.png"></li></ul>
  <li>Tạo truy vấn có đối số là filename giá trị sau: <i>GET /image?filename=../../../../../etc/passwd</i> </li>
  <li>Sau đó nhấn send, hoàn thành lab</li>
  <li><image src="./images/Cau4_2.png"></li>
</ul>


Câu 5: <a href="https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass">File path traversal, traversal sequences blocked with absolute path bypass</a>

Problem:

<ul>
  <li>Sử dụng Burp Suite để chặn và sửa đổi yêu cầu tìm nạp hình ảnh sản phẩm.</li>
  <li>Sửa đổi tham số tên tệp, đặt cho nó giá trị /etc/passwd.</li>
  <li>Quan sát rằng phản hồi có chứa nội dung của tệp /etc/passwd.</li>
</ul>

Solution:

<ul>
  <li>Bắt Request lúc render ảnh:</li>
  <ul><li><image src="./images/Cau5_1.png"></li></ul>
  <li>Sửa filename=/etc/passwd</li>
  <ul><li><image src="./images/resultCau5.png"></li></ul>
</ul>


Câu 6: <a href="https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively"></a>

Problem:

<ul>
  <li>Sửa đổi tham số tên tệp, cấp cho nó giá trị:....//....//....//etc/passwd</li>
  <li>Quan sát rằng phản hồi có chứa nội dung của tệp /etc/passwd.</li>
</ul>

Solution:

<ul>
  <li>Chặn yêu cầu hiện ảnh và đưa vào Repeater</li>
  <ul><li><image src="./images/Cau6_1.png"></li></ul>
  <li>Sửa tham số của filename có giá trị:....//....//....//etc/passwd</li>
  <li>Sau đó ấn send, thu được kết quả:</li>
  <ul><li><image src="./images/result_cau6.png.png"></li></ul>
</ul>


Câu 7: <a href="https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode">File path traversal, traversal sequences stripped with superfluous URL-decode</a>

ProblemL:
<ul>
  <li>Lab này decode url trước khi gửi đi, nên cần encode url trước khi gửi</li>
  <li>Để giải quyết phòng thí nghiệm, cần truy xuất nội dung của tệp /etc/passwd.</li>
</ul>

Solution:

<ul>
  <li>Chặn yêu cầu hiện ảnh và đưa vào Repeater</li>
  <ul><li><image src="./images/Cau7_1.png"></li></ul>
  <li>encode url tham số của filename có giá trị:..%252f..%252f..%252fetc/passwd</li>
    <ul>
      <li>
      ../../../etc/passwd <br>
      => url: ..%2f..%2f..%2fetc/passwd
      => encode url: ..%252f..%252f..%252fetc%2Fpasswd
      </li>
      <li><image src="./images/resultCau7.png"></li>
    </ul>
  <li>Nhấn Send và hoàn thành</li>
</ul>


Câu 8: <a href="https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path">File path traversal, validation of start of path</a>

