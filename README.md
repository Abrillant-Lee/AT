# AT指令(Attention)

### 1. AT指令简介

- AT命令，用来控制TE（TerminalEquipment）和MT(Mobile Terminal)之间交互的规则
  
  > 在物联网模块用于控制物联网模块与其他设备之间的交互规则，例如连接WiFi、连接蜂窝网络、发送数据等。在物联网应用中，AT指令可以用于控制设备的联网、数据传输等功能。

- AT 指令以 AT 开始，以 \r 或者 \r\n 结尾，参数之间使用 , 隔开，字符串参数使用双引号 "" 包裹，整形参数不适用双引号。
  
  ### 2. AT指令分类

- Test 命令：AT+<x>=? 
  
  - 测试指令类似于命令行里的 help 指令，用于提供该命令的使用信息，以及命令参数的取值范围。

- Read 命令：AT+<x>?
  
  - 用于查询该指令对应功能的当前值

- Set 命令：AT+<x>=<…>
  
  - 设置用户指定的参数到对应的功能里。

- Execute 命令：AT+<x>
  
  - 执行相关操作。

### 3. AT指令响应

- AT 标准定义了标准的响应结果字符串:  
  `\r\nOK\r\n`：表示命令执行成功。  
  `\r\nERROR\r\n`：表示命令执行失败。  
  `\r\n+CME ERROR: <err>\r\n`：表示命令执行失败，err 为错误码。  
  `\r\n+CMS ERROR: <err>\r\n`：表示命令执行失败，err 为错误码。  

### 4. ESP32/ESP8266使用AT指令连接华为云并上报属性流程

> 注:ESP32/ESP8266需要先烧录支持MQTT的AT固件,固件下载地址:https://docs.ai-thinker.com/%E5%9B%BA%E4%BB%B6%E6%B1%87%E6%80%BB, 烧录软件推荐使用flash_download_tool(如上文件),烧录固件时需要将ESP32/ESP8266的GPIO0引脚拉低,然后点击烧录按钮,烧录完成后,将GPIO0引脚拉高,重启ESP32/ESP8266,即可使用AT指令连接华为云并上报属性

1. 设置模块为STA模式:
   
   ```
   AT+CWMODE=1
   ```

2. 连接WIFI
   
   ```
   AT+CWJAP="WIFI名称","WIFI密码
   ```

3. 设置MQTT的登陆用户名与密码
   
   ```
   AT+MQTTUSERCFG=0,1,"NULL","填写用户名","填写密码",0,0,""
   ```

4. 设置MQTT的ClientID
   
   ```
   AT+MQTTCLIENTID=0,"填写ClientID"
   ```

5. 设置MQTT接入地址
   
   ```
   AT+MQTTCONN=0,"填写MQTT接入的地址",1883,1
   ```

6. 订阅主题
   
   ```
   AT+MQTTSUB=0,"订阅的主题tpoic",1
   ```

> 属性上报的topic可填写为:  
> $oc/devices/填写设备ID/sys/properties/report

7. 上报数据:
   
   ```
   AT+MQTTPUB=0," 订阅的主题tpoic ","上报的json数据",0,0
   ```
- 上报的json数据
  
  ```
  {\"services\":[{\"service_id\":\"填写服务ID\"\,\"properties\":{\"填写设备属性\": 填写属性数据值}}]}
  ```

  如: 

```
AT+MQTTPUB=0,"$oc/devices/64a8d175ae80ef457fc080fe_0001/sys/properties/report","{\"services\":[{\"service_id\":\"0001\"\,\"properties\":{\"DS18B20\":100}}]}",0,0
```

> [AT 命令集 — ESP-AT 用户指南 文档 (espressif.com)](https://docs.espressif.com/projects/esp-at/zh_CN/release-v2.2.0.0_esp8266/AT_Command_Set/index.html)


### 5. 如何使用STM32连接华为云
在上述AT指令+ESP32/8266的基础上将串口调试器发送AT指令改为STM32串口发送AT指令

------------------------------------------------------------------------------------------------------------------------------
                              我是有底线的😪😪😪


