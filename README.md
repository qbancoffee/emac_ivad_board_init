# emac_ivad_board_init
Arduino code to initialize eMac IVAD board via i2c.

When starting the eMac, the logic board or mother board initializes the IVAD
board via an i2c interface so that the CRT can be used. This arduino sketch 
sends the commands to the IVAD board in order to initialize it. The logic board
can now be removed and replaced with almost anything that can output a VGA signal. 

These commands were captured with a logic analyzer during the booting process and 
implemented here.

This program waits for a button press and turns on/off relays that turn the
CRT on and off.

