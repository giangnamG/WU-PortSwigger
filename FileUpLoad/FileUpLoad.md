Câu 1: <a href="https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload">Remote code execution via web shell upload</a>

Problem: Theo đề ta có: 
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
      <li><image src="./request.png"></li>
      <li>Màu vàng là gửi ảnh, xanh là render ảnh</li>
    </ul>
  </li>
  <li>Sau đó đưa 2 request đó vào Repeater</li>
  <li>
    Với Request POST ảnh<br>
    <ul>
      <li><image src="./sendImage.png"></li>
      <li>Sửa nội dung file ảnh thành <i><?php echo file_get_contents('/home/carlos/secret')?></i></li>
      <li>Đặt file name là payload.php</li>
    </ul>
  </li>
  <li>
    Với Request render ảnh:<br>
    <ul>
      <li><image src="./renderImage.png"></li>
      <li>Sửa tên file thành payload.php</li>
      <li>Sau đó ấn send, ta được kết quả: <i style="color:red">tjemWrTjKZC2mH9Jd1O25l0l3wRtJc2A</i></li>
    </ul>
  </li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  
</ul>


