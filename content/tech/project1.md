---
title: "Pen-Digitizer"
date: 2023-06-28T23:43:43-07:00
draft: false
order: 4
imgLink: "https://i.imgur.com/Mm2XxqD.jpg"
tagline: "A pen attachment that digitizes your handwriting live. CS-147 Final Project by Sai Sravan Gogulapati and Barr Avrahamov."
externalLink: "https://github.com/sravangogulapati/Pen-Digitizer"
---
*A pen attachment that digitizes your handwriting live.*<br>
CS-147 Final Project by Sai Sravan Gogulapati and Barr Avrahamov.<br>
<br>
We set out to make a quick and user-friendly way for people to digitize their notes as they write with their pen. The goal by the end of the quarter was to have a device that we can draw shapes with and see the drawings we made on our computer. We made the device using an ESP32, gyroscope, and a battery pack. We wrote a BLE client that runs on our computer using a Python called Bleak. The python program receives gyroscope data from the ESP32 over BLE and draws on a canvas accordingly. With our device, we are able to draw shapes and even write words that are legible as long as they are big enough. While it is not able to transcribe fine movements like we hoped, it is still satisfying to see that we can write using our device.

## Motivation
In this digital era, having digitized versions of handwritten notes enables better record-keeping, searchability, and easy access. Going fully digital using a tablet and a stylus, for example, would be a substantial purchase and requires a lifestyle change to move away from physically handwritten notes. There are a few smart pens that digitize your physical writing as you write on a paper. However, you must purchase a special pen and, in most cases, special paper in order for the pen to work. The user cannot use any of their preferred pens if they want to digitize their work. There is also the recurring cost of buying the special paper again once they run out. We want to design a solution that requires the least lifestyle change as well as the least amount of recurring costs required for basic functionality.

## Project Goals
We wanted to design an IOT pen attachment that a user can attach onto the end of their preferred writing utensil and start digitizing their work right away. We would like to have an app that receives the gathered data and displays the digitized notes. There would also be buttons on the attachment so the user can control app actions such as starting recording, stopping recording, or starting a new page. To make the project more realistic for ourselves, we focused on big movements such as drawing simple shapes instead of focusing on fine movements such as writing.

## Components
- LILYGO-TTGO (ESP32)
- Accelerometer and Gyroscope (LSM6DSO )
- Breadboard
- USB Power Supply

## Challenges and Future Work
Accelerometer values were being measured discretely, so calculating velocity from that was like integrating a discontinuous function. This led to the cursor drifting and not working as intended. We were unable to draw even simple shapes due to the noise. We instead considered the gyroscope approach and figured out a way where we did not rely on the time interval between each gyroscope measurement. In layman's terms we converted our product from more of a stylus/pen to a remote.
Another thing we were doing inefficiently was that we were polling from the laptop and reading the bluetooth buffer to see if the value was updated. We wanted to asynchronously receive gyroscope values from the ESP32, but the Notify function of the Python library we used (Bleak) was not able to establish an asynchronous connection with the ESP32. We realized that we needed to add a bluetooth descriptor to the ESP32 code so it could handle the notify connection. After that, we were able to use the notify function properly, and the laptop was able to receive values asynchronously.<br>
There are some accuracy issues with the gyroscope. We believe there were two root causes: Gravity/outside forces affecting the angular velocity of the device and packet sending rates. When the gyroscope was left to rest on the table there was still an angular velocity set even though it was perfectly still. We theorized that it may be due to gravity or some outside forces, but we were unable to track it down or remove it. This means that, if left still, the cursor would still continue to drift. The second issue that caused accuracy issues was the rate we were sending and receiving data over bluetooth. We decided to send a packet just one position of information over bluetooth every 100 milliseconds which restricted the drawing to work at 10 frames per second. This made the drawing much less accurate and stuttered. We wanted to implement a better method to send multiple position vectors in the packets and so the drawings would be smoother. This would create a delay of 100 milliseconds but we thought it would be worth the sacrifice. Sadly we did not have the time to implement such a method. Other additions to this project would include adding the ability to save drawings, adding more user controls on the pen, and cloud storage/backup for the drawings.