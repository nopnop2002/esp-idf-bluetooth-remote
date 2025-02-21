# esp-idf-bluetooth-remote
Communication demonstration with a selfie device that releases the shutter of a mobile phone.   

![Image](https://github.com/user-attachments/assets/9e4b833f-5c0f-499b-9aa9-a3bb69f43309)

These are selfie devices that use a Bluetooth remote control to trigger the shutter on your phone.   
These act as GATT servers.   
This project is a GATT client that communicates with these devices.   
You can control your ESP32 using these devices.   
This project is based on [this](https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/gatt_client) example.

__Note__   
It's not the original usage, but rather a hacky usage.   
If the specifications of the product change in the future, there is a possibility that it will not work properly.   
There are many cloned products.   
There is no guarantee that it will work with all products.   

# About AB Shutter3
These devices have the remote device name ```AB Shutter3```.   
These are very cheap and available for less than $1.   
There is a small power switch on the right side of the main unit.   
When you turn on the power, it will wait for pairing.   
When pairing with ESP32 is completed, an event will be notified according to the button.   
The power will turn off automatically if there is no operation for a certain period of time.   
This is to prevent battery consumption.   
The length of time that pairing lasts varies depending on the device.   
If you turn the power off and then on again, it will wait for pairing again.   

- One button product   
	A one-button products report two notifications: button press down and button release.   
	The value and length of the notification varies by product.   
	```
	I (600588) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (600598) GATT_CLIENT: event.NotificationLength=2
	I (600598) GATT_CLIENT: event.NotificationValue=0x100 // button press

	I (601708) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (601718) GATT_CLIENT: event.NotificationLength=2
	I (601718) GATT_CLIENT: event.NotificationValue=0x0 // button release
	```

- Two button product   
	A two-button product reports three notifications: button 1 press down, button 2 press down, and button release.   
	The value and length of the notification varies by product.   
	This product notify 2 bytes.   
	```
	I (3988) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (3998) GATT_CLIENT: event.NotificationLength=2
	I (3998) GATT_CLIENT: event.NotificationValue=0x100  // button1 press

	I (42838) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (42848) GATT_CLIENT: event.NotificationLength=2
	I (42848) GATT_CLIENT: event.NotificationValue=0x200 // button2 press

	I (43678) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (43688) GATT_CLIENT: event.NotificationLength=2
	I (43688) GATT_CLIENT: event.NotificationValue=0x0 // button release
	```
	This product notify 1 bytes.   
	```
	I (2472728) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (2472738) GATT_CLIENT: event.NotificationLength=1
	I (2472738) GATT_CLIENT: event.NotificationValue=0x10

	I (2472728) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (2472738) GATT_CLIENT: event.NotificationLength=1
	I (2472738) GATT_CLIENT: event.NotificationValue=0x20

	I (2473328) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
	I (2473338) GATT_CLIENT: event.NotificationLength=1
	I (2473338) GATT_CLIENT: event.NotificationValue=0x0
	```

-  Remote device address   
	Each device has a unique Remote device address.   
	But I don't know how to use this address effectively.   
	If we can fix the pairing partner, we can pair each ESP32 with a specific device.   
	```
	I (3432) GATT_CLIENT: AB Shutter3
	I (3432) GATT_CLIENT: Device found AB Shutter3
	I (5032) GATT_CLIENT: Connected, conn_id 0, remote ff:ff:11:94:a8:63


	I (13732) GATT_CLIENT: AB Shutter3
	I (13732) GATT_CLIENT: Device found AB Shutter3
	I (14202) GATT_CLIENT: Connected, conn_id 0, remote 2b:80:3c:28:ac:1a


	I (5522) GATT_CLIENT: AB Shutter3
	I (5522) GATT_CLIENT: Device found AB Shutter3
	I (5632) GATT_CLIENT: Connected, conn_id 0, remote 2a:07:98:03:91:d5


	I (3168) GATT_CLIENT: AB Shutter3
	I (3168) GATT_CLIENT: Device found AB Shutter3
	I (3268) GATT_CLIENT: Connected, conn_id 0, remote 2a:07:98:03:fc:55
	```


# Software requirements
ESP-IDF V5.0 or later.   
ESP-IDF V4.4 release branch reached EOL in July 2024.   

# Installation   
```
git clone https://github.com/nopnop2002/esp-idf-bluetooth-remote
cd esp-idf-bluetooth-remote
idf.py menuconfig
idf.py flash
```

# Configuration   
This project has no configuration items.   


# Write to NVS   
This project records button information to NVS.   
By comparing the NVS value with the value reported by the device, we can tell which button was pressed.   
- button 1 press down   
 The NVS key is ```AB_BUTTON1``` and the NVS value type is uint32_t.   

- button 2 press down   
 The NVS key is ```AB_BUTTON2``` and the NVS value type is uint32_t.   

- button release   
 The NVS key is ```AB_RELEASE``` and the NVS value type is uint32_t.   

This is one example:   
```
W (174663) GATT_CLIENT: ESP_GATTC_NOTIFY_EVT
I (174673) GATT_CLIENT: event.NotificationLength=2
I (174673) GATT_CLIENT: event.NotificationValue=0x0
I (174683) GATT_CLIENT: button1Press=0x100
I (174693) GATT_CLIENT: button2Press=0x200
I (174693) GATT_CLIENT: buttonRelease=0x0
```
