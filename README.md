# WebSocketStreamClient

A WebSocketClient that implements Client.h so that the PubCubClient MQTT library can use it - with wss or ws.

## Dependencies

* [ArduinoHttpClient 0.4.0](https://github.com/arduino-libraries/ArduinoHttpClient) and all of it's dependencys. 
<s>Because this works with esp8266/2.4.2 but not with esp8266/2.5.0, use the supplied WebSocketClient250 class instead.</s>

WITH Esp8266 Boards version 3.0.x and 2.7.X : (don't work on previous versions)<br>
no more use of websocketclient250 <br>
(websocketstream updated to meet client.h declarations)<br>

<!> IF the size of MQTT MESSAGE is > 128 bytes, the websocket hang<br>
=> add (pubsubclient)   .setBufferSize(512)<br>
=> change in websocket.h iTxBuffer[512]<br>
   or see : https://github.com/arduino-libraries/ArduinoHttpClient/pull/124<br>

<s>Tested with esp8266/2.5.0 AND ALSO 2.6.3 Board libraries. There is an example node.js server that you can use here with this library at [web-socket-mqtt](https://github.com/areve/web-socket-mqtt)</s>.


## Usage

See examples folder for a complete program.

```cpp

void onMqttPublish(char *topic, byte *payload, unsigned int length)
{
  // handle mqtt messages here
}

....

WiFiClientSecure wiFiClient;
WebSocketClient wsClient(wiFiClient, host, port);
WebSocketStreamClient wsStreamClient(wsClient, path);
PubSubClient mqtt(wsStreamClient);

wiFiClient.setFingerprint(fingerprint);
mqtt.setCallback(onMqttPublish);

if (mqtt.connect("your_identifier"))
{
  mqtt.publish("topic1", "hello world");
  mqtt.subscribe("topic2");
}

```