# Smart Movement Alarm with Telegram Notifications

This project implements a smart movement alarm system using an ESP32 microcontroller. It leverages a PIR motion sensor, LEDs, a buzzer, and a push button for local control, while also providing remote notifications via Telegram when motion is detected.

---

## Features

* **WiFi Connectivity:** Connects to your local WiFi network for internet access.
* **NTP Time Sync:** Synchronizes time via NTP for accurate timestamping of events.
* **Motion Detection:** Monitors a PIR motion sensor for movement.
* **Telegram Notifications:** Sends real-time alert messages to Telegram when motion is detected.
* **Web Server Interface:**
    * **Arm/Disarm Control:** Toggle the alarm state remotely.
    * **Event Log:** View a list of recent motion detection events with timestamps.
* **Visual & Audio Indicators:**
    * **Green LED:** Blinks when the alarm is armed and idle.
    * **Red LED & Buzzer:** Activates (blinks/sounds) when motion is detected and the alarm is triggered.
* **Physical Button Control:** A debounced push button allows for local arming and disarming of the alarm.

---

## Hardware Requirements

* **ESP32 Development Board**
* **PIR Motion Sensor:** Connected to **GPIO 15**
* **Green LED:** Connected to **GPIO 25**
* **Red LED:** Connected to **GPIO 26**
* **Buzzer:** Connected to **GPIO 4**
* **Push Button:** Connected to **GPIO 14** (configured with internal pull-up)

---

### Wiring

| ESP32 Pin | Component Pin    | Description           |
| :-------- | :--------------- | :-------------------- |
| GPIO 15   | PIR Sensor (OUT) | Motion sensor input   |
| GPIO 25   | Green LED (+)    | Green LED control     |
| GPIO 26   | Red LED (+)      | Red LED control       |
| GPIO 4    | Buzzer (+)       | Buzzer control        |
| GPIO 14   | Push Button      | Alarm toggle button   |
| GND       | All Components   | Common ground         |
| 3.3V      | PIR Sensor (VCC) | PIR sensor power      |

---

## Software Requirements

* **Arduino IDE:** With ESP32 board support installed.
* **Arduino Libraries:**
    * `WiFi.h`
    * `WebServer.h`
    * `HTTPClient.h`
    * `time.h`

---

## Setup Instructions

1.  **Clone or Download:** Obtain the project code from this repository.
2.  **Update WiFi Credentials:** Open the project in Arduino IDE and modify the following lines with your network details:

    ```cpp
    const char* ssid = "YOUR_WIFI_SSID";
    const char* password = "YOUR_WIFI_PASSWORD";
    ```

3.  **Set Telegram Bot Token and Chat ID:**
    * **Create a Telegram Bot:** Talk to `@BotFather` on Telegram and follow the instructions to create a new bot. You will receive a **Bot Token**.
    * **Get your Chat ID:** Send any message to your newly created bot. Then, open your web browser and visit:

        ```bash
        [https://api.telegram.org/bot](https://api.telegram.org/bot)<YourBotToken>/getUpdates
        ```
        Replace `<YourBotToken>` with the token you obtained from `@BotFather`. Look for `"id"` under the `"chat"` object in the JSON response; this is your `chatID`.
    * **Update Code:** In your Arduino sketch, set these values:

        ```cpp
        const char* telegramBotToken = "YOUR_BOT_TOKEN";
        const char* chatID = "YOUR_CHAT_ID";
        ```

4.  **Upload to ESP32:** Compile and upload the code to your ESP32 board using the Arduino IDE.
5.  **Monitor Serial Output:** Open the **Serial Monitor** in Arduino IDE (set to 115200 baud). It will display the ESP32's connection status and the assigned IP address.
6.  **Access Web Interface:** Open a web browser on a device connected to the same network and navigate to the IP address shown in the Serial Monitor (e.g., `http://192.168.x.x`). You can now arm/disarm the alarm and view events from this page.

---

## How It Works

The ESP32 initiates by connecting to your specified WiFi network and synchronizing its internal clock with an NTP server.

Once connected and the alarm is armed (either via the web interface or the physical button), the system continuously monitors the PIR sensor for motion.

Upon detecting motion, the alarm triggers:
* The **Red LED** will blink, and the **buzzer** will sound to provide immediate local alerts.
* A timestamped alert message is sent to your configured Telegram chat.

When the system is armed but no motion is detected, the **Green LED** blinks periodically, indicating that the system is active but idle.

The alarm's armed/disarmed state can be toggled conveniently through both the dedicated web interface and the physical push button connected to the ESP32.

---

## Troubleshooting

* **No Telegram messages received:**
    * Double-check that your `telegramBotToken` and `chatID` are entered correctly in the code.
    * Manually test sending a message using the Telegram Bot API URL in a web browser to ensure your token is valid and the chat ID is accessible by your bot.
    * Verify that your ESP32 is successfully connected to your WiFi network.
* **Web server not accessible:**
    * Confirm that the ESP32 is connected to WiFi and note the IP address displayed in the Serial Monitor.
    * Ensure that no firewall or router settings are blocking access to the ESP32's IP address on your local network.
* **Time not syncing:**
    * Verify the NTP server address configured in the code.
    * Check your network connection to ensure the ESP32 has internet access to reach the NTP server.

---

## Acknowledgments

* Thanks to the ESP32 community and Arduino developers for their extensive libraries and support.
* Telegram Bot API documentation: [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

---

Feel free to contribute to this project by opening issues for bugs or suggesting new features!
