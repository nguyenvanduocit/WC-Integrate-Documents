# Giới thiệu
WC cho phép integrate rất sâu vào hệ thống của họ. Để làm được điều đó, họ tạo ra các inteface, abstract class cho phép ta implement, extend để phụ vụ cho các tác vụ riêng của chúng ta.

**Vấn đề**

Giả như chúng ta sử dụng luôn order và product của WC hoặc chỉ dùng hệ thống của riêng mình thì không có gì để nói, vấn đề của chúng ta là chúng ta không muốn phụ thuộc 100% vào WC, nếu khách hàng không cài WC thì hệ thống của chúng ta vẫn hoạt động được. Và khi họ cài WC, thì chúng ta sử dụng các tính năng được WC cung cấp để mở rộng khả năng trên hệ thống của chúng ta.

**Giải pháp**

Vì vậy các bạn có thể hình dung những gì chúng ta cần làm như sau :

    1. Chúng ta xây dựng hệ thống của riêng mình.
    2. Xây dựng các class trung gian để map các class của chúng ta với class của WC. Một bên của các class này là dữ liệu của chúng ta, bên còn lại là các phương thức phục vụ cho WC
    3. Add phương thức vào các hook để báo cho WC biết về các class trung gian này. 

Sau khi add các hook này, WC sẽ có thể không sử dụng các class order và product mặc định của họ nữa, mà thay vào đó là các class chúng ta mói tạo ra. Điều này cho phép chúng ta tích hợp rất sâu vào hệ thống của họ.

**Scope**

Trong phạm vi của bài viết này, mình sẽ lấy ví dụ từ hệ thống payment của theme ClassifiedEngine ( CE ). Trong hệ thống này có các định nghĩa như sau

    - payment : Một order của khách hàng.
    - Package : Một gói ad mà người dùng muốn mua
    - PaymentGateway : Một cổng thanh toán

# Các class cần kế thừa

Chức năng chính của WC là thanh toán, vì vậy nó có hai đối tượng mà chúng ta quan tâm nhất là order và product.

## WC_Abstract_Order

Class này dùng để thể hiện một Order. Nó chứa các thuộc tính của một order kèm theo các phương thức xử lý order.
Class này tương ứng với class payment trong hệ thống của CE.

## WC_Product

Class này thể hiện một product. Chứa dữ liệu và các phương thức xử lý product. Class này tương ứng với Package trong CE