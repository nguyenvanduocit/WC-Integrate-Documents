# Extend class WC_Product

**Tài liệu đang phát triển**

## Chức năng

Chức năng của class này là để mô tả một sản phẩm, nó chứa dữ liệu của một sản phẩm, đồng thời nó cũng chứa các phương thức để xửa lý các dữ liệu này.

Class này tương ứng với class package trong CE. 

## Extend

Mục đích chúng ta cần extend class này là để map các thuộc tính của package với thuộc tính của product để WC có thể sử dụng được. Chúng ta override lại các phương thức để thay vì xử lý dữ liệu trên product thì giờ ta xử lý lại trên package theo bussiness riêng.

```PHP
if(class_exists("WooCommerce")):
    class ET_WC_Package extends WC_Product
    {}
endif;
```

## Thuộc tính

|      Tên     	| Kiểu   	| Mô tả                                   	|
|:------------:	|--------	|-----------------------------------------	|
|      id      	| int    	| id của post                             	|
|     post     	| object 	| Post                                    	|
| product_type 	| String 	| Kiểu của product, vd : simple, variable 	|

Bên cạnh các thuộc tính trên, ta thêm một thuộc tính như sau

|      Tên     	| Kiểu          	| Mô tả                                   	|
|:------------:	|---------------	|-----------------------------------------	|
|   package     | payment_package  	| đối tượng package trong hệ thống của ce  	|

## Phương thức

Dưới đây chỉ là một vài phương thức cơ bản, khi extend cần override các phương thức này để xử lý dữ liệu theo cách của chúng ta.

|       Tên       	| Tham số 	| Giá trị trả về 	| Mô tả                                      	|
|:---------------:	|---------	|----------------	|--------------------------------------------	|
| get_post_data() 	| null    	| WP_Post        	| Trả về đối tượng post chứa dữ liệu về post 	|
| get_permalink() 	| null    	| String         	| Lấy url trỏ đến product                    	|
|   is_virtual()  	| null    	|                	| Kiểu của product, vd : simple, variable    	|

