# Smoke-Detector
#include <ESP8266WiFi.h>
#include <ESP8266HTTPClient.h>
#include <WiFiClientSecureBearSSL.h>

const char* ssid = " Your Wifi name ";
const char* password = "password";

String botToken = "8275248491:AAF6PaBh6Uf6Lj3_uR4nS_T6r6HmAY-xHb8";
String chatID = "8076040012";

const int mq2Pin = A0;
int threshold = 10;

WiFiClientSecure client;
int previousState = 0;

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");
  while(WiFi.status() != WL_CONNECTED){
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nWiFi connected!");
  client.setInsecure(); // ปิดตรวจสอบ SSL
}

void loop() {
  int sensorValue = analogRead(mq2Pin);
  Serial.print("Sensor Value: ");
  Serial.println(sensorValue);

  if(sensorValue < threshold && previousState == 0){
    Serial.println("Detected smoke! Sending Telegram...");
    sendTelegramMessage("Smoke detected!"); // ข้อความง่าย ๆ
    previousState = 1;
  } else if(sensorValue >= threshold && previousState == 1){
    previousState = 0;
  }

  delay(1000);
}

void sendTelegramMessage(String message){
  if(WiFi.status() == WL_CONNECTED){
    HTTPClient http;
    String url = "https://api.telegram.org/bot" + botToken + "/sendMessage?chat_id=" + chatID + "&text=" + message;
    Serial.println(url);

    http.begin(client, url);
    int httpCode = http.GET();
    Serial.print("HTTP response code: ");
    Serial.println(httpCode);
    http.end();
  } else {
    Serial.println("WiFi not connected");
  }
}
