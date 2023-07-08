# AT指令

### ESP32/ESP8266使用AT指令连接华为云并上报属性流程

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
  ``````
