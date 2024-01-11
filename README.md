# Modbus Setup

<p style="text-align: right;">Last Updated: Jan. 11, 2024</p>

### Table of Contents

- [Modbus ID Range](#modbus-id-range)
- [MBControl Default](#mbcontrol-default)
- [EKO Pyranometer MS-80SH](#eko-pyranometer-ms-80sh)
- [MBControl Solar Irradiance Sensor MBMet-500-AB](#mbcontrol-solar-irradiance-sensor-mbmet-500-ab)
- [MBControl PV Module Temperature Sensor MBMet-830](#mbcontrol-pv-module-temperature-sensor-mbmet-830)
- [MBControl Wind Speed Sensor MBMet-100B-B](#mbcontrol-wind-speed-sensor-mbmet-100b-b)
- [PUSR Default](#pusr-default)
- [GA Default](#ga-default)
- [Renke Default](#renke-default)
- [Renke Pyranometer RS-TBQ-N01-AL](#renke-pyranometer-rs-tbq-n01-al)
- [Renke Wind Direction](#renke-wind-direction)
- [Renke Wind Speed](#renke-wind-speed)
- [Renke Ambient Temperature](#renke-ambient-temperature)
- [Renke Weather Station](#renke-weather-station)
- [Renke 4-20mA RS485](#renke-4-20ma-rs485)

---

# Modbus ID Range
### Total 1 ~ 247 (0x01 ~ 0xF7)
 - [01 ~ 30 (0x01 ~ 0x1E)] PV Inverter
 - [31 ~ 60 (0x1F ~ 0x3C] PV Temperature
 - [61 (0x3D)] GHI : Renke Pyranometer RS-TBQ-N01-AL
 - [62 (0x3E)] GHI : EKO Pyranometer MS-80SH 
 - [71 ~ 80 (0x47 ~ 0x50)] POA
 - 
 - [90 ~ 100 (0x5A ~ 0x64)] Structure Sensor
 - 
 - [200 (0xC8)] Wind Direction
 - [201 (0xC9)] Wind Speed
 - [202 (0xCA)] Ambient Temperature 

---

# MBControl Default

### Wire (LAPP 1032100)
<pre>
	White	Vcc
	Brown	GND
	Green	A+
	Yellow  B-
</pre>

### Modbus Parameters

Use Function 16 (0x10) (Write Multiple Registers) to Write and Save parameters. Once rebooting it takes effect.

<Pre>
Address 100 (0x0064) [U16] Modbus ID 1 ~ 247
Address 101 (0x0065) [U16] Baud rate: 0=4800, 1=9600, 2=19200
Address 102 (0x0066) [U16] Parity: 0=None, 1=Odd, 2=Even
Address 103 (0x0067) [U16] Stop bits: 1 (Fix)
Address 104 (0x0068) [U16] Temperature Unit: 0=°C, 1=°K, 2=°F
Address 105 (0x0069) [U16] Save Parameters: 1, Write Only
</pre>

- Example: ID=71(0x47), Baud=9600, No Parity, 1 Stop bit, °C
<pre>
  Send: 06 10 0064 0006 0C 0047 0001 0000 0001 0000 0001 [CRC]
  Receive: 06 10 00 64 00 06 [CRC]
</pre>

---

# EKO Pyranometer MS-80SH
GHI measurement. Set the Modbus ID and RS485 parameters in Hibi software supplied by EKO.

- Address 2, 3 : [F32] Adjusted solar radiation intensity (W/m²)
- Example: 3.6 W/m² (0x4064959C = 3.57163)
<pre>
  Send: 3E 03 0002 0002 [CRC]
  Receive: 3E 03 04 40 64 95 9C [CRC]
</pre>

---

# MBControl Solar Irradiance Sensor MBMet-500-AB
POA measurement.

- Address 0 : [U16] Solar irradiation (W/m²)
- Example: 7 W/m² (0x0007 = 7)
<pre>
  Send: 47 03 0000 0001 [CRC]
  Receive: 47 03 02 00 07 [CRC]
</pre>

---

# MBControl PV Module Temperature Sensor MBMet-830
PV Module Temperature measurement.

- Address 0 : [S16] 10x Temperature (°C)
- Example: 24.9°C (0x00F9 = 249)
<pre>
  Send: 1F 03 0000 0001 [CRC]
  Receive: 1F 03 02 00 F9 [CRC]
</pre>

---

# MBControl Wind Speed Sensor MBMet-100B-B
Wind Speed measurement.

- Address 0 : [U16] 10x Wind Speed (m/s)
- Example: 3.2 m/s (0x0020 = 32)
<pre>
  Send: 01 03 0000 0001 [CRC]
  Receive: 01 03 02 00 20 [CRC]
</pre>

---

# PUSR Default

3.81mm pitch 5P

![Local Image](images/usr_rtu_tb.png)

---

# GA Default

3.50mm pitch 5P

![Local Image](images/ga_tb.png)

---

# Renke Default
### 3 Way Cable:  
>    https://youtu.be/ZsB1V8zTqgs?t=33

### DC Power: 12V (10 ~ 30V dc)

### Wire
<pre>
	Brown	Vcc
	Black	GND
	Yellow	A+
	Blue    B-
</pre>

### Modbus-RTU
<pre>
  Default speed   4800bps
  Default address 0x01
</pre>

### Default Command

#### Version Number
- Address: 0x0009
- Function: 0x03
- Example: V2.04
<pre>
  Send: 01 03 0009 0001 [CRC]
  Receive: 01 03 02 02 04 [CRC]
</pre>

#### Modbus ID
- Address: 0x07D0
- Function: 0x06 / 0x03
- Example: ID = 0x00C8
<pre>
  Send: C8 06 07D0 00C8 [CRC]
  Receive: C8 06 07 D0 00 C8 [CRC] // Echo
</pre>

#### Query Modbus ID (0xFF)
- Address: 0x07D0
- Function: 0x03
- Example: ID = 0x003D
<pre>
  Send: FF 03 07D0 0001 [CRC]
  Receive: 3D 03 02 00 3D [CRC]
</pre>

#### Baud Rate 
- Address: 0x07D1
- Function: 0x06 / 0x03
- Content:
<pre><code> baud rate: 
	    0 – 2400bps
	    1 – 4800bps
	    2 – 9600 bps
	    3 – 19200bps 
</code></pre>

- Example: Baud = 9600 bps
<pre>
  Send: C8 06 07D1 0002 [CRC]
  Receive: C8 06 07 D1 00 02 [CRC] // Echo
</pre>

---

# Renke Pyranometer RS-TBQ-N01-AL
GHI measurement.

- Address 0 : [U16] Solar radiation intensity (W/m²)
- Example: 16 W/m² (0x0010 = 16)
<pre>
  Send: 3D 03 0000 0001 [CRC]
  Receive: 3D 03 02 00 10 [CRC]
</pre>

---

# Renke Wind Direction

- Address 0 : [U16] Wind direction 0 ~ 7
- Address 1 : [U16] Wind direction 0 ~ 360°

---

# Renke Wind Speed

- Address 0 : [U16] 10x Wind Speed (m/s)
- Example: Wind Speed = 2.9 m/s (0x1D = 29)
<pre>
  Send: C9 03 0000 0001 [CRC]
  Receive: C9 03 02 00 1D [CRC]
</pre>

---

# Renke Ambient Temperature

- Address 505 (0x01F9) : [S16] 10x Ambient Temperature
- Example: Ambient Temperature = 22.4°C (0xE0 = 224)
<pre>
  Send: CA 03 01 F9 00 01 [CRC] 
  Receive: CA 03 02 00 E0 [CRC]
</pre>
- Example: Ambient Temperature = -10.1°C (0xFF9B = -101)
<pre>
  Send: CA 03 01 F9 00 01 [CRC] 
  Receive: CA 03 02 FF 9B [CRC]
</pre> 

---

# Renke Weather Station

### Wind Speed
- Modbus ID: 01 (*)
- Baud: 4800 (*)

### Wind Direction
- Modbus ID: 02 (*)
- Baud: 4800 (*)

### Base Station
- Modbus ID: 01 --> 202 (0xCA)
- Baud: 4800 --> 9600

### Address Map
- 500 (0x1F4) : [U16] 10x Wind Speed
- 502 (0x1F6) : [U16] Wind Direction (0\~7)
- 503 (0x1F7) : [U16] Wind Direction (0\~360°)
- 504 (0x1F8) : [U16] 10x Humidity
- 505 (0x1F9) : [S16] 10x Ambient Temperature (°C)

---

# Renke 4-20mA RS485

- I/O : 4\~20mA / 655\~3276 (12bit)
- Address 00 : CH1
- Address 01 : CH2
- Example: Read CH1 (0x78A = 1930)
<pre>
  Send: 5A 03 00 00 00 01 [CRC] 
  Receive: 5A 03 02 07 8A [CRC] 
</pre>

---
