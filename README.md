
# ESP8266-Deauther 


ESP8266-Deauther is built for the ESP8266 microcontroller and programmed using the Arduino IDE. It includes functions for connecting to a wireless network, starting a server, and deauthenticating clients. Additionally, it creates a .bin file for storing the deauther script.
## Installation

Install my-project 

```bash
Download the "esp8266_deauther_davy_NODEMCU" .bin file 
```
```bash
Connect the esp8266 to your pc 
```

```bash
 Go to "esp.huhn.me" and boot up the downloaded bin file 
 ```

## Demo




## Usage/Examples


```C++
const char *ssid = "your_AP_SSID";
const char *password = "your_AP_PASSWORD";

const char *deauth_ssid = "target_AP_SSID";
const char *deauth_password = "target_AP_PASSWORD";

const char *deauth_file = "/deauther.bin";

WiFiServer server(80);
MDNSResponder mdns;

void setup() {
  Serial.begin(115200);
  delay(10);

  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());

  // Start the server
  server.begin();
  Serial.println("Server started");

  // Set up MDNS
  if (!mdns.begin("esp8266", WiFi.localIP())) {
    Serial.println("Error setting up MDNS responder!");
    while (1) {
      delay(1000);
    }
  }
  Serial.println("mDNS responder started");

  // Create deauther.bin file
  File deauth_file = SPIFFS.open(deauth_file, FILE_WRITE);
  if (!deauth_file) {
    Serial.println("Error creating deauther.bin file!");
    while (1) {
      delay(1000);
    }
  }
  deauth_file.close();

}

void loop() {
  // Handle client connections
  WiFiClient client = server.available();
  if (!client) {
    return;
  }

  // Read the request
  String request = client.readStringUntil('\r');
  client.flush();

  // Parse the request
  int idx = request.indexOf('/');
  request = request.substring(idx + 1);

  // Deauthenticate clients
  if (request == "deauth") {
    // Connect to target AP
    WiFi.begin(deauth_ssid, deauth_password);

    while (WiFi.status() != WL_CONNECTED) {
      delay(500);
      Serial.print(".");
    }

    // Get a list of clients
    IPAddress apIP = WiFi.softAPIP();
    int port = 80;
    String host = "esp8266.local";
    Serial.print("Request DHCP hostnames...");
    int n = DNS.begin(host, WiFi.softAPIP());
    Serial.print(n);
    Serial.println(" hostnames:");
    String hostname;
    for (int i = 0; i < n; ++i) {
      hostname = DNS.getHostname(i);
      Serial.println(i + ": " + hostname);
    }
    DNS.end();

    // Deauthenticate clients
    for (int i = 0; i < n; ++i) {
      String cmd = "sudo iw dev wlan0 disassoc " + String(hostname);
      Serial.println
      ```
## Sceenshots
![ESP8266-Deauther Main Menu](https://i.postimg.cc/L8156tzV/Screenshot-2024-05-14-111807.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/nzyZW5Wb/Screenshot-2024-05-14-114259.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/nzyZW5Wb/Screenshot-2024-05-14-114259.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/xTWHRT5t/Screenshot-2024-05-14-114332.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/266VYPT1/Screenshot-2024-05-14-114348.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/vHxKZhmT/Screenshot-2024-05-14-114955.png)


![ESP8266-Deauther Main Menu](https://i.postimg.cc/rFYtQGG1/Screenshot-2024-05-14-115126.png)

![ESP8266-Deauther Main Menu](https://i.postimg.cc/d3VjTsvw/Screenshot-2024-05-14-115139.png)

![ESP8266-Deauther Main Menu](https://i.postimg.cc/WbhYznXC/Screenshot-2024-05-11-221539.png)
