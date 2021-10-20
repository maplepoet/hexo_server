---
title: "Wireless Comm."
date: 2021-08-19 17:51:51
tags:
url: "https://iotdesignpro.com/articles/different-types-of-wireless-communication-protocols-for-iot"
abstract: "Wireless Communication protocols used in IOT"
copyright: false
---

<!-- # Wireless Communication -->

Some **protocols used in IOT** with their features and applications

## **Wi-Fi**

Wi-Fi (Wireless Fidelity) is the most popular **IOT communication protocols** for wireless local area network (WLAN) that utilizes the IEEE 802.11 standard through 2.4 GHz UHF and 5 GHz ISM frequencies. Wi-Fi provides Internet access to devices that are within the range of about 20 - 40 meters from the source. It has a data rate upto 600 Mbps maximum, depending on channel frequency used and the number of antennas. In embedded systems, ESP series controllers from Espressif are popular for building IoT based Applications. **ESP32** and **ESP8266** are the most commonly use wifi modules for embedded applications. You can find various projects based on [ESP32](https://iotdesignpro.com/esp32-projects) and [ESP8266](https://iotdesignpro.com/esp8266-projects) by following the link.

In terms of using the Wi-Fi protocol for IOT, there are some pros & cons to be considered. The infrastructure or device cost for Wi-Fi is low & deployment is easy but the power consumption is high and the Wi-Fi range is quite moderate. So, the Wi-Fi may not be the best choice for all types of IOT applications but it can be used for applications like [Home Automation](https://iotdesignpro.com/iot-home-automation-projects).

There are many development boards available that allow people to build IOT applications using Wi-Fi. The most popular ones are the [Raspberry Pi](https://iotdesignpro.com/raspberry-pi-projects) and [Node MCU](https://iotdesignpro.com/tags/nodemcu). These boards allow people to build IOT prototypes and also can be used for small real-time applications. Likewise is the Marvell Avastar 88W8997 SoC, which follows the Wi-Fi’s IEEE 802.11n standard. The chip has applications like wearables, wireless audio & [smart home](https://iotdesignpro.com/iot-home-automation-projects).

## **Bluetooth**

Bluetooth is a technology used for exchanging data wirelessly over short distances and preffered over vaouros **IOT network protocols**. It uses short-wavelength UHF radio waves of frequency ranging from 2.4 to 2.485 GHz in the ISM band. The Bluetooth technology has 3 different versions based on its applications:

- **Bluetooth:** The Bluetooth that is used in devices for communication has many applications in IOT/M2M devices nowadays. It is a technology using which two devices can communicate and share data wirelessly. It operates at 2.4GHz ISM band and the data is split in packets before sending and then is shared using any one of the designated 79 channels operating at 1 MHz of bandwidth.
- **BLE (Bluetooth 4.0, Bluetooth Low Energy):** The BLE has a single main difference from Bluetooth that it consumes low power. With that, it makes the product of low cost & more long-lasting than Bluetooth.
- **iBeacon:** It is a simplified communication technique used by Apple and is completely based on Bluetooth technology. The Bluetooth 4.0 transmits an ID called UUID for each user and makes it each to communicate between iPhone users.

Bluetooth has many applications, such as in telephones, tablets, media players, robotics systems, etc. The range of Bluetooth technology is between 50 – 150 meters and the data is being shared at a maximum data rate of 1 Mbps.

After launching the BLE protocol, there have been many new applications developed using Bluetooth in the field of IOT. They fall under the category of low-cost consumer products and Smart-Building applications. Like Wi-Fi, **Bluetooth also has a module Bluetooth HC-05 that can be interfaced with development boards like Arduino or Raspberry Pi to build DIY projects**. When it comes to Real-time applications, Marvell’s Avastar 88W8977 comes with Bluetooth v4.2 and has features like high speed, mesh networking for IOT. Another product, M5600 is a wireless pressure transducer with a Bluetooth v4.0 embedded in it.



## **Zigbee**

ZigBee is another **iot wireless protocols** has features similar to the the Bluetooth technology. But it follows the IEEE 802.15.4 standard and is a high-level communication protocol. It has some advantages similar to Bluetooth i.e. low-power consumption, robustness, high security, and high scalability.

Zigbee offers a range of about 10 – 100 meters maximum and data rate to transfer data between communicated devices is around 250 Kbps. It has a large number of applications in technologies like M2M & IOT.

Having limitations in regards to data rate, range, and power consumption, Zigbee is only appropriate for Small-Scale Wireless applications. Though having some limitations**, it provides a 128-bit AES encryption and is giving a big hand in making secure communication for Home automation & small Industrial applications**. Zigbee too has its DIY module named **XBee** & **XBee Pro** which can be interfaced with Arduino or Raspberry Pi boards to make simple projects or application prototypes.

The company Develco has made products using Zigbee technologies like Sensors, gateways, meter interfaces, smart plugs, smart relays, etc which all work on the Zigbee wireless Mesh network, consuming low power and free from external interferences. Another company, Datanet has Zigbee based products which are used in real-time applications already, like the DNL910 & DNL920.

## **Z-Wave**

Z-Wave is a communication protocol specially designed for Home Automation products and it is also known as a low-power RF communications technology. The data packets are exchanged at data rates of 100kbps maximum and the protocol operates at a frequency of 900 MHz in the ISM band. It has a distance range of up to 30 meters maximum. It supports control of up to 232 devices. The only maker of chips for this technology is Sigma Designs. 

The Z-Wave has module ZIY (Z-Wave It Yourself) which is an Arduino & Raspberry Pi compatible board and can be used for Home Automation applications. Silicon Labs has a product Z-Wave 700, **specially developed for Smart Home applications having features like long battery life** (10 years) and improved range to about 100 meters. Also, the company has launched a Z-Wave 700 Development Kit which includes Z-Wave software, sample code and the module with an adapter, enabling others to develop Z-Wave based application products.

## **6LoWPAN**

6LowPAN (IPv6 Low-power Wireless Personal Area Network) is a network protocol that supports data encapsulation and header compression mechanisms with other applications like that of Bluetooth & ZigBee. The standard can be used across multiple communications platforms, including Ethernet, Wi-Fi, IEEE 802.15.4 and sub-1GHz ISM.

It can be adapted as Bluetooth 4.0 or ZigBee and operate at 2.4 GHz or 900 MHz, respectively. It consumes low power and can be used in a wide number of IOT and M2M applications.

6LoWPAN protocol has a 6LoWPAN L-Tek Arduino Shield that can be connected with Arduino board to get 6LoWPAN connectivity at a frequency band of 900 MHz. The users can **develop application prototypes using the module**. Talking about modules, Melange Systems has Tarang UT20 & TarangMini SM modules that have the connectivity to 6LoWPAN protocol. Microchip has developed SmartConnect 6LoWPAN for IP mesh connectivity over 802.15.4 links in 2.4GHz frequency band. IDT has ZWIR45xx series modules that are used for 6LoWPAN protocol applications.



## **RFID**

Radio-frequency identification (RFID) is a technology that uses electromagnetic fields to identify objects or tags which contains some stored information. The range of RFID varies from about 10cm to 200m maximum and such a long difference makes the two range have names like short-range distance and long-range distance. Since the range has a huge difference, the frequency at which the RFID operates has a huge difference too i.e. it starts from KHz and ranges till GHz or can be said as frequency ranges from Low frequency (LF) to Microwave depending upon the application and distance of communication.

RFID has RC522 Arduino & Raspberry Pi compatible module that can be used to build an IOT based RFID application or application prototypes like [attendace system.](https://iotdesignpro.com/projects/arduino-rfid-based-attendance-system)

## **Cellular**

The cellular network has been in use since the last 2 decades and comprises of GSM/GPRS/EDGE(2G)/UMTS or HSPA(3G)/LTE(4G) communication protocols. This protocol is generally **used for long-distance communications**. The data can be sent of large size and with high speeds compared to other technologies.

The operating frequencies range from 900 – 2100 MHz with a distance coverage of 35km to 200km and the data rates i.e. the speed of transferring data is from 35 Kbps to 10 Mbps. A company Quectel has cellular IOT products like EC21, EC23, EG91 and many more LTE standard products working on 4G. UMTS/HSPDA UC15, UC20, UC15 Mini & UC20 Mini are the 3G based IOT module launched by the same company.



## **NB-IOT**

NB-IOT stands for **Narrow Band Internet of Things**, is an LPWAN i.e. Low Power Wide Area Network technology. The technology can be used for applications requiring low power consumption, long-distance communication and for a long time (large battery life). The advantage of NB-IOT is that it has good coverage capacity i.e. the signal can transmit through walls or in underground areas where normal cellular signals won’t reach. It has a distance coverage of around 10 Kms maximum.

Quectel has launched NB-IOT modules like LTE BC95, LTE BC68 and many more modules that can be used to build real-time products in the field of IOT.

 

## **5G**

5G is the fifth generation of cellular network protocol. It’s designed for high speeds communication between smartphones as well as other devices (unlike the other cellular networks). The download speed is expected to be around 1Gbps on average. The technology protocol will work alongside with 3G & 4G technologies and would have a huge rise in Internet of Things (IOT) technology. The technology has launched in 2019 for test purposes and is available only in a few cities of the world but it is planned to launch worldwide in 2020.

Like having modules for 2G, 3G & 4G, Quectel company also has modules for 5G which are RG500Q and RM500Q, working on a sub-6 GHz frequency band and can be used for building products for IOT.



## **NFC**

NFC (Near Field Communication) is a protocol used for enabling simple and safe two-way interactions between electronic devices. It has mostly smartphones based applications like allowing contactless payment transactions, accessing digital content and connecting various electronic devices.

It operates at a frequency of 13.56MHz in the ISM band and the maximum distance range is about 10cm with a data rate of 100–420kbps. It replaces the card swiping payment transaction and can be used for wireless payment like some magic.

Being a good protocol for IOT technology, there are various modules and real-time products that follow the NFC protocol. Like the Seeed Studio NFC shield, DFRobot NFC module, Grove NFC, and all 3 of them are Arduino and Raspberry Pi compatible. For real-time products, NFC has CLRC663 plus, MFRC630, NTAG I2C plus products.

 

## **LoRaWAN**

LoRa is getting popular now days and used in **IOT network protocol**. LoRaWAN (Long Range Wide Area Network) has applications for long distances and is designed to **provide low-power for communication in IoT,** **M2M applications**. It has a capacity of connecting millions of devices with data rates ranging from 0.3 kbps to 50 kbps. The distance for LoRaWAN application ranges from 2 - 5km for the urban environment & maximum 15km for the suburban environment.

TE has launched products like MS8607, HTU21D, and MS5637 which are used to get humidity, temperature & barometric pressure values using the LoRaWAN protocol and has a major role in the field of IOT.

 

## **LTE-M**

LTE-M is also known as LTE (Long Term Evolution) Cat-M1 protocol. It is a technology used to connect IOT devices directly with 4G network without the need to access through any gateway in between. It provides a data rate of about 100 Kbps and chips are less costly. Since it transmits less data, it provides a long battery life to the devices.

A module named LTE BG96 Cat M1 module is used to make IOT based products working on LTE-M protocol. The same module also supports LTE Cat NB1 protocol with an improved data rate of 375 kbps downlink & uplink speed.



------------------------------

Source: https://iotdesignpro.com/articles/different-types-of-wireless-communication-protocols-for-iot

