<pre>
 ___                                               ___                                
(   )                                             (   )  .-.                          
 | |    .--.    ___ .-. .-.     .--.    ___ .-.    | |  ( __)  ___ .-. .-.     .--.   
 | |   /    \  (   )   '   \   /    \  (   )   \   | |  (''") (   )   '   \   /    \  
 | |  |  .-. ;  |  .-.  .-. ; |  .-. ;  |  .-. .   | |   | |   |  .-.  .-. ; |  .-. ; 
 | |  |  | | |  | |  | |  | | | |  | |  | |  | |   | |   | |   | |  | |  | | |  | | | 
 | |  |  |/  |  | |  | |  | | | |  | |  | |  | |   | |   | |   | |  | |  | | |  |/  | 
 | |  |  ' _.'  | |  | |  | | | |  | |  | |  | |   | |   | |   | |  | |  | | |  ' _.' 
 | |  |  .'.-.  | |  | |  | | | '  | |  | |  | |   | |   | |   | |  | |  | | |  .'.-. 
 | |  '  `-' /  | |  | |  | | '  `-' /  | |  | |   | |   | |   | |  | |  | | '  `-' / 
(___)  `.__.'  (___)(___)(___) `.__.'  (___)(___) (___) (___) (___)(___)(___) `.__.'  
</pre>

# Govee RGBICWW Teardown 

![Govee PCB Photo](https://lemonlime0x3c33.io/wp-content/uploads/2024/06/IMG_9414-scaled-e1718673977366.jpeg)

For detailed instructions on device teardown and hardware interfaces please see the
following posts on my website:

1) For Physical Device Teardown and Component Identification:
   [physical teardown](https://lemonlime0x3c33.io/18/06/2024/govee-rgbicww-smart-table-lamp-part-1-teardown/)
   
2) For UART Interface:
   [UART](https://lemonlime0x3c33.io/21/06/2024/govee-rgbicww-smart-table-lamp-part-2-uart/)
   
3) For Firmware Extraction:
   [Firmware](https://lemonlime0x3c33.io/27/06/2024/govee-rgbicww-smart-table-lamp-part-3-firmware/)

4) For Firmware Analysis:
   Coming Soon! (I am busy researching freeRTOS reverse engineering tools)

## This Repo is a Work in Progess

I am hoping to reverse engineer enough of the firmware/code to write my own small custom
application to run on it. I like tearing stuff down for fun and have found many other 
people's blogs/posts super helpful and wanted to share mine as well just in case it
helps someone. :) 

## Repo Structure

README.md

Firmware
    -Govee_firmware.bin

Components 
    -Datasheets
    -PCB-Photos

**Hope you find something useful :)**
