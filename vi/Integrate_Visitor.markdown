Hệ thống thanh toán của CE sử dụng các visitor. Vì vậy để integrate, ta cũng phải tạo một visitor : 
```php
if (class_exists("WooCommerce")) {
//Integrate with WC payment
	class ET_WC_Payment_IntegrateVisitor extends ET_PaymentVisitor
	{

		protected $_payment_type;
		private $gatewayType;
		private $gateway;

		function __construct($paymentType)
		{
			$this->_payment_type = $paymentType;
			$this->gatewayType = $paymentType;
			$available_payment_gateways = WooCommerce::instance()->payment_gateways()->get_available_payment_gateways();
			$paymentType_lower = strtolower($paymentType);
			if (array_key_exists($paymentType_lower, $available_payment_gateways)) {
				$this->gateway = $available_payment_gateways[$paymentType_lower];
			}
		}

		//return url
		function setup_checkout(ET_Order $order)
		{
			$order->received_url = $this->_settings['return'];
			$order->cancel_url = $this->_settings['cancel'];
			$payment_result = $this->gateway->process_payment($order);
			return array(
				'url' => $payment_result["redirect"],
				'ACK' => true,
				'extend' => false,
			);
		}

		//This function is useless
		function do_checkout(ET_Order $order)
		{
			$order_datas = $order->get_order_data();
			$paymentStatus = 'fraud';
			switch (strtoupper($order_datas['status'])) {
				case 'COMPLETED':
				case 'PUBLISH':
					$paymentStatus = 'Completed';
					break;
				case 'PROCESSING':
				case 'PENDING':
					$paymentStatus = 'pending';
					break;
				case 'DRAFT':
					$paymentStatus = 'fraud';
					break;
				default:
					$paymentStatus = 'fraud';
					break;
			}
			return array(
				'ACK' => in_array(strtoupper($order_datas['status']), array('COMPLETED', 'PUBLISH', 'PROCESSING', 'PENDING')),
				'payment' => $this->gatewayType,
				'payment_status' => $paymentStatus
			);
		}

	}
}
```

Đồng thời trong class CE_Payment_Factory, bạn cũng phải thêm một trường hợp đặt biệt để có thể sử dụng visitor này :

```php
if (class_exists("WooCommerce")) {
	$available_payment_gateways = WooCommerce::instance()->payment_gateways()->get_available_payment_gateways();
	$paymentType_lower = strtolower($paymentType);
	if (array_key_exists($paymentType_lower, $available_payment_gateways)) {
		$class = new ET_WC_Payment_IntegrateVisitor($paymentType);
		return apply_filters('et_factory_build_payment_visitor', $class, $paymentType, $order);
	}
}
```