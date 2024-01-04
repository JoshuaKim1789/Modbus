<span style="font-size:2em; font-weight: bold;">Modbus Setup</span>

Last modified: Jan. 04, 2024

# Modbus Address Range
 - Total 1 ~ 247 (0x01 ~ 0xF7)
 - [01 ~ 30 (0x01 ~ 0x1E)] PV Inverter
 - [31 ~ 60 (0x1F ~ 0x3C] PV Temperature
 - [61 (0x3D)] GHI
 - [71 ~ 80 (0x47 ~ 0x50)] POA
 - 
 - [90 ~ 100 (0x5A ~ 0x64)] Structure Sensor
 - 
 - [200 (0xC8)] Wind Direction
 - [201 (0xC9)] Wind Speed
 - [202 (0xCA)] Ambient Temperature 

# Renke Default
- 3 Way Cable:  
    https://youtu.be/ZsB1V8zTqgs?t=33

- DC Power: 12V (10 ~ 30V dc)
- Wire
    <pre>
	Brown	Vcc
	Black	GND
	Green	A+
	Blue   	B-
    </pre>

- Modbus-RTU
    <pre>
    Default speed 	4800bps
    Default address 0x01
    </pre>

- Default Command
> - Version Number
	- Regiser Address: 0x0009
	- Function: 0x03
	- Example: V2.04
	<pre>
	  Send: 01 03 0009 0001 <CRC>
      Receive: 01 03 02 02 04 <CRC>
    </pre>
> - Modbus Address
    - Register Address: 0x07D0
    - Function: 0x06 / 0x03
    - Example: Address = 0x00C8
	<pre>
	  Send: C8 06 07D0 00C8 <CRC>
      Receive: C8 06 07 D0 00 C8 <CRC> // Echo
    </pre>
> - Baud Rate 
    - Register Address: 0x07D1
    - Function: 0x06 / 0x03
    - Content:
>      <pre><code> baud rate: 
	    0 – 2400bps
	    1 – 4800bps
	    2 – 9600 bps
	    3 – 19200bps 
      </code></pre>
     - Example: Baud = 9600 bps
	  <pre>
	    Send: C8 06 07D1 0002 <CRC>
        Receive: C8 06 07 D1 00 02 <CRC> // Echo
      </pre>

---
