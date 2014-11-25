# Extend class WC_Abstract_Order

*Tài liệu được đánh dấu là chưa hoàn thiện, đang phát triển. Vì vậy có thể có nhiều thay đổi trong tương lai, vui lòng start và theo dõi notification để luôn cập nhật*


## Chức năng

Class này có chức năng chính là lưu thuộc tính của một order, kèm theo các phương thức xử lý dữ liệu, cũng bao gồm luôn các phương thức xử lý coupon, ...

Class này tương ứng với class ET_AdOrder trong CE. 

## Extend

Mục đích chúng ta cần extend class này là để map các thuộc tính của package với thuộc tính của product để WC có thể sử dụng được. Chúng ta override lại các phương thức để thay vì xử lý dữ liệu trên product thì giờ ta xử lý lại trên package theo bussiness riêng.

```PHP
if(class_exists("WooCommerce")):
    class ET_WC_Order extends WC_Abstract_Order
    {
        protected $etOrder;
        protected $cancel_url = "";
        protected $received_url = "";
        public function __construct($order = '')
        {
            $this->init($order);
        }
       public function populate($result)
       {
           $orderData = $result->get_order_data();
           // Standard post data
           $this->id = $result->ID;
           $this->order_date = $orderData['created_date'];
           $this->modified_date = $orderData['created_date'];
           $this->customer_message = '';
           $this->customer_note = '';
           $this->post_status = $orderData['status'];
           // Billing email cam default to user if set
           if (empty($this->billing_email) && !empty($this->customer_user)) {
               $user = get_user_by('id', $this->customer_user);
               $this->billing_email = $user->user_email;
           }
           $this->cancel_url = et_get_page_link('process-payment', array('paymentType' => $orderData['payment']));
           $this->received_url = et_get_page_link('process-payment', array('paymentType' => $orderData['payment']));
       }
       public function update_status($new_status, $note = '')
       {
           if (!$this->id) {
               return;
           }

           // Standardise status names.
           $new_status = 'wc-' === substr($new_status, 0, 3) ? substr($new_status, 3) : $new_status;
           $old_status = $this->get_status();
           switch (strtoupper($new_status)) {
               case 'COMPLETED':
               case 'PUBLISH':
                   $this->post_status = 'publish';
                   break;
               case 'PROCESSING' :
               case 'ON-HOLD':
               $this->post_status = 'pending';
                   break;
               case 'CANCELLED' :
                   $this->post_status = 'draft';
                   break;
               default:
                   $this->post_status = 'draft';
                   break;
           }
           $this->etOrder->post_status = $this->post_status;
           $log = new WC_Logger();
           $log->add('paypal', "Our Debug : " . $this->post_status);
           wp_update_post(array('ID' => $this->etOrder->ID, 'post_status' => $this->post_status));
       }
    }
endif;
```

## Thuộc tính

Cũng vì lý do integrate, vì vậy class này xem như là cầu nối, một bên là dữ liệu của ce, một bên là WC cần các phương thức inteface. Vì vậy chúng ta cũng phải lưu thêm một thuộc tính mới là $etOrder để chứa các dữ liệu của payment trong ce.

## Phương thức

Dưới đây chỉ là một vài phương thức cơ bản, khi extend cần override các phương thức này để xử lý dữ liệu theo cách của chúng ta.

|                   Tên                  	| Tham số         	| Giá trị trả về           	| Mô tả                                       	|
|:--------------------------------------:	|-----------------	|--------------------------	|---------------------------------------------	|
| get_product_from_item($item)           	| mixed           	| Đối tượng "like" product 	| Lấy đối tượng kiểu product                  	|
| update_status($new_status, $note = '') 	| string , string 	| void                     	| Cập nhật trạng thái của order               	|
| get_items($type = '')                  	| string          	| array                    	| Lấy tất cả các product item trong một order 	|
| get_total()                            	| null            	| int                      	| lấy giá trị của order                       	|

