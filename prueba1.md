# 1. Proyecto alimentador para gatos mediante un bot de Telegram

## 1.1. Esquema

<img src="img/diagrama2.PNG"
     alt="Esquema"
     style="float: left; margin-right: 10px;" />
<br>

## 1.2. Material

* 3 x Cables macho-hembra
* 1 x Esp32
* 1 x Micro Servomotor Sg90
* Multiples cajas de carton
* Una botella de 600 ml.

## 1.3. Codigo
``` C++
#include <ESP32_Servo.h>

#include <WiFi.h>
#include <WiFiClientSecure.h>

#include <UniversalTelegramBot.h>
#include <ArduinoJson.h>

Servo myservo;  


static const int servoPin = 13;


const char* ssid     = "Alimento";
const char* password = "123456789";


#define BOTtoken "5257384733:AAE5ML6PRtaUx-YnIOyknu3ks9zzk0-aemA"  // Tu Bot Token (Obtener de Botfather)

#define CHAT_ID "1643917970"

WiFiClientSecure client;

UniversalTelegramBot bot(BOTtoken, client);

void handleNewMessages(int numNewMessages) {

  for (int i=0; i<numNewMessages; i++) {
    // Chat id of the requester
    String chat_id = String(bot.messages[i].chat_id);
    if (chat_id != CHAT_ID){
      bot.sendMessage(chat_id, "Usuario no autorizado", "");
      continue;
    }

    String text = bot.messages[i].text;

    if (text == "/comida") {
      bot.sendMessage(chat_id, "Sirviendo", "");
      myservo.write(90);             
      delay(500);                       
      myservo.write(0);              
    }
  }
}

void setup() {
  Serial.begin(115200);

  myservo.attach(servoPin); 


  Serial.print("Conectado a ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  client.setCACert(TELEGRAM_CERTIFICATE_ROOT);
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // Muestra IP local 
  Serial.println("");
  Serial.println("WiFi conectado.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());

  bot.sendMessage(CHAT_ID, "Comida", "");
}

void loop() {
  int numNewMessages = bot.getUpdates(bot.last_message_received + 1);

  while(numNewMessages) {
    handleNewMessages(numNewMessages);
    numNewMessages = bot.getUpdates(bot.last_message_received + 1);
  }
}
```

## 1.4 Materiales usados

<img src="img/esp32.jpeg"
     alt="Esquema"
     style="float: left; margin-right: 10px;" />
<br>

##

<img src="img/carcaza.jpeg"
     alt="Esquema"
     style="float: left; margin-right: 10px;" />
<br>

## 1.5 Muestra

![Alt Text](https://github.com/AlfonsoAHR/exposicion/blob/main/prueba.gif)
