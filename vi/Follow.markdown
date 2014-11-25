#Follow

BEGIN

Để bắt đầu thanh toán một $order, ta gọi ``$gateway->process_payment($order)``. Trong đó ``$gateway`` là đối tượng của một class payment thuộc WC.

Khởi nguyên của dòng lưu chuyển bắt đầu từ ``gateway->process_payment($order)``. trong đó, $order là đối tượng của class ET_Order

Lúc này WC sẽ gọi tới phương thức ``WC_Order_Factory->get_order()`` để tạo một đối tượng có class kế thừa class ``WC_Abstract_Order``.

Trong phương thức ``WC_Order_Factory->get_order()``, WC sẽ gọi tới filter ``woocommerce_order_class`` để lấy tên của class, phù hợp với tên class của order.

Chúng ta add phương thức vào filter ``woocommerce_order_class`` để trả về tên của class`` ET_WC_Order``.

Sau đó wc sẽ tạo new ``ET_WC_Order($order)``. Từ nay, WC chính thức sửa dụng order theo class ``ET_WC_Order`` của chúng ta.

Trong Order, có phương thức ``get_product_from_item()``, Phương thức này sẽ trả về một đối townjg của class Product. Nó sẽ gọi đến ``WC()->product_factory->get_product( $the_product, $args )``. Phương thức này cũng hoạt động giống phương thức ``get_order()``. Chúng ta cũng add hook để trả về cho nó tên class chúng ta vừa extend để integrate : ``ET_WC_Package``

Khi quá trình thanh toán xảy ra giữa server của chúng ta và những xác nhận của server payment. WC sẽ gọi đến các filter khác để xác định currency, ... và các hook xác định post status. Chúng ta cũng đã hook và xử lý.

Khi một giai đoạn trong quá trình thanh toán kết thúc, WC sẽ gọi tới phương thức ``update_status()`` trong đối tượng Order để cập nhật lại trạng thái của order.

Chúng ta không quan tâm WC đã làm những thao tác thành toán, xác minh, ... như thế nào. Tất cả những gì chúng ta cần biết, là trạng thái được truyền vào phương thức ``update_status()`` là gì. Dựa vào trạng thái đó ta sẽ update trạng thái của những thứ khác trong hệ thống của chúng ta

END