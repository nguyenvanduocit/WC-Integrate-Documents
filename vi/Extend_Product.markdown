# Extend class WC_Product

## Chức năng

Chức năng của class này là để mô tả một sản phẩm, nó chứa dữ liệu của một sản phẩm, đồng thời nó cũng chứa các phương thức để xửa lý các dữ liệu này.

Class này tương ứng với class package trong CE. 

## Extend

Mục đích chúng ta cần extend class này là để map các thuộc tính của package với thuộc tính của product để WC có thể sử dụng được. Chúng ta override lại các phương thức để thay vì xử lý dữ liệu trên product thì giờ ta xử lý lại trên package theo bussiness riêng.

## Thuộc tính

|      Tên     	| Kiểu   	| Mô tả       	|
|:------------:	|--------	|-------------	|
|      id      	| int    	| id của post 	|
|     post     	| object 	| Post        	|
| product_type 	| String 	|             	|

## Phương thức

df