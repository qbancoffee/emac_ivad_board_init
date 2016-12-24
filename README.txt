This program uses the arduino i2c lines to initialize the eMac's ivad board so that the CRT 
can be used without the logic board. This project was inspired by a thread on macrumors
by patriciooholegu

http://forums.macrumors.com/threads/i-have-decided-to-hack-emacs-crt-to-work-with-any-standard-motherboard.1754089/

The initialization sequence was captured using a logic analyzer from “https://www.saleae.com/ “.
Using their capture program you can probably open the saved capture files to see the actual recorded communications.

“emacIvadBoardFullComms.logicdata”
“emacIvadBoardHalfComms.logicdata”

I also exported the captured communications to csv files.

“emacIvadBoardComms.csv”
“emacIvadHalfBoardComms.csv”

After looking through the communications and doing some testing, I realized that the initialization
block was sent more than once and that sending it only once was enough to turn it on if timed correctly.

Wiring it all was pretty easy as most of it was already figured out by 
http://www.lbodnar.dsl.pipex.com/eServer/ 


I followed the eMac to VGA mapping on this page for the most part but I took the i2c lines 
going to the ivad board (SCK(pin 5) and SDA(pin 6) ) and plugged them straight into the arduino.

    eMac
    ---
1  |o o|  2
3  |o o|  4
5  |o o|  6
7  |o o|  8
9  |o o| 10
11 |o o| 12
13 |o o| 14
15 |o o| 16
17 |o o| 18
    ---
         DB15F VGA
 ------------------------
 \    5 o o o o o 1     /  
  \   10 o o o o 6     /   
   \ 15 o o o o o 11  /   
    ------------------



eMac   DB15F  Signal
5      15     DDE Clock
6      12     DDE Data
9      13     H Sync
1      14     V Sync 
1       3     B video
1       8     B ground
1       2     G video
16      7     G ground
17      1     R video
18      6     R Ground
3,7,8   5,10  Ground 



I don't know what any of the commands do but maybe someone can figure it out one day.

It isn't enough just to turn the board on, you have to make sure that the computer is set to the correct resolution and 
frequency.
I used 1280X960 @ 72Hz.
 
This was pretty easy setup on ubuntu 14.04 by adding a modeline in .xprofile but it was a litte more complicated on the raspberry pi.
I had to create a custom edid file. I used a modified version of an edid generator as this version did not work for me out of the box.
https://github.com/akatrevorjay/edid-generator






