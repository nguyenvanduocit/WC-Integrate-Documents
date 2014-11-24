# Giới thiệu
WC cho phép integrate rất sâu vào hệ thống của họ. Để làm được điều đó, họ tạo ra các inteface, abstract class cho phép ta implement, extend để phụ vụ cho các tác vụ riêng của chúng ta.

Chúng ta có thể hình dung folow chúng ta cần làm như sau:

- Kế thừa các abstract class cần thiết từ WC.
- Add hook để cung cấp tên các class vừa tạo.

# Các class cần kế thừa

Chức năng chính của WC là thanh toán, vì vậy nó có hai đối tượng mà chúng ta quan tâm nhất là order và product.

## WC_Abstract_Order
Class này dùng để thể hiện một Order. Nó chứa dữ liệu order kèm theo các phương thức xử lý order. Chúng ta cần extend class này và code cho
## WC_Product
Class này thể hiện một product. Chứa dữ liệu và các phương thức xử lý product.

