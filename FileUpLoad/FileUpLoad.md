Câu 1: <a href="https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload">Remote code execution via web shell upload</a>

Solution: 
<ul>Theo đề ta có:
  <li>Chức năng up load ảnh không thực hiện bất kỳ 1 xác thực nào</li>
  <li>Vậy ta có thể upload các file thực thi như php</li>
  <li>Flag sẽ nằm trong thư mục: <i>/home/carlos/secret</i></li>
  <li>Account: wiener:peter</li>
</ul>
