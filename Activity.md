# 1. Activity là gì?

**Activity** đại diện cho một chức năng của app, là một giao diện màn hình, nơi user tương tác trực tiếp với app. Một ứng dụng có thể có một hoặc nhiều Activity.

Một Activity từ khi được gọi đến khi kết thúc sẽ có những ***trạng thái (state)*** khác nhau.

# 2. Vòng đời Activity

## 2.1. Giới thiệu

Trong quá trình sử dụng app, user có thể di chuyển sự tương tác giữa các Activity hoặc *thoát app*. Trong Android, Activity class đã cung cấp các hàm **callback** để chúng ta có thể nắm bắt được ***trạng thái*** của Activity mỗi khi có thay đổi.

## 2.2. Các trạng thái

Các trạng thái của Activity:

|Trạng thái|Giải thích|
|--|--|
|`onCreate()`|Đây là hàm bắt buộc phải được **implement**, được gọi đầu tiên một lần duy nhất khi hệ thống khởi tạo Activity, `callback` này chỉ được gọi lại khi hệ thống xóa Activity đi hoặc khi chuyển trạng thái giữa 2 chế độ màn hình (Landscape và Portrait). <br> Vì được gọi đầu tiên và một lần duy nhất nên phương thức `setContentView()` sẽ được gọi ở hàm này, tùy vào logic của Activity, một số logic cũng sẽ được cho vào hàm này.|
|`onStart()`|Hàm này được gọi khi giao diện được hiển thị nhưng chưa tương tác được với user.|
|`onResume()`|Khi hệ thống gọi đến callback này, có nghĩa là các View, Component,… của Activity đã **khởi tạo xong**, đây là trạng thái mà app có thể tương tác với user.<br>Activity sẽ ở trạng thái này cho đến khi có một Activity hoặc app khác được gọi chẳng hạn như có cuộc gọi đến, hoặc tắt màn hình điện thoại.|
|`onPause()`|Callback này sẽ được gọi tới đầu tiên và ngay lập tức khi user thoát khỏi Activity *(Activity chưa bị destroy)*. Tức là Activity không còn nằm ở foreground nữa nhưng vẫn có thể nhìn thấy đc Activity trên màn hình.<br>`onPause()` thường được sử dụng khi Activity đang **không được focus** bởi user, một số tính năng trong đó sẽ được tạm dừng hoặc duy trì ở một mức độ nào đó để chờ cho user quay lại.<br>Một số lý do mà bạn nên sử dụng `onPause()`:<br>-  Một số sự kiện làm cho Activity **bị gián đoạn**. Đây là trường hợp xảy ra phổ biến nhất.<br>- Từ Android 7.0 (API 24) trở lên, Android có chức năng **đa nhiệm (Multi Window)**. Khi chạy trong chế độ này ở bất kì thời điểm nào cũng chỉ có 1 app được focus bởi user, hệ thống sẽ tạm dừng tất cả các app khác.<br>- Khi bị một Activity khác **đè lên** (ví dụ như một dialog). Activity hiện tại sẽ không tương tác với user, tuy nhiên vẫn được nhìn thấy trên màn hình.|
|`onStop()`|Khi Activity không còn được nhìn thấy trên màn hình nữa, Activity sẽ rơi vào trạng thái `onStop()`, lúc này bất kỳ chức năng nào trong Activity cũng có thể bị dừng lại, qua đó app sẽ **giải phóng tài nguyên** không cần thiết khi mà nó không còn hữu dụng với user. Tuy nhiên, trong một số trường hợp `onStop()` cũng được dùng để **lưu trữ dữ liệu**.|
|`onRestart()`|Phương thức callback này gọi khi activity đã ***stopped***, gọi trước khi bắt đầu ***start*** lại Activity.|
|`onDestroy()`|Callback này được gọi khi user **thoát hoàn toàn** khỏi Activity(nhấn nút **back** hoặc gọi tới hàm `finish()` của Activity).|