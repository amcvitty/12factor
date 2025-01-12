## VIII. Đồng bộ hoá

### Mở rộng theo chiều ngang thông qua mô hình tiến trình

Bất kỳ chương trình máy tính nào, khi vận hành, đều được đại diện bởi một hoặc nhiều tiến trình. Ứng dụng web có thể sử dụng bất kỳ dạng nào của tiến trình vận hành. Ví dụ, tiến trình PHP vận hành như là tiến trình con của Apache, chạy như một tiến trình nền dựa vào số lượng yêu cầu được gửi đến. Ứng dụng Java, tiếp cận theo cách ngược lại, với JVM cung cấp một tiến trình nền tảng lớn (a massive uberprocess) với lượng lớn các tài nguyên (CPU và bộ nhớ) được cấp phát ngay từ đầu, việc đồng bộ hoá được quản lý bên trong thông qua các luồng. Trong cả hai trường hợp, các tiến trình vận hành chỉ hiện hữu rất ít đối với các nhà phát triển của ứng dụng.
developers of the app.

![Mở rộng được biểu diễn thông qua các tiến trình vận hành, Scale is expressed as running processes, hình thái của tải được biểu diễn như là kiểu tiến trình.](/images/process-types.png)

**Trong ứng dụng áp dụng mười hai hệ số, các tiến trình là thành phần nền tảng đầu tiên.** Các tiến trình trong ứng dụng áp dụng mười hai hệ số sử dụng các tín hiệu rõ rệt từ [các mô hình tiến trình của unix cho các dịch vụ chạy nền](http://adam.heroku.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/). Sử dụng mô hinh này, các lập trình viên có thể thiết kế ứng dụng của họ để điều khiển các công việc khác nhau bằng cách gán mỗi loại tương ứng với một _kiểu tiến trình_. Ví dụ, yêu cầu HTTP có thể điều khiển bởi tiến trình web, và các ứng dụng chạy gầm tốn nhiều thời gian có thể được quản lý bởi các tiến trình làm việc.

Điều này không loại bỏ các tiến trình các nhân khỏi việc quản lý chính các kênh nội bộ của tiến trình đó, thông qua các luồng bên trong máy áo đang thực thi, hoặc mô hình bất đồng bộ/xự kiện trong các công cụ như là [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/), hoặc [Node.js](http://nodejs.org/). Nhưng một máy ảo (VM) riêng biệt chỉ có thể mở rộng (mở rộng theo chiều dọc), do đó ứng dụng cần có thể mở rộng thành các tiến trình chạy trên nhiều máy chủ vật lý.

Mô hình tiến trình thực sự nổi trội khi mà việc mở rộng tài nguyên trở nên phổ biển. [Việc không chia sẻ, phân hoạch tự nhiên của các tiến trình của ứng dụng áp dụng mười hai hệ số](./processes) có nghĩa việc thêm các tiến trình đồng bộ hơn là đơn giản và tin cậy. Danh sách các kiểu tiến tình và số lượng tiến trình mỗi loại được biết đến như là _công thức tiến trình_.

Tiến trình của ứng dụng mười hai hệ số [không bao giờ chạy như là dịch vụ nền](https://dustin.sallings.org/2010/02/28/running-processes.html) hoặc ghi ra tệp tin PID. Thay vào đó, tin tưởng vào hệ thống quản lý tiến trình của hệ điều hành (như là [Upstart](http://upstart.ubuntu.com/), hoặc hệ thống quản lý tiến trình phân tán trên nền tảng đám mấy, hoặc công cụ như là [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) trong quá trình phát triển) để quản lý [luồng xuất ra](./logs), phản hồi các tiến trình bị lỗi, và quản lý yêu cầu khởi động lại hoặc ngưng hoạt động của người dùng.
