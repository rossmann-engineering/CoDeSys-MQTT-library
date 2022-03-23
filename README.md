CoDeSys MQTT library Overview

The purpose of this library is to publish PLC variables to an MQTT Broker. Suitable for industrial IoT Solutions.  
The library is compatible with PLCs based on CoDeSys V3 and already used in industrial applications. 
One single Function block is necessary, programmed in IEC61131 code. Documentation is integrated in the library.

Features:  

- MQTT protocol Version 3.1.1
- Publish either to a local MQTT-Broker or a public MQTT-Broker
- Subscribe to a Topic
- Well documented IEC61131 library
- supports QoS0 (At-most-once)
- Optional auto reconnect
- Optional User and Password authentication
- Last will and Testament

Function block: FB_MQTTClient  
Library: MQTT_Client.library  
Namespace: MQTT_Client
  
|  Name |  Datatype |  Initial Value	 | Comment  |
|---|---|---|---|
|  i_xEnable |  BOOL |  TRUE |  Enables the Function block. The rising Edge of this Input automatically connects to the MQTT-Broker |
| i_sBrokerAddress  |  STRING |  'www.mqtt-dashboard.com' | IP-Address (example: 192.168.178.101) or Webaddress (www.mqtt-dashboard.com) of the MQTT-Broker|
|  i_uiPort | UINT  |  1883 | Port of the MQTT-Broker|
|  i_sUsername | STRING  |  '' |  Optional Username if required - Only change initial value if required|
|  i_sPassword | STRING  | ''  |  Optional Password if required - Only change initial value if required|
|  i_sWillTopic |  STRING |  '' | Optional Topic the Will Message will be published to- leave untouched if not required|
|  i_sWillMessage | STRING  | ''  |  Optional Will Message - leave untouched if not required|
|  i_sWillRetain | STRING  |  FALSE |  Retain Last Will Message, if required Standard: FALSE - otherwise leave untouched|
|  i_xAutoReconnect | BOOL  | TRUE  |  Automatically try to reconnect after an Exception|
|  i_sPayload | STRING  | 'Hello MQTT-Broker from CoDeSys'  |  Payload to Publish|
|  i_sTopicPublish | STRING  | 'CoDeSys'  |  Topic to publish to |
|  i_sTopicSubscribe | STRING  | 'CoDeSys'  | Subscribtion Topic|
|  i_xRetain | BOOL  |  TRUE | Retain Flag|
|  i_xPublish | BOOL  | FALSE  | Publish the Payload to the Topic of the MQTT-Broker|
|  i_xSubscribe | BOOL  | FALSE  |  Subscribe to the Topic given in i_sTopicSubscribe|
|  i_sClientID | STRING  |  12345678 | Sets a custom Client ID, leave at default for Automatik|
|  q_stLastReceivedMessage |  STRING |   | Message Received from a Topic|
|  q_stLastReceivedMessageTopic | STRING  |   | Topic Last received Message |
|  q_arsLastReceivedMessages | ARRAY [0..24] of STRING  |   | Last 25 Received Messages|
|  q_arsLastReceivedMessagesTopic |  ARRAY [0..24] of STRING |   |  Last 25 Received Topics|
|  q_xReceivedMessageNotification | BOOL  |   |  Message Received Notification|
|  q_sDiagMsg |  STRING |   |  Diag Message - Current State|
|  q_udiState | UDINT  |   | Current state of the Function block|
|  q_xError |  BOOL |   |  Error Flag - The Exception will be acknowledged with a rising edge at 'i_xEnable'|


Function block state (output 'q_udiState'):

0: Wait for Enable (Rising Edge)  
5: Create Socket  
10: Connect to TCP-Server  
15: Initialize Out-Buffer  
20: Send Connect to the MQTT-Broker  
30: Wait for CONNACK from MQTT-Broker  
40: Analyze CONNACK  
60: Wait for Publish (Goto 70) or Subscribe (Goto 65)   or Disconnect (Enable low) (Goto 80)  
63: Analyze Publish Message from Broker  
65: Subscribe to Topic'  
66: Wait for SUBACK from Broker  
67: Analyze SUBACK  
70: Publish Message (Goto to 60)  
80: Send Disconnect  
90: Disconnect Socket  

Possible Diag Messages (output 'q_sDiagMsg')

- Wait for Enable
- Create Socket
- Connect to TCP-Server
- Initialize Out-Buffer
- Connect to MQTT-Broker
- Wait for Connack from Broker
- Analyze connack
- Wait for Publish or Subscribe or Disconnect (Enable low)
- Analyze Publish Message from Broker
- Subscribe to Topic
- Wait for SUBACK from Broker
- Analyze SUBACK
- Publish Message
- Send Disconnect notification to server
- Close TCP Socket

Exceptions

- Creating Socket Timed Out
- Connecting to TCP-Server Timed out
- Connecting to MQTT-Broker (MQTT Control Packet) Timed out
- Waiting for "Connack" from MQTT-Broker Timed out
- Connection Refused, unacceptable protocol version
- Connection Refused, identifier rejected
- Connection Refused, Server unavailable
- Connection Refused, bad Username or password
- Connection Refused, not autorized
- Publishing Message (MQTT Control Packet) Timed out
- Sending Disconnect to MQTT-Broker Timed out
- Closing TCP-Socket Timed out
- Suback Message reported Failure (Return code 0x80)
- Waiting for "Suback" from MQTT-Broker Timed out
- Subscribe (MQTT Control Packet) Timed out