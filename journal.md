# Journal: How I made a watch
<sub> Hours formatting is "HOURS: today/total" </sub>

## Day 1: 7/15: Display & Brainstorming.
Today, I spent my time thinking about the idea. I have two main challenges. First of all, power consumption. As I don't have infinite battery, the watch is going to have to be able to be super power efficient. Even after exploring solutions for a good 2ish hours, I still don't know what to do. The STM32 series seems too complicated, while the RP2040, a chip I have experience with, isn't very power efficient.

Well, atleast I have decided on a display. I sort of like this one:

<img width="446" height="364" alt="image" src="https://github.com/user-attachments/assets/3756e94d-3465-4347-ad45-fe718a25ef07" />

It also has a built in touch sensor. I just need, now, a way to wake up the module when I need to...

Basically, I am thinking of keeping the entire thing off until the user signals that they want to check the time. When this happens, it all wakes up and serves your bidding. The only thing is, how?

### Hours: 4

## Day 2: 7/16: Power.

Before I start the PCB design, I need to find a suitable way to wake the entire thing up.

I was thinking that a button on the side would be okay, but that just doesn't seem very easy. I could maybe keep the chip on the entire time, and when it detects the right motion it could turn on, but that is bad for battery life, etc. What to do ... what to do.

Ok. I think I may have got it. I will probably just use an RP2040, which will turn on when you shake the watch. This will then be good enough. Either way, I will either use the ESP32-WROOM-32D-N16, as it is one of the only modules supported for economic PCBA, or I will use the RP2040. I might just make it so that the watch needs to be charged every night... That way it will be just fine to stay on 24/7.

<sub> I basically just spent the entire day debating this problem, but didn't have too much to journal about... </sub>

### HOURS: 6/10

## Day 3: 7/17: Rabbit Hole

I decided I wanted more juice. And I fell down a rabbit hole..

Let me start from the begininng.

I had a trade off. I had to have one:

Either, my watch would be a literal tree. It would be so big, that it would look so clumsy and stuff and just be all around weird:

![download](https://github.com/user-attachments/assets/ee06ba57-3fe6-4919-837d-9c954aa20f51)

Or, it would have very little battery life, something like a day's worth

<img width="225" height="225" alt="download" src="https://github.com/user-attachments/assets/dd31926e-2fa4-446b-b97e-e0877dcbfb1d" />

Or, it would just be a play old boring watch...

So, I decided, I will solve the power problem.

Enter: !!RABIT HOLE!!

So I fell down a rabbit hole of different batteries and all that junk. I even questioned my screen choice!

But all in all, I managed to pull my self out. I will just use either one or two LIR2032 batteries, and all will be well... I hope.

Ok, well either way, I am thinking of having a switch. When the switch is on, it will be on bye-bye-power mode. Basically, the battery life will be very short, less than 12 hours, however you will get full functionallity. However, when you really need to save battery life, you will be able to flick the switch, and it will turn everything off other than the RTC. You will be able to press a button, turning the watch on for ~5 seconds, and then it will shut everything back off.

I will now (maybe) start designing!!

Ok, a bit of designing later, here I am. (the main chip is the rp2040, and you can see that I added a buck boost so that it can use a single sell liion):

<img width="438" height="439" alt="image" src="https://github.com/user-attachments/assets/b2014ef9-d775-412f-9585-96514db544cf" />

But, I have sort of realized a problem. I had to go through some trouble with the USB-C and stuff, but it was all in all a whole lot easier than the last time, mainly because I have experience!

### HOURS: 5/15

## Day 4: 7/18: ESP32!!

Today, I decided. You know how I was using the rp2040? Well, I think I am going to use the ESP32. It just has the wifi stuff, and is also just seeming to be a better choice as it has a low power mode to. I feel like the wifi part would really start to make it feel truly professional, as it would be possible to connect to any phone and, for example, make it take a picture.

So I got to it. I did some basic stuff like copy-ishing the base schematic, but I also spent a good chunk of my time researching all the stuff, to make sure that I have the right idea and don't misinterpret something (like pF for uF).

Here's my schematic so far:

<img width="548" height="594" alt="image" src="https://github.com/user-attachments/assets/2730a9d7-b175-4d1d-b574-9e6d4fe4b523" />

Also, I am going to be using 2 pcbs. One will be in the middle, having some stuff, and the other having the rest. I am thinking that the IMU will be on the lower one, with the other components fit around it, and the upper pcb will have the other components, notabley the main computer, and whatever else I deem needed.

<img width="778" height="727" alt="image" src="https://github.com/user-attachments/assets/906ffb68-bbe5-4a72-b868-270859ff8272" />

Ok. I think I have basically finished PCB1, as you can see here!

It has the RTC, the ESP32, and the USB-C connector, so basically all the basic computational stuff. There are some 17 pins that I will use to communicate with the other PCB. I should probably rewire them to use a better connector, but hey, this is all I got. Because it only uses two layers, however, most of the wiring was actually rather tough, as I also have a very large component (the battery holder) that can't have wires running through it.

I do have some space left, but that can be used as extra space for more layer to layer connectors...

I could go into more detail, but I have gotten so late working on this, and I have a bad runny nose, so I have got to go.

Just so you know, I did a whole lot of datasheet searching and stuff, but I don't have time to explay right now :(

### HOURS: 6/21
