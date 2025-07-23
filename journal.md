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

## DAY 5: 7/20: Wiring, and batteries. Again.

So, remember when I fell down that battery finding rabbit hole? Well, that happened to me again. Let me explain.

So I was going around, thinking that more battery capacity would equal better, and I thought, ooh wouldn't it be nice to find a better, bigger, coin cell?

So I went to my (not so) trusty partner, ChatGPT, and well, got absolutly nothing useful out of it. Of the not-hallucinated coin-cells it told me, way to many of them weren't availible. Questioning myself, I checked if my original battery was availible anywhere near me, so then I really panicked.

Panick Panick Panick

Ok. Once I had calmed down, I then went looking for new batteries. I tried AAAA batteries, but there weren't any holders on LCSC. I tried AAA batteries, but they were too fat. Finally, I looked at LiPo. The very same ones that I tried before. But this time, I reallized something; the battery I chose, I could build my PCB around. As long as the battery was within size, I could work with basically any battery.

So, that's where it leaves me. Right now, I am working on finishing up the PCB: I have added the battery terminal, and just am about to work on some power saving circuitry...

Oki doki

Here's my first PCB (the one sitting furthest from the screen), 

<img width="739" height="716" alt="image" src="https://github.com/user-attachments/assets/6f351a0e-e311-4e05-b962-546fe09cc7cd" />

And, as you can see, it will connect to the middle PCB

<img width="739" height="781" alt="image" src="https://github.com/user-attachments/assets/a3e61cb6-200b-4816-80e1-6d7b22156a2b" />

I cut out a hole for the JST connector, to absolutely save every little bit of space I can...

Here is the IMU part of it, this took quite some time as it runs off of 1.8V logic, while everything else is 3.3V

Oh, yeah I also worked on the lipo charger, which runs off of the USB-C port.

Because of the fact that there is litterally no space left on the board, its components are scattered everywhere. Honestly, I am actually really surpised that I was able to fit everything onto such a small board!

I am thinking of adding GPS to the entire assembly, but then one of the boards will have to use 2-sided assembly, which costs significantly more. I might just solder on the the GPS module, and as it has most of its circuitry in-built, it might be nice...

![51rD8sx03ML _AC_SX425_](https://github.com/user-attachments/assets/afde9257-ec3f-4cbf-81f7-75ffcd41c4a4)

### HOURS 6.5/27.5.

## DAY 6: 7/21: 3D and PCB

Here is my current PCB stuff, all joined togeather. I am going to get some dimensions and see if I can add the GPS module...

<img width="725" height="676" alt="image" src="https://github.com/user-attachments/assets/1fca7008-5d13-4bc7-960d-fdf877efde94" />

<sub> I also updated some things in the PCB, as I noticed I completely forgot to wire some connections up :) </sub>

Well that was weird.. I was just working on seeing what more I can add by making a 3D model, and Fusion just crashed?!...

UPDATE

I am trying to find a spot to fit the GNSS (GPS) module in, but it can't be driectly under everything, or it looks to thick and won't have good reception

<img width="578" height="537" alt="image" src="https://github.com/user-attachments/assets/86f876a8-3c60-4a3b-87da-86215b8733b8" />

So then I need to figure out what to do. I have already spent soo much time on this... what to do, what to do?

OKI DOKI!!!!!!!!!

Here is what I have!

<img width="765" height="531" alt="image" src="https://github.com/user-attachments/assets/a559add6-1c1f-4697-a4a3-b30dd8168900" />

Basically, this is the entire watch module thingy. Top to bottom, its about 2cm, with a diameter of 4.8 cm. I may have to add another 7~8mm for the GPS module, as a main goal of this watch was to be able to tell your speed...

Just so you know, this took me sooooooo ridiculously long as I had to come up with so many new ideas on the go, as previous ideas kept feeling worse...

<img width="741" height="753" alt="image" src="https://github.com/user-attachments/assets/2ce0d1c3-c09d-4f1f-b61d-6302f3634111" />

Anyways, you can even see that I modeled in some spaces so that the battery could fit probably, and if I chose to go with one 4-layer, standard assembly board (I know its more expensive, but it saves me around 2mm...) I could then add the GNSS module on to another layer. I am thinking, however, I am going to add a 3rd PCB, which will just contain the GNSS and the stuff it requries to work. I will, however, need to get the mousebites stuff to work, as there is now way 3 individually assembled boards will work, or maybe I could chose bigger components and assemble myself? I am not too good with this stuff...

Oh well, a task for tomorow.

### HOURS 6 / 33.5

## Day 7: 7/22: Finish?

I may have finished.

Finished.

Finished!

Finished!!!!!!!!!!!!!!!

Oki lemme explain:

So today, I finished the GPS,

<img width="451" height="478" alt="image" src="https://github.com/user-attachments/assets/e6e64ee9-6d9b-47e4-b3af-6704c34975fc" />

Finished the case,

<img width="699" height="666" alt="image" src="https://github.com/user-attachments/assets/9eb6d779-7fe8-4d41-aeba-46792c699904" />

<img width="632" height="573" alt="image" src="https://github.com/user-attachments/assets/3d8198a1-b471-45b9-9bdd-fcab5c43bf98" />

(Just need to add the outside thingy that goes around your wrist)

And placed down 165 mousbites. 5 at a time. Literally took me ages. But hey, a ~$30 saving is worth a few hours of work!

Its sort of sad that I have one over crammed PCB, and two other PCBs with like on component on them each... I can't combine the two as the GPS needs to stay away form everything, and they're all just adding so much hight :(

Well, yah das about it. I also bikes like ~20 kilometers today, so yay?

### HOURS 5.5/39
