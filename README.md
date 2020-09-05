# MT07-OBDII

Using OBDII_PIDs example from https://github.com/Seeed-Studio/CAN_BUS_Shield and the Leonardo CAN-BUS from https://www.hobbytronics.co.uk/leonardo-canbus I was able to hack into my Yamaha MT07 OBD-II data.

```
SEND PID: 0x0

------------------------------------------------------------------
Get Data From id: 0x7E8
0x6	0x41	0x0	0xBA	0x3E	0xB0	0x1	0x55	

SEND PID: 0x40

------------------------------------------------------------------
Get Data From id: 0x7E8
0x6	0x41	0x40	0x48	0x8	0x0	0x0	0x55	

SEND PID: 0x60
SEND PID: 0x80
SEND PID: 0xA0
```

According to https://es.wikipedia.org/wiki/OBD-II_PID, 

PID 00 reports PIDs supported [01 - 20], out response was: 0xBA	0x3E	0xB0	0x1, so lets decode it,

### 0xBA (10111010):

| Hexadecimal | B   |    |     |     | A   |    |     |    |
|-------------|-----|----|-----|-----|-----|----|-----|----|
| Binary      | 1   | 0  | 1   | 1   | 1   | 0  | 1   | 0  |
| Supported?  | Yes | No | Yes | Yes | Yes | No | Yes | No |
| PID number  | 01  | 02 | 03  | 04  | 05  | 06 | 07  | 08 |

| Supported PID | Meaning                    |
|:-------------:|----------------------------|
|       01      | Monitor status             |
|       03      | Fuel system status         |
|       04      | Calculated engine load     |
|       05      | Engine coolant temperature |
|       07      | Long term fuel trim—Bank 1 |

### 0x3E (111110):

| Hexadecimal | 3   |     |     |     | E   |     |     |    |
|-------------|-----|-----|-----|-----|-----|-----|-----|----|
| Binary      | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 0  |
| Supported?  | Yes | Yes | Yes | Yes | Yes | Yes | Yes | No |
| PID number  | 09  | 0A  | 0B  | 0C  | 0D  | 0E  | 0F  | 10 |

| Supported PID | Meaning                           |
|---------------|-----------------------------------|
| 09            | Long term fuel trim—Bank 2        |
| 0A            | Fuel pressure                     |
| 0B            | Intake manifold absolute pressure |
| 0C            | Engine speed                      |
| 0D            | Long term fuel trim—Bank 1        |
| 0E            | Vehicle speed                     |
| 0F            | Timing advance                    |

### 0xB0 (10110000):

| Hexadecimal | B   |    |     |     | 0  |    |    |    |
|-------------|-----|----|-----|-----|----|----|----|----|
| Binary      | 1   | 0  | 1   | 1   | 0  | 0  | 0  | 0  |
| Supported?  | Yes | No | Yes | Yes | No | No | No | No |
| PID number  | 11  | 12 | 13  | 14  | 15 | 16 | 17 | 18 |

| Supported PID | Meaning                           |
|---------------|-----------------------------------|
| 11            | Long term fuel trim—Bank 2        |
| 13            | Oxygen sensors present            |
| 14            | Intake manifold absolute pressure |

### 0x1 (00010000):

| Hexadecimal | 1  |    |    |     | 0  |    |    |    |
|-------------|----|----|----|-----|----|----|----|----|
| Binary      | 0  | 0  | 0  | 1   | 0  | 0  | 0  | 0  |
| Supported?  | No | No | No | Yes | No | No | No | No |
| PID number  | 19 | 1A | 1B | 1C  | 1D | 1E | 1F | 20 |

| Supported PID | Meaning                                |
|---------------|----------------------------------------|
| 1C            | OBD standards this vehicle conforms to |

