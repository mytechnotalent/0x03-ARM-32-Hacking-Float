![image](https://github.com/mytechnotalent/0x02_arm_32_hacking_float/blob/main/RPI32AAHF.png?raw=true)

## FREE Reverse Engineering Self-Study Course [HERE](https://github.com/mytechnotalent/Reverse-Engineering-Tutorial)

<br>

# 0x03_arm_32_hacking_float
ARM 32-bit Raspberry Pi Hacking Float example in Kali Linux.

<br>

## Join DC540 Discord [HERE](https://discord.gg/TC9V9RCr5U)

<br>

# Schematic
![image](https://github.com/mytechnotalent/0x02_arm_32_hacking_float/blob/main/schematic.png?raw=true)

<br>

# Parts
[Raspberry Pi 4](https://www.adafruit.com/product/4292)<br>
[64GB Micro SD Card](https://www.amazon.com/SDSDQUA-064G-A11-Professional-MicroSDXC-formatted-recording/dp/106171327X)<br>
[Micro SD Card Reader/Writer](https://www.amazon.com/uni-Adapter-Supports-Compatible-MacBook/dp/B081VHSB2V)

<br>

# STEP 1: Download Kali Linux ARM Image - Raspberry Pi 32-bit
[Download](https://images.kali.org/arm-images/kali-linux-2020.4-rpi4-nexmon.img.xz) [https://www.offensive-security.com/kali-linux-arm-images/]

# STEP 2: Download balenaEtcher
[Download](https://www.balena.io/etcher)

# STEP 3: Flash Kali Linux ARM Image
[Watch YT Null Byte Video](https://www.youtube.com/watch?v=Jquf9BDm4iU&t=493s)

# STEP 4: Power Up RPI & Login
***POWER UP DEVICE AND LOGIN AS KALI AND SET UP SSH***

# STEP 5: Create File In VIM
```c
#include <stdio.h>

int main()
{
    float x;

    x = 10.5;

    printf("%0.2f\n", x);

    return 0;
}
```

# STEP 6: Save File As - 0x03_arm_32_hacking_float.c [:wq]

# STEP 7: Build & Link
```
gcc -o 0x03_arm_32_hacking_float 0x03_arm_32_hacking_float.c
```

# STEP 8: Run Binary
```
./0x03_arm_32_hacking_float
10.50
```

# STEP 9: Run Radare2 - Debug Mode
```
r2 -d ./0x03_arm_32_hacking_float
```

# STEP 10: Run Radare2 - Debug Step 1 [Examine Binary @ Entry Point]
```
aaa
s main
vv
```
![image](https://github.com/mytechnotalent/0x03_arm_32_hacking_float/blob/main/1.png?raw=true)

# STEP 11: Run Radare2 - Debug Step 2 [Examine LSB & MSB @ R3]
```
q
[0x0046550c]> pd 2 @ 0x00465512
│           0x00465512      4ff00003       mov.w r3, 0
│           0x00465516      c4f22813       movt r3, 0x4128
```

# STEP 12: Run Radare2 - Debug Step 3 [Hack float]
```
wa movw r3, 0xd70a @0x00465512
wa movt r3, 0x4127 @0x00465516
```

# STEP 13: Run Radare2 - Debug Step 4 [Review Hack]
```
[0x0046550c]> pd 2 @ 0x00465512
│           0x00465512      4df20a73       movw r3, 0xd70a
│           0x00465516      c4f22713       movt r3, 0x4127
```

# STEP 14: Run Radare2 - Debug Step 5 [Hack Binary Permanently]
```
q
r2 -w ./0x03_arm_32_hacking_float
[0x000003fc]> aaa
[0x000003fc]> s main
[0x0000050c]> vv
```
![image](https://github.com/mytechnotalent/0x03_arm_32_hacking_float/blob/main/2.png?raw=true)
```
q
[0x0000050c]> wa movw r3, 0xd70a @0x00000512
[0x0000050c]> wa movt r3, 0x4127 @0x00000516
```

# STEP 15: Prove Hack
```
./0x03_arm_32_hacking_float
10.49
```
** NOTE **
If you wanted to hack from 10.50 to 10.51 instead you would simply:
```
[0x0000050c]> wa movw r3, 0x28f6 @0x00000512
[0x0000050c]> wa movt r3, 0x4128 @0x00000516
```
This should give you a good idea how the LSB and MSB work for floating point numbers now.

<br>

# License
[Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)
