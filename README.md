# esp-idf-bluetooth-remote
Bluetooth remote control shutter driver for esp-idf.   

![Image](https://github.com/user-attachments/assets/9e4b833f-5c0f-499b-9aa9-a3bb69f43309)

These are selfie devices that use a Bluetooth remote control to trigger the shutter on your phone.   
These act as GATT servers.   
This project is a GATT client that communicates with these devices.   
This project is based on [this](https://github.com/espressif/esp-idf/tree/master/examples/bluetooth/bluedroid/ble/gatt_client) example.

# About AB Shutter3
These devices have the name ```AB Shutter3```.   
These are very cheap and available for less than $1.   
When you turn on the power, it will wait for pairing.   
When pairing with ESP32 is completed, an event will be notified according to the button.   
The power will turn off automatically if there is no operation for a certain period of time.   
This is to prevent battery consumption.   
If you turn the power off and then on again, it will wait for pairing again.   

- One button product   
 A one-button products report two notifications: button press down and button release.
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

