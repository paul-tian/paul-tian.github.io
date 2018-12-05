---
layout: default
---

# Insert data into Database from an Arduino Board

Same as the [browser side](/notes/browser_get_and_post), here the Arduino board also controls the ```'demo'.'user'``` table which created [here](/notes/tomcat_and_mysql).

## Step 1 What did the Browser Really do when Sending POST Request

The codes [here](/notes/browser_get_and_post) indicate that if the browser use ```GET``` method, it needs only a simple URL with values in it:

```html
http://localhost:8080/test/op.jsp?getid=255&getname=Browser&getpassword=MySQL
```
However if the ```POST``` method is used, these values are not visible(actually, not intuitive), we need some tools to find out what the browser really send to the server.

Tools are many, like [SIDT](https://sidt.ai)(a light-weighted tool) or [WireShark](https://www.wireshark.org)(a fully loaded heavy weapon), or even paid tools like [Charles](https://www.charlesproxy.com). You can choose any of it, as far as it can capture packet send from browser(SIDT is an exception, it's a HTTP request tool, which can make a HTTP request yourself, but this needs knowledge and experience).

Here is an example that captured by [WireShark](https://www.wireshark.org) when the browser send the ```POST``` request(reassembled TCP contents and modified for reading):

```html
POST /test/op-redir.jsp HTTP/1.1(CRLF)
Host: 127.0.0.1:8080(CRLF)
Accept: text/html,application/xhtml+xml,application/xml;
q=0.9,*/*;q=0.8(CRLF)
Accept-Language: en-us(CRLF)
Accept-Encoding: gzip, deflate(CRLF)
Content-Type: application/x-www-form-urlencoded(CRLF)
Origin: http://127.0.0.1:8080(CRLF)
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_1) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.1 Safari/605.1.15(CRLF)
Referer: http://127.0.0.1:8080/test/main.jsp(CRLF)
Upgrade-Insecure-Requests: 1(CRLF)
DNT: 1(CRLF)
Content-Length: 34(CRLF)
Cookie: JSESSIONID=AACF253ABB16227CA93297A579AA44C4(CRLF)
Connection: keep-alive(CRLF)
(CRLF)
getid=88&getname=88&getpassword=88
```

These contents revealed what the browser really did when we click the ```add``` button on ```main.jsp```. It constructed a [HTTP message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages) which told the server to consider it as a request and contained our data at the tail of this message.

## Step 2 Form a HTTP GET/POST Message on Arduino

With the understanding of HTTP message, it is now possible to construt one on Arduino.

After modified the official example for[MKR1000](https://www.arduino.cc/en/Tutorial/Wifi101WiFiWebClientRepeating) and [MKR WiFi 1010](https://www.arduino.cc/en/Tutorial/WiFiNINAWiFiWebClientRepeating), which can send a get request, the following codes can send ```POST``` request to insert database(```GET``` method could be found in comments):

```C
#include <SPI.h>
#include <WiFiNINA.h> // for MKR WiFi 1010
// #include <WiFi101.h> // for MKR1000
#include "arduino_secrets.h"
///////please enter your sensitive data in the Secret tab/arduino_secrets.h
char ssid[] = SECRET_SSID;  // your network SSID (name)
char pass[] =
    SECRET_PASS;   // your network password (use for WPA, or use as key for WEP)
int keyIndex = 0;  // your network key Index number (needed only for WEP)

int status = WL_IDLE_STATUS;

// Initialize the Wifi client library
WiFiClient client;

// server address:
IPAddress server(W, X, Y, Z); // server IP
unsigned long lastConnectionTime =
    0;  // last time you connected to the server, in milliseconds
const unsigned long postingInterval =
    10L * 10L;  // delay between updates, in milliseconds

long baseid = 4;
char basename[] = "paul";
char basepass[] = "luap";

void setup() {
  // Initialize serial and wait for port to open:
  Serial.begin(9600);
  while (!Serial) {
    ;  // wait for serial port to connect. Needed for native USB port only
  }

  // check for the WiFi hardware:
  if (WiFi.status() == WL_NO_MODULE) { // for MKR WiFi 1010
    Serial.println("WiFi hardware problem!");
    // don't continue
    while (true)
      ;
  }

  // if (WiFi.status() == WL_NO_SHIELD) { // for MKR1000
  //   Serial.println("WiFi hardware problem!");
  //   while (true)
  //     ;
  // }

  // attempt to connect to Wifi network:
  while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP
    // network:
    status = WiFi.begin(ssid, pass);

    // wait 5 seconds for connection:
    delay(3000);
  }
  // you're connected now, so print out the status:
  printWiFiStatus();
}

void loop() {
  // if there's incoming data from the net connection.
  // send it out the serial port.  This is for debugging
  // purposes only:
  while (client.available()) {
    char c = client.read();
    Serial.write(c);
  }

  // if interval time have passed since your last connection,
  // then connect again and send data:

  if (millis() - lastConnectionTime > postingInterval) {
    httpRequest();
  }
}

// this method makes a HTTP connection to the server:
void httpRequest() {
  // close any connection before send a new request.
  // This will free the socket on the Nina module
  client.stop();

  // forming entity
  String postName = String(basename + String(baseid));
  String postPass = String(basepass + String(baseid));
  String postEntity =
      String("getid=" + String(baseid) + "&getname=" + postName +
             "&getpassword=" + postPass);
  Serial.println(postEntity.length());
  Serial.println(postEntity);
  baseid++;

  // if there's a successful connection:
  if (client.connect(server, 8080)) {
    Serial.println("connecting...");

    // send the GET request: --------------------------------------------------
    //    client.println("GET /test/op.jsp?getid=66&getname=7&getpassword=8
    //    HTTP/1.1"); client.print("Host: "); client.println(server);
    //    client.println("User-Agent: ArduinoWiFi/1.1");
    //    client.println("Connection: close");
    //    client.println();
    // ------------------------------------------------------------------------

    // send the POST request: -------------------------------------------------
    client.println("POST /test/op.jsp HTTP/1.1");
    client.print("Host: ");
    client.println(server);
    client.println("User-Agent: ArduinoWiFi/1.1");
    client.println("Connection: close");
    client.println("Content-Type: application/x-www-form-urlencoded;");
    client.print("Content-Length: ");
    client.println(postEntity.length());
    client.println();
    client.println(postEntity);
    // ------------------------------------------------------------------------

    // note the time that the connection was made:
    lastConnectionTime = millis();
  } else {
    // if you couldn't make a connection:
    Serial.println("connection failed");
  }
}

void printWiFiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your board's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print your subnet mask:
  IPAddress subnet = WiFi.subnetMask();
  Serial.print("NETMASK: ");
  Serial.println();

  // print your gateway address:
  IPAddress gateway = WiFi.gatewayIP();
  Serial.print("GATEWAY: ");
  Serial.println(gateway);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}
```

After burning the codes into the Arduino, the board should now possible to fill the database with (id, name, password) values (4, paul4, luap4), (5, paul5, luap5), etc.

## Step 3 Summary

The most important step is to figure out what the browser really send, or on the other hand, what does the server need to fulfill the demand of inserting data into database. After that, the following steps would be as easy as eating a pie.  
Actually I didn't capture browser packet at first, so I missed the line:

```C
client.println("Content-Type: application/x-www-form-urlencoded;");
```

which cost me a lot of time to find out the cause of error.

[homepage](/)
