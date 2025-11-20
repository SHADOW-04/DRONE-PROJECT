🚁 AeroFold — Portable Carbon-Frame Quadcopter

Foldable • Lightweight • Arduino-Driven • Budget-Optimized

AeroFold is a compact, foldable quadcopter designed with durability, hackability, and minimal cost in mind.
The drone runs on a DIY Arduino Nano–based flight controller, uses the MPU6050 IMU, and is controlled through an EvoFox One S controller via Bluetooth (HC-05) or 2.4GHz.

This repo hosts:

🌐 The Intro Website (index.html + style.css)

⚙️ Firmware skeleton

🧩 Wiring & build docs

📸 Gallery & media assets

🧱 BOM + specs



---

🔗 Live Preview

(After you enable GitHub Pages)
👉 https://your-username.github.io/your-repo-name/


---

✨ Project Overview

AeroFold aims to be:

Portable: Foldable carbon-fiber ZMR250/QAV250-style frame

Durable: Carbon nylon props, 30A SimonK ESCs

DIY-friendly: Arduino Nano flight controller

Budget-build: All components sourced from Amazon / Robu.in

Beginner-safe: Bench-tested firmware scaffolding, easy to extend



---

📦 Bill of Materials (BOM)

Component	Details

Frame	ZMR250 / QAV250 (with foldable mod)
Motors	DX2205 2300KV × 4
ESCs	SimonK 30A × 4
Propellers	5045 carbon nylon (2× CW, 2× CCW)
Battery	3S 11.1V 1500mAh LiPo
Flight Controller	Arduino Nano (CH340) + MPU6050 (10DOF module)
Controller	EvoFox One S Universal Wireless
Communication	HC-05 Bluetooth / 2.4GHz dongle
Power	XT60 connectors, PDB board, L4941BV regulator
Tools	Soldering kit, multimeter, hex drivers



---

⚡ Wiring Summary

Battery (XT60) → PDB
ESC power → PDB
ESC signal wires → Arduino PWM pins (D3, D5, D6, D9 recommended)
Common ground → EVERY module

MPU6050:
  SDA → A4
  SCL → A5
  VCC → 5V / 3.3V
  GND → GND

HC-05 Bluetooth:
  TX → RX
  RX → TX
  (or use SoftwareSerial on pins like D10/D11)

Tip: Keep all grounds shared to avoid motor jitter & resets.
Always program & test with props removed.


---

🧠 Flight Mixing Logic

The quad uses standard X-layout mixing:

M1 (Front Right)  = Throttle + Pitch - Roll - Yaw
M2 (Front Left)   = Throttle + Pitch + Roll + Yaw
M3 (Rear Left)    = Throttle - Pitch + Roll - Yaw
M4 (Rear Right)   = Throttle - Pitch - Roll + Yaw

This is implemented in the included Arduino firmware skeleton.


---

🛠️ Firmware (Arduino Nano)

// flight_controller.ino (skeleton)
// Simplified — add PID, filters, arming, failsafe before flying.

#include <Wire.h>
#include <MPU6050.h>
#include <Servo.h>
#include <SoftwareSerial.h>

MPU6050 imu;
Servo esc1, esc2, esc3, esc4;
SoftwareSerial bt(10, 11); // RX, TX

void setup() {
  bt.begin(9600);
  Wire.begin();
  imu.initialize();

  esc1.attach(3);
  esc2.attach(5);
  esc3.attach(6);
  esc4.attach(9);

  esc1.writeMicroseconds(1000);
  esc2.writeMicroseconds(1000);
  esc3.writeMicroseconds(1000);
  esc4.writeMicroseconds(1000);
}

void loop() {
  int throttle = 1200;
  int roll = 0, pitch = 0, yaw = 0;

  int m1 = throttle + pitch - roll - yaw;
  int m2 = throttle + pitch + roll + yaw;
  int m3 = throttle - pitch + roll - yaw;
  int m4 = throttle - pitch - roll + yaw;

  esc1.writeMicroseconds(constrain(m1,1000,2000));
  esc2.writeMicroseconds(constrain(m2,1000,2000));
  esc3.writeMicroseconds(constrain(m3,1000,2000));
  esc4.writeMicroseconds(constrain(m4,1000,2000));
}


---

🧪 Build Steps

1. Assemble the frame and mount all motors


2. Wire ESCs → PDB → Battery


3. Mount Arduino + MPU6050 on anti-vibration foam


4. Connect ESC signal wires to PWM pins


5. Pair EvoFox controller with HC-05


6. Run bench tests with props removed


7. Add PID tuning + IMU filtering


8. Perform first lift in open, safe area




---

🖼️ Gallery

Place your drone photos here (stored in /assets/images/):

/assets/images
   ├── drone-top.jpg
   ├── drone-side.jpg
   ├── wiring-closeup.jpg
   ├── bench-test.jpg

Add them in markdown like:

![AeroFold Top View](assets/images/drone-top.jpg)


---

🌐 Website Structure

/
├── index.html
├── style.css
├── /assets
│   └── /images
│       └── (...your drone photos)
├── /firmware
│   └── flight_controller.ino
└── /docs
    └── wiring-diagram.png


---

🧾 Status

> 🚧 Prototype — Wiring complete, firmware in progress, IMU not mounted yet.




---

📬 Contact

Built by Your Name / GitHub Username
Feel free to open PRs, discussions, or ideas to improve the project.


---

⚖️ License

MIT — free to modify, fork, remix, and build on.


---

❤️ Support

If you like this project, consider 🌟 starring the repo on GitHub!
