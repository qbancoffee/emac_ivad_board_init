This program uses the arduino i2c lines to initialize the eMac's ivad board so 
that the CRT can be used without the logic board. This project was inspired by
a thread on macrumors by patriciooholegu

http://forums.macrumors.com/threads/i-have-decided-to-hack-emacs-crt-to-work-wit
h-any-standard-motherboard.1754089/

The initialization sequence was captured using a logic analyzer from 
“https://www.saleae.com/ “. Using their capture program you can probably open 
the saved capture files to see the actual recorded communications.
I used version 1.2.11 on an ubuntu 14.04 machine.

“emacIvadBoardFullComms.logicdata”
“emacIvadBoardHalfComms.logicdata”

I also exported the captured communications to csv files.

“emacIvadBoardComms.csv”
“emacIvadHalfBoardComms.csv”

After looking through the communications and doing some testing, I realized 
that the initialization block was sent more than once and that sending it 
only once was enough to turn it on if timed correctly.

Wiring it all was pretty easy as most of it was already figured out by 
http://www.lbodnar.dsl.pipex.com/eServer/ 


I followed the eMac to VGA mapping on this page for the most part but I took 
the i2c lines going to the ivad board (SCK(pin 5) and SDA(pin 6) ) and plugged
them straight into the arduino.

    eMac Logic Board connector pinout
                 -----
             1  | o o |  2
             3  | o o |  4
             5  | o o |  6
             7  | o o |  8
             9   ]o o | 10
             11 | o o | 12
             13 | o o | 14
             15 | o o | 16
             17 | o o | 18
                 -----

         DB15F VGA
 ------------------------
 \    5 o o o o o 1     /  
  \   10 o o o o 6     /   
   \ 15 o o o o o 11  /   
    ------------------


      Mapping

eMac   DB15F  Signal
5      15     DDE Clock
6      12     DDE Data
9      13     H Sync
11     14     V Sync 
13      3     B video
14      8     B ground
15      2     G video
16      7     G ground
17      1     R video
18      6     R Ground
3,7,8   5,10  Ground 



I don't know what any of the commands do but maybe someone can figure it out 
one day.

It isn't enough just to turn the board on, you have to make sure that the 
computer is set to the correct resolution and frequency.
I used 1280X960 @ 72Hz.
 
This was pretty easy to setup on ubuntu 14.04 by adding a modeline in .xprofile 
but it was a litte more complicated on the raspberry pi.
I had to create a custom edid file. I used a modified version of an edid
generator as this version did not work for me out of the box.
https://github.com/akatrevorjay/edid-generator

Included is the edid.dat file I generated for the raspberry pi. This file 
should be included in the /boot directory and /boot/config.txt should be updated
to use the edid.dat file. I've also included my config.txt to help as a guide.


Unfortunately I have not been able to get the image to completely fill the 
screen, there is a 1/2 inch border all around. I'm pretty sure this can
be fixed by tweaking the edid.dat file because I was able to get the image to 
completely fill the screen by creating a Modeline and including
it in the .xprofile file under ubuntu 14.04.  Unfortunately the same technique 
did not work with the pi.

I'm hoping there is someone out there that can get this to work one day.
