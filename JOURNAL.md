---
title: "CWatch"
author: "Madhav"
description: "A custom smart watch"
created_at: "2025-07-15"
---

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

## Day 8: 7/23: Finally, Finished

Ok. Today, I have finished!

I have decided not to use the mousbite-ification as it is just too complicated, and when I combine the PCBs, lots of things just don't work. I really did try to get it to work, but I don't think it will. I also added a photoresistor, a buzzer, and a microphone. The mic took the majority of the time, as I had to learn about I2S first, a completely foreign topic to me.

In more detail, I spent the majority of my time trying to figure out if adding a mic and speaker would be possible, but eventually decied that a speaker would be too much, so I have decided to go ahead and just add a microphone and a buzzer. It will limit what is doable, but it will alse greatly reduce complexity, and save me from learning 2 completely new topics, speaker driving and I2S for the mic

Also, it would be unlikely for my thing to have enough space for a speaker, as cool as it would be...

<img width="1135" height="438" alt="image" src="https://github.com/user-attachments/assets/bf007a2c-7ba3-4908-aa96-0a44e6fe5759" />

<img width="796" height="408" alt="image" src="https://github.com/user-attachments/assets/1e6b516a-c284-4173-9bc1-d2d9ece96cd0" />

Well ima a go submit it now.

---

ummm. k i sort of have a problem... I was going though my BOM, and now the cost is around ~200. I don't really know how to shave off the cost, without sacrificing

I honestly have no idea what to do. I've been spending all my time on this...

I have tried panelizing my 3 PCBs, again, and now its giving me another error,

<img width="409" height="148" alt="image" src="https://github.com/user-attachments/assets/531797cd-6dbd-4d32-b550-ac49131b8611" />

<img width="479" height="181" alt="image_480" src="https://github.com/user-attachments/assets/c54a6ed9-97cf-4403-8bf5-df161512606d" />

I guess I just don't yet know too much about panelization, but its just a bit too much. I have actually spent way too much time trying to get it to work...

Also, I have not yet finished designing the case, I just realized...

Though, honestly, all I need to do is desing the wrist band the thing that keeps it attached to my arm.

### Hours 4.5/43.5

## Day 9: 7/25: General Research and Improvements

Oki, so today, after one day of break, I am back to working on this watch.

A really cool thing I figured out is that GPS modules also output time, and thus my RTC might be a bit redundant. I think it is, however, still a good idea to have an RTC, as it can provide time at a very low power.

Well yeah, thats just something I realized...

---

And it gets worse from hear in out. I was doing some datasheet diving, with the help of a (possibly hallucinating) ChatGPT and realized that the battery life isn't that long...

<img width="705" height="394" alt="image" src="https://github.com/user-attachments/assets/05d33127-89ee-4c2e-b13d-e86c9ff5f79b" />

However, the datahseet says that there are 6 different power levels, so it is probably ok if I switch in between them to save power, and on the extreme power save mode, it is acutally nice.

I can switch the screen off (slowly) whenever there is a long absence of touch, and whenever the watch is touched, it can then turn everything on!

And, if I am going to have an always on page, I can just keep it on Power Mode 4, so that it just shows the time, updating either every second or minute.

According to the mischeivious ChatGPT, apparently the FAQ's maximum power consumption of 33.2 mA is wrong, as that is the power it will draw with the backlight barely on. It thinks that it will draw more like 40-60 mA when the LCD is on at maximum brightness. Well yeah.

Good thing I checked this. I am thinking of adding some high-side mosfet switches to save even more power on some components, so its ULP (Ultra-Low-Power) mode can hopefully last for months.

#### Hold that though

Let's do some math.

The screen, in lowest power mode, probably draws around 3 mA. This alone, on a 300 mAh battery would make it last around 100 hours, or around 4 days. So, it is, in fact, essential to provide a way to switch on and off the screen. I am thinking of dedicating one IP (Inter-PCB) connector. Currently, I have 23 IP pins, and well I am sort of running out of space. I might make one of the ground pins into a controlled GND (via N-Channel MOSFET)

<img width="565" height="530" alt="image" src="https://github.com/user-attachments/assets/ec3af5cf-03fa-4355-8a4b-9c893698268e" />

There, I added 5 more IP pins. They are connected to IO 1-3, 39 and 40

--

I have had an idea that it sure would be nice to be able to tell how much power the battery has left, so I decided I am going to add an INA226. It is a high sensitivity, low power drawing chip that can measure the battery and the current draw. Using the current draw, it can be estimated the amount of battery life left.

I could have just measured the voltage, however I would need a voltage divider, and even using 2x 100K resistors, it would still have a leakage current of around 20 uA. This alone is not huge, however there is no way of turning it off, and it would just constantly draw this power, greatly reducing battery life.

Instead, I am thinking of using the INA because I have some experience with it, and well it just seems like it would be nice to know how much battery is left, y'know.
---

Well. That took WAYYYYYYYY TOOOO long. I have no idea why, but its probably because of how dense the board is.

<img width="1154" height="891" alt="image" src="https://github.com/user-attachments/assets/c0c269e1-59e7-4c28-9a6a-5755b0de2c9f" />

This might not look like a lot, and the schematic isn't too bad either, 

<img width="894" height="573" alt="image" src="https://github.com/user-attachments/assets/76d1fb89-3e84-4b60-9d90-3cdb2a6477b3" />

However, the thing is, when you have all of your stuff all cramped into the same area, running simple dataline like the SDA/SCL lines can make everything a real pain. Adding even one thing can mean reconfiguring so much of the board, as all the ground and power planes are all so sensitive...

<img width="927" height="848" alt="image" src="https://github.com/user-attachments/assets/b3a9a4d7-e017-4d25-aa21-57d2c1cb1805" />

<img width="941" height="838" alt="image" src="https://github.com/user-attachments/assets/fbb6f75f-6e45-41d5-83b9-662744053185" />

And the SD card definitely didn't help.

---

<sub>Also, a side note, I am going to try and journal with more detail. Previously, about 2% of my time went to journalling, and I think I am going to up that to 5%, as these journals just seem to be a bit bland and lacking detail</sub>

### HOURS 4.5/48

## Day 10: More brain-strorming

Today, I spent some time thinking of what features to add. I spent a good hours or so, but I haven't really came up with much. I feel like I already have the stuff required for a smartwatch, and I might just need to add the MOSFETS, and finish the 3D model.

I am also thinking of having it so that there is a home screen and you swipe around to 3 commonly used pages, and an app page containing the other stuff...

Well I'm going to sign off now, as today has been a busy day (I had a familly event today, and thus I could only do some brain-storming. I wil also think about this before I go to bed...

### HOURS: 1.5 (49.5)

## Day 11: 7/27: Watch Bands & Other features.

Today was another day of brain-storming. I really want to make this project justify its cost, and thus I have decided to try to add as many features as possible. As I will be having voice commands, I was thinking that I should add an AI accelerator to make everything run on as little power as possible, however I couldn't really find a good way to integrate it, so yeah

---

On another front, today I explored watch bands and stuff. I had been spending some time thinking of how to connect the main watch part of it all to my hand, and I really like the way my current watch connects to my wrist, so I decided to look for a similar one.

My first stop was to look for a buckle. I really had like this one:

<img width="2560" height="2560" alt="image" src="https://github.com/user-attachments/assets/b8f746ad-3bfe-4dc0-ac35-13eff9aaa0f3" />

However, it was CA$4 plus shipping, feeling a bit expensive for a buckle. After that, I contemplated if the buckle would even be all I needed, as I can't print flexible items; some of the only 3D printable materials that cand bend are also notoriously hard to print. This is then compounded by the fact that the only 3D printer I have access to is at my local library, and thus can only print PLA (very, very fast).

So then, I looked for a complete strap. I found an absolutely terrific strap, one that I really liked, however it was a bit expensive, ~CA$15, plus $5 shipping.

<img width="616" height="659" alt="image" src="https://github.com/user-attachments/assets/d3a8d6f2-8fc1-4f1e-b34d-59163acb4af8" />

So then, I went to one the sites I find the least trust-worthy: aliexpress...

And I found this band:

<img width="455" height="450" alt="image" src="https://github.com/user-attachments/assets/f64055a9-2f30-43b3-a8cc-0ac12cd30ab4" />

Its pretty nice, in my opinion (not as good as the one on amazon, but still) and it just feels like it would be easy-ish to use.

Not to mention it was only $1.15 with the welcome coupon, or $8.71 without.

---

I was double checking my cirucits, and I realized I completely forgot the auto-reset circuit!

Will fix.

NVM false alarm. That was for the UART programmer, which I'm not using...

### HOURS:2.5/52 

## Day 12: 7/28: Double Checking PCB stuff.

Today, I worked on making sure that all my PCB stuff was good to go.

I still have to add the NMOS switches, but as it stands right now, all of my schematics seem to be right. I did catch a problem for the buzzer driver, as I had directly wire the io pin to the transistor, but I have now corrected that by adding a 2.2k resistor.

I am also thinking of adding a hardware switch,

<img width="728" height="519" alt="image" src="https://github.com/user-attachments/assets/e5cabf84-c59d-4149-948f-5011458905f7" />

So that it can inturupt deep sleep mode and get back to working. If I use the mosfets to switch on and off all (but the RTC) devices, they should consume <1 mA, more like ~300 uA, which should extend the 100 Hours to about 1000 hours, or ~4 days to ~42.

Well yeah, time to get MOSFETing!

---

First up, look for a MOSFET.

There are 4 types of MOSFETS, and I know that N-channel MOSFETS are for low side switches...

ooh! what if I use a speaker? I thought. And fell down another rabit hole. The speaker would be cool, however I felt like that would be a bit too much to learn, so I figured that I shouldn't do it, even though I had found all the nessasary parts...

Anyways, back to MOSFETs. Personally, I like the BSS138 N-Channel MOSFET, So that will work, however it is a low side MOSFET, so it might be difficult to use in some cases...

Well I'm going to go watch (at 2x) a 20-minute MOSFET video and get back to you...

---

I tried implementing low-side switching, and once I had finished it all, I realized something. A big fat mistake.

The thing is, I2C really only need 3 wires. 3.3V for power, along with SDA and SCL. You can completely elimintate the GND wire, as, especially in a one bus system, one of the lines will likely stay low for long enought to be the ground of the reciever. That would mean that all my work was basically for nothing, and I will have to restart with high-side switching.

I had actually renamed some 10+ GNDs to make sure it would work, and its just so sad to have to undo it all.

Well time to implement High-Side Switching!

---

Finally. I finally added the main High-Side Switch, and that was way too much work. Let me explain:

Remember when I said I had already crammed an insane amount of components in as little space as possible? Well every change causes soooo many disturbances, and I had to do it twice.

<img width="631" height="635" alt="image" src="https://github.com/user-attachments/assets/0a4616dd-63fb-4a01-a6a3-478c664d32ae" />

<img width="902" height="756" alt="image" src="https://github.com/user-attachments/assets/dbade2b6-ddfa-4bb8-a563-2d379641e84f" />

Above, you can see the complexity of just one of my PCBs, all on a 2-layer board.

<img width="1000" height="670" alt="image" src="https://github.com/user-attachments/assets/23421b9c-cfef-4d6d-936d-aa90adf7d279" />

Near key-components, it gets even worse. I am starting to think I should have used a 4-layer PCB, but hey! I saved on much of the cost...

The thing is, though, I have more than just one PCB. I have 3. So when I implemented the low-side switch, I put the switch on PCB2, with the main components on PCB1, so that meant that those PCBs got shook up so badly, and then it all happened again for the high-side switch.

<img width="893" height="319" alt="image" src="https://github.com/user-attachments/assets/51ab1edf-585c-41cd-bf62-b17e18f08c81" />

<sub> PCB2 </sub>

---

I am now going to quickly wire up the button to a GPIO that can then be used to wake it up, and I am going to add a Low-Side MOSFET to shut the entire board down if the battery gets too low. (The only way to re-wake it up will be to power it via USB)

How I will do that, however, I haven't yet figured, but an over-discharged battery can be a bad thing, and I want to avoid that.

---

OK, my plan is to have a diode that connects a GPIO to the 5V line. Normally, the IO will be high, ensuring that operation is normal, however when it is time to shut down, it will be sent low. When the signal is low, the MOSFET will disable.

To turn it back on, the USB will be plugged in. This will over-ride the (no longer active) GPIO, causing the board to wake back up and perform like normal. There are still some caveats, but the first step is freeing up board space...

---

Oki, after way to many double checking and stuff, I have this:

<img width="768" height="554" alt="image" src="https://github.com/user-attachments/assets/d77a9683-5692-481f-8ce5-ec7b53876e46" />

PGND is the battery ground, while everything else is connected to the normal ground. This results in the MOSFET being able to shut everything down, but as the USB is also connected to the normal ground, when it is plugged in it will work like normal, and I can command it to turn the MOSFET on again.

### HOURS: 6.5/58.5

## Day 13: (7/29)

The first thing I did was wire up that low side switch we were talking about, but as I said last time, it was pretty painful as there is next to no space left to route wires...

Here are all the components so far,

<img width="781" height="809" alt="image" src="https://github.com/user-attachments/assets/53641e04-dc6f-4b82-9451-fa9396a1148c" />

And with the wires, you can barely even tell where the things go.

<img width="776" height="815" alt="image" src="https://github.com/user-attachments/assets/8269a609-85eb-4ae7-b9ea-dd52fe384e34" />

<img width="654" height="421" alt="image" src="https://github.com/user-attachments/assets/f23629e9-8d07-4ee0-a8e9-e136d394fc14" />

I was lucky enough to be able to fit the components in the above are, thanks to yesterday's work.

---

I am now going to go finish up double-checking my entire board, and then on to 3D modelling!

Just freed up all this space:

<img width="836" height="703" alt="image" src="https://github.com/user-attachments/assets/c3775009-cb01-4fc9-a4f7-e457c4a3596c" />

This was all taken up by some LEDs and stuff, but they were only for the charge status pin, which I am just going to connect to an ESP32 pin.

I wonder what I should do with all this space...
