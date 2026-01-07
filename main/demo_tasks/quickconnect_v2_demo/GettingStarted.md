# Getting Started With Quick Connect V2 Guide

**_NOTE:_** This guide covers only Quick Connect V2. For all other demos, please refer to the [Getting Started Guide](../../../GettingStartedGuide.md).

This guide contains instructions on how to setup, build and run only the Quick Connect V2 demo
without use of the security features of the ESP32-C3 i.e. without enabling the
DS peripheral, flash encryption and Secure Boot. The guide is meant to provide the
user with a friendly first-use experience.

[1 Pre-requisites](#1-pre-requisites)<br>

[2 Demo setup](#2-demo-setup)<br>

[3 Build and flash the demo project](#3-build-and-flash-the-demo-project)<br>

[4 Monitoring the demo](#4-monitoring-the-demo)<br>


## 1 Pre-requisites

For hardware and software requirements, refer to sections [1.1 Hardware Requirements](../../../GettingStartedGuide.md#11-hardware-requirements) and [1.2 Software Requirements](../../../GettingStartedGuide.md#12-software-requirements) in the [Getting Started Guide](../../../GettingStartedGuide.md)

## 2 Demo setup

### 2.1 Get credentials from Quick Connect V2

To get credentials for your device (Thing), visit [Quick Connect Credentials](https://quickconnect.freertos.aws.com/credentials) page and enter a desired device name (Thing Name) in the `Vend Credentials` section, and click the Vend button. Download both the `.crt` and `.key` files. These are your **PEM-encoded device certificate** and **PEM-encoded private key**. (An explanation of these entities is given in the [AWS IoT Core Setup Guide](../../../AWSSetup.md).)

### 2.2 Configure the project with the AWS IoT Thing Name and AWS device Endpoint

The **AWS device Endpoint** is provided on the [Quick Connect Examples](https://quickconnect.freertos.aws.com/examples) page in the sample program code under the ENDPOINT variable.

Follow the [2.2 Configure the project](../../../GettingStartedGuide.md#22-configure-the-project-with-the-aws-iot-thing-name-and-aws-device-endpoint) section in the [Getting Started Guide](../../../GettingStartedGuide.md) to configure your project.

Select `Enable QuickConnect V2 demo` in the `Featured FreeRTOS IoT Integration` to activate the demo.


### 2.3 Provision the ESP32-C3 with the private key, device certificate and CA certificate in Development Mode

For Quick Connect, use the following specific values:

- `CA_CERT_FILEPATH`: Use the file in the `main/certs` directory, or download the AWS Root CA certificate from [here](https://www.amazontrust.com/repository/AmazonRootCA1.pem).
- `DEVICE_CERT_FILEPATH`: The file path to the PEM-encoded device certificate (`.crt` file downloaded from the Quick Connect Website)
- `PRIVATE_KEY_FILEPATH`: The file path to the PEM-encoded private key (`.key` file downloaded from the Quick Connect Website)
- `KEY_ALG_INFO`: Use `RSA 2048`

For complete provisioning instructions, follow the [2.3 Provision the ESP32-C3](../../../GettingStartedGuide.md#23-provision-the-esp32-c3-with-the-private-key-device-certificate-and-ca-certificate-in-development-mode) section in the [Getting Started Guide](../../../GettingStartedGuide.md).


## 3 Build and flash the demo project

For build and flash instructions, follow [3 Build and flash](../../../GettingStartedGuide.md#3-build-and-flash-the-demo-project) section of the [Getting Started Guide](../../../GettingStartedGuide.md).

**NOTE:** If you encounter a ***stack overflow in task CoreMqttAgentCo has been detected*** <br>
run `idf.py menuconfig`. navigate to Featured FreeRTOS IoT Integration → coreMQTT-Agent Manager Configurations, and increse the `coreMQTT-Agent task stack size` and `Connection handling task stack size` from their default values.

## 4 Monitoring the demo

1. On the serial terminal console, confirm that the TLS connection was
successful and that MQTT messages are published.

```c
I (4115) core_mqtt_agent_manager: WiFi connected.
I (4115) app_wifi: Connected with IP Address:10.0.0.9
I (4115) esp_netif_handlers: sta ip: 10.0.0.9, mask: 255.255.255.0, gw: 10.0.0.1
I (4115) main_task: Returned from app_main()
I (4685) core_mqtt_agent_manager: TLS connection established.
I (5025) coreMQTT: MQTT connection established with the broker.
I (5025) core_mqtt_agent_manager: Session present: 0

I (5025) quickconnect_v2_demo: coreMQTT-Agent connected.
I (5025) core_mqtt_agent_manager: coreMQTT-Agent connected.
I (5025) quickconnect_v2_demo: Task "DemoTask" sending publish request to coreMQTT-Agent with message [{"label":"ESP32-S3 MCU Temperature","display_type":"line_graph","unit":"C","values":[{"value":0.0,"label":"temp"}]}] on topic "Thing-name" with ID 1.
I (5055) quickconnect_v2_demo: Task "DemoTask" waiting for publish 1 to complete.
I (5175) coreMQTT: Ack packet deserialized with result: MQTTSuccess.
I (5175) coreMQTT: State record updated. New state=MQTTPublishDone.

```

2. On the [Quick Connect Visualizer](https://quickconnect.freertos.aws.com/visualizer) page, enter your device name (Thing name) in the `Visualizing device data` section, then confirm that the MQTT messages from the device are being received and a graph is being displayed.

**Note**: This demo sends data in line graph format only. For other data visualization formats, refer to the [Quick Connect](https://quickconnect.freertos.aws.com/examples) documentation.