##  Tencent IOT SDK for rt-thread Package 
## 1 介绍
Tencent IOT SDK for rt-thread Package 是基于腾讯云物联网开发平台（IoT Explorer）[设备端 C-SDK] (https://github.com/tencentyun/qcloud-iot-explorer-sdk-embedded-c.git)在RThread环境开发的软件包，配合平台对设备数据模板化的定义，实现和云端基于数据模板协议的数据交互框架，开发者基于IoT_Explorer C-SDK数据模板框架，通过脚本自动生成模板代码，快速实现设备和平台、设备和应用之间的数据交互。

### 1.1 SDK架构图
![sdk-architecture](https://main.qcloudimg.com/raw/583a783ca9189bd3beb9df0991dc0bec.jpg)

### 1.2 目录结构

| 名称            | 说明 |
| ----            | ---- |
| docs            | 文档目录 |
| qcloud-iot-explorer-sdk-embedded-c         | 腾讯物联网设备端C-SDK |
| ports           | 移植文件目录 |
| samples         | 示例目录 |
| LICENSE         | 许可证文件 |
| README.md       | 软件包使用说明 |
| SConscript      | RT-Thread 默认的构建脚本 |

### 1.3 许可证

沿用`qcloud-iot-sdk-embedded-c`许可协议MIT。

## 2 软件包使用
### 2.1 menuconfig配置
- RT-Thread env开发工具中使用 `menuconfig` 使能 `Tencent-IoT` 软件包，配置产品及设备信息，并根据产品需求选择合适的应用示例修改新增业务逻辑，也可新增例程编写新的业务逻辑。

```
 --- Tencent-IoT:  Tencent Cloud IoT Explorer Platform SDK for RT-Thread   
 (0WUKPUCOTC) Config Product Id (NEW)                                      
 (dev001) Config Device Name (NEW)                                         
 (N6B8M91PB4YDTRCpqvOp4w==) Config Device Secret (NEW)                     
 [*]   Enable TLS/DTLS                                                     
       Select Product Type (Data template protocol)  --->                  
 [ ]   Enable Event (NEW)                                                  
 [ ]   Enable Action (NEW)                                                 
 [*]   Enable Smart_light Sample (NEW)                                     
 [ ]   Enable OTA (NEW)                                                    
 [ ]   Enable GateWay (NEW)                                                
       Version (latest)  --->                                              
```

- 选项说明

`Config Product Id`：配置产品ID，平台创建生成。

`Config Device Name`：配置设备名，平台创建生成。

`Config Device Secret`：配置设备密钥，平台创建生成，考虑到嵌入式设备大多没有文件系统，暂时没有支持证书设备配置。

`Enable TLS/DTLS`： 是否使能TLS，若使能，则会关联选中mbedTLS软件包。

` Select Product Type`：产品类型为自定义产品还是[数据模板协议](https://cloud.tencent.com/document/product/1081/34916)产品。

`Enable event`：选用数据模板的前提下是否使能事件功能。

`Enable Action`：选用数据模板的前提下是否使能行为功能。
 
`Enable Smart_light Sample`：是否使能智能灯场景示例。

`Enable OTA`：是否使能OTA示例。若使能OTA可进一步选择下载使用HTTPS还是HTTP。

`Enable GateWay`：是否使能网关示例。

`Version`：软件包版本选择, v3.1.2及以后的版本只支持物联网开发平台(IoT Explorer)，若需使用物联网通信平台(IoT Hub),请选择3.0.2版本及之前的版本。

- 相关链接
 [物联网通信平台SDK说明文档](https://github.com/tencentyun/qcloud-iot-sdk-embedded-c/blob/master/docs/物联网通信平台.md)

- 使用 `pkgs --update` 命令下载软件包

### 2.2 编译及运行
1. 使用命令 scons --target=xxx 输出对应的工程，编译 

2. 执行示例程序：

### 2.4 运行demo程序
系统启动后，在 MSH 中使用命令执行：

#### 2.4.1 数据模板智能灯 + TLS示例：
- 示例说明：该示例展示了设备和[物联网开发平台](https://cloud.tencent.com/product/iotexplorer)基于[数据模板协议](https://cloud.tencent.com/document/product/1081/34916)的通信示例，关于数据模板协议参见链接说明，使能TLS，示例在物联网开发平台下发控制灯为红色的命令，设备端收取了消息，打印颜色，并上报对应消息。

- 配置选项
```
--- Tencent-IoT:  Tencent Cloud IoT Explorer Platform SDK for RT-Thread     
   (0WUKPUCOTC) Config Product Id                                           
   (dev001) Config Device Name                                              
   (N6B8M91PB4YDTRCpqvOp4w==) Config Device Secret                          
   [*]   Enable TLS/DTLS                                                    
         Select Product Type (Data template protocol)  --->                 
   [*]   Enable Event                                                       
   [*]   Enable Action                                                      
   [*]   Enable Smart_light Sample                                          
   [ ]   Enable OTA                                                         
   [ ]   Enable GateWay                                                     
         Version (latest)  --->       
```

- 运行示例

```
 \ | /
- RT -     Thread Operating System
 / | \     4.0.3 build Jun 15 2020
 2006 - 2020 Copyright by rt-thread team
lwIP-2.0.2 initialized!
[I/sal.skt] Socket Abstraction Layer initialize success.
[I/SDIO] SD card capacity 65536 KB.
hello rt-thread
msh />tc_data_template_light_example start
INF|20|qcloud_iot_device.c|iot_device_info_set(65): SDK_Ver: 3.1.1, Product_ID: 0WUKPUCOTC, Device_Name: dev001
msh />DBG|20|HAL_TLS_mbedtls.c|HAL_TLS_Connect(228):  Connecting to /0WUKPUCOTC.iotcloud.tencentdevices.com/8883...
DBG|21|HAL_TLS_mbedtls.c|HAL_TLS_Connect(233):  Setting up the SSL/TLS structure...
DBG|21|HAL_TLS_mbedtls.c|HAL_TLS_Connect(285): Performing the SSL/TLS handshake...
INF|21|mqtt_client.c|IOT_MQTT_Construct(127): mqtt connect with id: qPU1e success
DBG|21|mqtt_client_subscribe.c|qcloud_iot_mqtt_subscribe(141): topicName=$thing/down/property/0WUKPUCOTC/dev001|packet_id=64736
DBG|21|data_template_client.c|_template_mqtt_event_handler(242): template subscribe success, packet-id=64736
INF|21|light_data_template_sample.c|event_handler(321): subscribe success, packet-id=64736
INF|21|data_template_client.c|IOT_Template_Construct(785): Sync device data successfully
DBG|21|mqtt_client_subscribe.c|qcloud_iot_mqtt_subscribe(141): topicName=$thing/down/event/0WUKPUCOTC/dev001|packet_id=64737
DBG|21|mqtt_client_subscribe.c|qcloud_iot_mqtt_subscribe(141): topicName=$thing/down/action/0WUKPUCOTC/dev001|packet_id=64738
INF|21|light_data_template_sample.c|data_template_light_thread(650): Cloud Device Construct Success
DBG|21|light_data_template_sample.c|_usr_init(354): add your init code here
INF|21|light_data_template_sample.c|_register_data_template_property(432): data template property=power_switch registered.
INF|21|light_data_template_sample.c|_register_data_template_property(432): data template property=color registered.
INF|21|light_data_template_sample.c|_register_data_template_property(432): data template property=brightness registered.
INF|21|light_data_template_sample.c|_register_data_template_property(432): data template property=name registered.
INF|21|light_data_template_sample.c|data_template_light_thread(676): Register data template propertys Success
INF|21|light_data_template_sample.c|_register_data_template_action(294): data template action=blink registered.
INF|21|light_data_template_sample.c|data_template_light_thread(686): Register data template actions Success
DBG|21|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report_info", "clientToken":"0WUKPUCOTC-0", "params":{"module_hardinfo":"ESP8266","module_softinfo":"V1.0","fw_ver":"3.1.1","imei":"11-22-33-44","lat":"22.546015","lon":"113.941125", "device_label":{"append_info":"your self define info"}}}
DBG|21|data_template_client.c|_template_mqtt_event_handler(242): template subscribe success, packet-id=64737
INF|21|light_data_template_sample.c|event_handler(321): subscribe success, packet-id=64737
DBG|21|data_template_client.c|_template_mqtt_event_handler(242): template subscribe success, packet-id=64738
INF|21|light_data_template_sample.c|event_handler(321): subscribe success, packet-id=64738
DBG|21|data_template_client.c|_reply_ack_cb(169): replyAck=0
DBG|21|data_template_client.c|_reply_ack_cb(172): Received Json Document={"method":"report_info_reply","clientToken":"0WUKPUCOTC-0","code":0,"status":"success"}
DBG|21|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"get_status", "clientToken":"0WUKPUCOTC-1"}
DBG|21|data_template_client.c|_get_status_reply_ack_cb(185): replyAck=0
DBG|21|data_template_client.c|_get_status_reply_ack_cb(189): Received Json Document={"method":"get_status_reply","clientToken":"0WUKPUCOTC-1","code":0,"status":"success","data":{"reported":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}}
DBG|21|light_data_template_sample.c|data_template_light_thread(709): Get data status success
DBG|22|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-2", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}
INF|22|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|23|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-2","code":0,"status":"success"}
DBG|27|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-3", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}
INF|27|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|28|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-3","code":0,"status":"success"}
DBG|32|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-4", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}
INF|32|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|33|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-4","code":0,"status":"success"}
DBG|37|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-5", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}
INF|37|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|38|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-5","code":0,"status":"success"}
DBG|43|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-6", "params":{"power_switch":0,"color":0,"brightness":0,"name":"dev001"}}
INF|43|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|44|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-6","code":0,"status":"success"}
DBG|46|data_template_client_manager.c|_on_template_downstream_topic_handler(527): control_str:{"color":1,"brightness":1,"name":"","power_switch":1}
INF|46|light_data_template_sample.c|OnControlMsgCallback(405): Property=brightness changed
INF|46|light_data_template_sample.c|OnControlMsgCallback(405): Property=color changed
INF|46|light_data_template_sample.c|OnControlMsgCallback(405): Property=power_switch changed
[  lighting  ]|[color:GREEN]|[brightness:--------------------]|[dev001]
DBG|46|data_template_client.c|IOT_Template_ControlReply(698): Report Document: {"code":0, "clientToken":"clientToken-21f40729-cd65-425c-aaa8-4e6de6aecbbf"}
DBG|46|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"control_reply", "code":0, "clientToken":"clientToken-21f40729-cd65-425c-aaa8-4e6de6aecbbf"}
DBG|46|light_data_template_sample.c|data_template_light_thread(744): Contol msg reply success
DBG|46|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-7", "params":{"power_switch":1,"color":1,"brightness":1}}
INF|46|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
DBG|46|mqtt_client_publish.c|qcloud_iot_mqtt_publish(334): publish topic seq=64739|topicName=$thing/up/event/0WUKPUCOTC/dev001|payload={"method":"event_post", "clientToken":"0WUKPUCOTC-8", "eventId":"status_report", "type":"info", "timestamp":0, "params":{"status":1,"message":"light on"}}
INF|47|light_data_template_sample.c|event_handler(335): publish success, packet-id=64739
INF|47|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-7","code":0,"status":"success"}
DBG|47|data_template_event.c|_on_event_reply_callback(105): recv:{"method":"event_reply","clientToken":"0WUKPUCOTC-8","code":0,"status":"","data":{}}
DBG|47|data_template_event.c|_on_event_reply_callback(123): eventToken:0WUKPUCOTC-8 code:0 status:(null)
DBG|47|light_data_template_sample.c|event_post_cb(185): recv event reply, clear event
DBG|47|data_template_event.c|_traverse_event_list(79): eventToken[0WUKPUCOTC-8] released
DBG|48|mqtt_client_publish.c|qcloud_iot_mqtt_publish(340): publish packetID=0|topicName=$thing/up/property/0WUKPUCOTC/dev001|payload={"method":"report", "clientToken":"0WUKPUCOTC-9", "params":{"power_switch":1,"color":1,"brightness":1,"name":"dev001"}}
INF|48|light_data_template_sample.c|data_template_light_thread(761): data template reporte success
INF|49|light_data_template_sample.c|OnReportReplyCallback(416): recv report_reply(ack=0): {"method":"report_reply","clientToken":"0WUKPUCOTC-9","code":0,"status":"success"}
```

### 2.5 服务端下行消息控制
- [物联网开发平台](https://cloud.tencent.com/product/iotexplorer)可以通过控制台直接调试，如下截图
![control](https://main.qcloudimg.com/raw/6027fc232f761b4726a13eb964721b09.jpg)



### 2.6 其他示例说明
 关于 SDK 的更多使用方式及接口了解, 参见 `qcloud_iot_api_export.h`，其他示例不再一一列举。

### 2.7 可变参数配置
开发者可以根据具体场景需求，配置相应的参数，满足实际业务的运行。可变接入参数包括：
1. MQTT 心跳消息发送周期, 单位: ms 
2. MQTT 阻塞调用(包括连接, 订阅, 发布等)的超时时间, 单位:ms。 建议 5000 ms
3. TLS 连接握手超时时间, 单位: ms
4. MQTT 协议发送消息和接受消息的 buffer 大小默认是 512 字节，最大支持 256 KB
5. CoAP 协议发送消息和接受消息的 buffer 大小默认是 512 字节，最大支持 64 KB
6. 重连最大等待时间

修改 qcloud_iot_export.h 文件如下宏定义可以改变对应接入参数的配置。

```
/* MQTT心跳消息发送周期, 单位:ms */
#define QCLOUD_IOT_MQTT_KEEP_ALIVE_INTERNAL                         (240 * 1000)

/* MQTT 阻塞调用(包括连接, 订阅, 发布等)的超时时间, 单位:ms 建议5000ms */
#define QCLOUD_IOT_MQTT_COMMAND_TIMEOUT                             (5000)

/* TLS连接握手超时时间, 单位:ms */
#define QCLOUD_IOT_TLS_HANDSHAKE_TIMEOUT                            (5000)

/* MQTT消息发送buffer大小, 支持最大256*1024 */
#define QCLOUD_IOT_MQTT_TX_BUF_LEN                                  (512)

/* MQTT消息接收buffer大小, 支持最大256*1024 */
#define QCLOUD_IOT_MQTT_RX_BUF_LEN                                  (512)

/* COAP 发送消息buffer大小，最大支持64*1024字节 */
#define COAP_SENDMSG_MAX_BUFLEN                                     (512)

/* COAP 接收消息buffer大小，最大支持64*1024字节 */
#define COAP_RECVMSG_MAX_BUFLEN                                     (512)

/* 重连最大等待时间 */
#define MAX_RECONNECT_WAIT_INTERVAL                                 (60000)

```