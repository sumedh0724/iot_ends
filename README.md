# iot_ends

Experiment: RFID-Based Smart Attendance System (PulseRoll)

Aim
To build an RFID attendance system using an Arduino Mega 2560 and an RC522 reader
that records each card tap, stores it in a database and calculates each student's
attendance percentage against the 75% rule.

Components Required
1. Arduino Mega 2560
2. RC522 RFID reader module (13.56 MHS)
3. MIFARE Classic 1K RFID cards
4. Laptop with Python 3 (pyserial, Bottle, SQLite)
5. USB cable, jumper wires, breadboard
   
Connections (RC522 → Mega): SDA → D10, SCK → D52, MOSI → D51, MISO → D50, RST →
D9, 3.3V → 3.3V, GND → GND

Block Diagram
flowchart LR
A[RFID Card] -->|RF| B[RC522 Reader]
B -->|SPI| C[Arduino Mega]
C -->|USB Serial| D[Laptop: Python + SQLite]
D --> E[Web Dashboard]

Method
1. Connect the RC522 to the Arduino Mega as shown above (use 3.3 V, not 5 V).
2. Upload the firmware that reads the card UID and sends it over serial at 9600 baud.
3. Run the Python server, which reads the UID from the serial port.
4. The server marks the tap as IN or OUT, on time or late, and saves it in SQLite.
5. Attendance % and the 75% prediction are calculated from the saved taps.
6. Open the dashboard in a browser and tap cards to see live updates.

Pseudo Code
ARDUINO:
start SPI and RC522
loop:
if new card present:
read UID
send UID over serial
wait 1 second, halt card
SERVER:
loop:
uid = read line from serial
if uid not registered: raise alert
else if same card within 5 s: ignore
else if first tap today: save IN (late if after 09:10)
else: save OUT
update dashboards
PREDICTOR:
percent = attended / total * 100
can_miss = floor(attended / 0.75 - total)

Result
The RFID attendance system was successfully built and tested. Each card tap was
recorded in real time, unknown cards and duplicate taps were handled correctly, and the
dashboard showed every student's attendance percentage and how many more days they
could miss while staying above 75%.
