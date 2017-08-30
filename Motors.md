Motors

motor A 50% 0.1s
0c:00:81:37:11:09:64:00:32:64:7f:03

gatttool -b 00:16:53:A4:CD:7E --char-write-req --handle=0x0e --value=0c0081371109640032647f03

motor A 100% 0.1s
0c:00:81:37:11:09:64:00:64:64:7f:03

motor B 50% 0.1s
0c:00:81:38:11:09:64:00:32:64:7f:03

motor B 50% 0.2s
0c:00:81:38:11:09:c8:00:32:64:7f:03

So...

4th byte is motor:
A = 37
B = 38

5th and 6th byte remain unknown

7h and 8th byte is timing (LSB + MSB)
0.1s = 64 00
0.2s = c8 00

64h = 100d
c8h = 200d
So time is in mil 647f03liseconds
00 0A = 2560 = 2.56 seconds
00 0F = 3.84 seconds

9th byte is duty cycle:
 50% = 32
100% = 64

Duty Cycle also controls the direction:
50% Clockwise = 50d = 32h
100% Clockwise = 100d = 64h
50% Counterclockwise = 255d-50d = 205d = CDh
100% Counterclockwise = 255d-100d = 155d = 9Bh

10th, 11th and 12th bytes remain unknown

motor A+B 50% 0.1s (not sure, screen too small)
0d:01:81:39:11:0a:64:00:32:32:64:7f:03

So...
39 is A+B

same 0c:00:81 command with 39 also works (that is why I'm not so sure)
So...

First 3 bytes specify the command:
Run Motor A at x duty cycle for y time = 0c:00:81

4th byte specify the motors affected:
  A = 37
  B = 38
  C = 01 (but 36 also confirmed)
  D = 02
A+B = 39

Not sure yet:
C 50 0.1
0c:00:81:01:11:09:64:00:32:64:7f:03
C 50 360º
0e:00:81:01:11:0b:68:01:00:00:32:64:7f:03
D 50 360º
0e:00:81:02:11:0b:68:01:00:00:32:64:7f:03

First byte seems to be the message length

68:01 = 360º (LSB + MSB)

0c0081 M 11 09 T DT 647f03 = Turn motor M (1 Byte) with duty cycle DT (1 Byte) for T (2 Bytes) milliseconds
0c0081 M 11 0b DEG DT 647f03 = Rotate motor M (1 Byte) DEG (4 Bytes) degrees with duty cycle DT (1 Byte)

5A 00 00 00 = 90º
B4 00 00 00 = 180º
0E 01 00 00 = 270º
68 01 00 00 = 360º
10 0E 00 00 = 3600°
40 19 01 00 = 72000°
