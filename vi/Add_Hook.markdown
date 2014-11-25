# Add hook để khai báo các class

**wc_woocommerce_order_class** : Thông báo cho WC biết về tên của class mới, thay thế class order để xử lý product có kiểu là payment_package

 ```php
 $this->add_filter('woocommerce_order_class', 'wc_woocommerce_order_class', 10, 3);
 function wc_woocommerce_order_class($classname, $post_type, $order_id)
{
    if ($post_type = "payment_package") {
        $classname = "ET_WC_Order";
    }
    return $classname;
}
 ```

Khi thanh toán một post có post type là payment_package, đây là post type trong hệ thống của chúng ta, WC không biết dùng class nào cho nó, vì vậy chúng ta khao báo tên class là ET_WC_Order, lúc này WC sẽ new class này kèm tham số là đối tượng post đó. 

**woocommerce_product_class** : Thông báo cho WC về class mới, thay thế cho class product để xử lý dữ liệu trong product có kiểu là payment_package.

```php
$this->add_filter('woocommerce_product_class', 'wc_woocommerce_product_class', 10, 4);
function wc_woocommerce_product_class($classname, $product_type, $post_type, $product_id)
{
    if ($classname == false && (in_array($post_type, array('payment_package')))) {
        $classname = "ET_WC_Package";
    }
    return $classname;
}
```

**wc_woocommerce_product_class** : Cho WC biết danh sách các post status có thể tiến hành completed, chỉ những post có status nằm trong array này mới được chuyển sáng bước compeleted.
```php
$this->add_filter('woocommerce_valid_order_statuses_for_payment_complete', 'woocommerce_valid_order_statuses_for_payment_complete', 10, 2);
function woocommerce_valid_order_statuses_for_payment_complete($status, $order)
{
    if ($order instanceof ET_WC_Order) {
        return array('pending', 'draft');
    }
    return $status;
}
```

**wc_order_statuses** : Danh sách các status của một order có thể có
```php
$this->add_filter('wc_order_statuses', 'wc_order_statuses', 10, 2);
function wc_order_statuses($order_statuses)
{
    $et_statuses = array(
        'draft' => _x('Draft Payment', 'Order status', ET_DOMAIN),
        'pending' => _x('Pending', 'Order status', ET_DOMAIN),
        //'processing' => _x( 'Processing', 'Order status', ET_DOMAIN ),
        'publish' => _x('Publish', 'Order status', ET_DOMAIN),
    );
    return $et_statuses;
}
```

**woocommerce_payment_complete_order_status** : Status có ý nghĩa là order đã thanh toán xong
```PHP
$this->add_filter('woocommerce_payment_complete_order_status', 'woocommerce_payment_complete_order_status', 10, 2);
function woocommerce_payment_complete_order_status($status, $order)
{
	if ($order instanceof ET_WC_Order) {
		return 'publish';
	}
	return $status;
}
```

**woocommerce_order_item_needs_processing** : Trong wc có trạng thái pà processing, dùng cho các sản phẩm đang trong quá trình xử lý, ví dụ như đang giao hàng. filter này cho biết là product của chúng ta có cần được processing không.
```php
$this->add_filter('woocommerce_order_item_needs_processing', 'woocommerce_order_item_needs_processing', 10, 3);
function woocommerce_order_item_needs_processing($is_isneed, $_product, $id)
{
    if ($_product instanceof ET_WC_Package) {
        return false;
    }
    return $is_isneed;
}
```