## **All MQTT settings**

The topics are predefined once in the `MqttVariables`.

Mqtt works with subscriptions and publications. An example for a dimmer publication is 
  `Devices/PLC/House/Out/Dimmers/FB_AO_DIMMER_001/OUT`

If you change any of these topic. Keep an eye on the **length of the topic and or STRING() size**
	
```ST
	(* Broker setup *)
	clientID						:STRING:='PLC-House';
	broker							:STRING:='192.168.1.52:1883'; 	
	(* use the below for username password authentication to broker *)
	(* broker						:STRING:='user:password@192.168.1.36:1883'; *)	 
	
	(* Topics *)
	MqttMain						:STRING(16) := 'Devices/';
	MqttType						:STRING(16) := 'PLC/';
	MqttDevice						:STRING(16) := 'House/';
	// e.g. Devices/PLC/House/
	MqttBaseTopic					:STRING(100) := CONCAT(CONCAT(MqttMain,MqttType),MqttDevice);							
	MqttAvailabilityTopic 			:STRING(100) := CONCAT(MqttBaseTopic,'availability');							
	MqttAvailabilityOnlinePayload	:STRING(20) := 'online';							
	MqttAvailabilityOfflinePayload	:STRING(20) := 'offline';
	MQTTAvailabilityHartbeatTime	:TIME:=T#5S;
	MqttPubLogPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'logger/');
	MqttHADiscoveryPrefix			:STRING(16) := 'homeassistant/';
	
	(* FB Topics *)
	MqttPushbuttonPrefix			:STRING(100) := CONCAT(MqttBaseTopic,'Out/DigitalInputs/Pushbuttons/');
	MqttPubSwitchPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'Out/DigitalOutputs/');
	MqttSubSwitchPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'In/DigitalOutputs/');
	MqttPubCoverPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'Out/Covers/');
	MqttSubCoverPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'In/Covers/');
	MqttPubDimmerPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'Out/Dimmers/');
	MqttSubDimmerPrefix				:STRING(100) := CONCAT(MqttBaseTopic,'In/Dimmers/');
	MqttPubVirtualPrefix			:STRING(100) := CONCAT(MqttBaseTopic,'Out/Virtuals/');
	MqttSubVirtualPrefix			:STRING(100) := CONCAT(MqttBaseTopic,'In/Virtuals/');
	MqttPubRS485Prefix				:STRING(100) := CONCAT(MqttBaseTopic,'Out/RS485/');
	MqttSubRS485Prefix				:STRING(100) := CONCAT(MqttBaseTopic,'In/RS485/');
	
	(* Wildcard subscriptions *)
	MqttSubSwitchTopic				:STRING(100) := CONCAT(MqttSubSwitchPrefix,'+');
	MqttSubCoverTopic				:STRING(100) := CONCAT(MqttSubCoverPrefix,'+');
	MqttSubVirtualTopic				:STRING(100) := CONCAT(MqttSubVirtualPrefix,'+');
	MqttSubDimmerTopic				:STRING(100) := CONCAT(MqttSubDimmerPrefix,'#');
	MqttSubRS485Topic				:STRING(100) := CONCAT(MqttSubRS485Prefix,'#');

```

## MQTT Birth and Last will message

### **General**

The software supports so-called Birth and Last Will and Testament (LWT) messages. The former is used to send a message after the service has started, and the latter is used to notify other clients about an ungracefully disconnected client. In addtion to a Birth message on startup the Birth message is also published cyclic as a harbeat.

### **Examples**

<ins>Birth message:</ins></br>
Topic: `Devices/PLC/House/availability`</br>
Payload: `online`


<ins>LWT message:</ins></br>
Topic: `Devices/PLC/House/availability`</br>
Payload: `offline`

Note that the Topics and payloads can be changed in de code. The Birth message is by default published during startup and after that every 5 seconds (hartbeat). This can be changed as well in the code. 
