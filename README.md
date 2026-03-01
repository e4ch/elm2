# ELM2
Reverse engineering the analog telephone PTT Tritel ELM2

Note: This is work in progress.
- IMG_9916.jpg (raw) and IMG_9916_cut.jpg (rotated and cut) is a photo of the top side of the PCB
- scan_pcb_elm2.jpg is a scan of the lower side of the PCB
- scan_pcb_elm2_lowres.psd is a Photoshop 7 Elements file of both of the above merged (two layers) where you can adjust the opacity to view both the connections and components (unfortunately with reduced resolution; the original is 300MB, but GitHub only allows max. 25MB files)
- schema_elm2_sketch.gif is a scan of the manual drawn connections based on the above images - first draft of the schematic
- **Schematic.pdf** is the current PDF export of the schematic in the kicad folder - second draft of the schematic and current state
The connections should be fine, but very chaotic still.

I always assumed that the RJ9 connector to the handset with the four cables goes directly to the microphone and speaker. I've opened the handset to check which is which and found a whole new PCB in there too:
- IMG_0208.HEIC is a photo of the PCB
- pcb_small_back.jpg is a scan of the backside of that PCB
- pcb2_lores.psd is a combined Photoshop 7 Elements file of both of the above merged (two layers) where you can adjust the opacity to view both the connections and components (unfortunately with slightly reduced resolution because GitHub only allows 25MB files max.)
- schema_elm2_pcb2_sketch.gif is a photo of the manual drawn connections based on these images - first draft of the schematic for PCB2
- **Schematic2.pdf** is the finished schema of the board in the headset

The schema of the second board is now finished. It is based around the IC TEA1061. The connection to the base is as follows:
- RJ9 pin 1 - white  - J31 pin 6: VLINE
- RJ9 pin 2 - blue   - J31 pin 5: GND
- RJ9 pin 3 - green  - J31 pin 4: MUTE
- RJ9 pin 4 - violet - J31 pin 3: RX

The VLINE / GND transport supply voltage to this circuit. RX is voice data for the speaker and goes directly to the DTMF input of the IC. MUTE is used to disable speaker output, maybe during ringing or something.

The interesting part is how microphone sound is transferred back, as there is no separate line for this. The LN output of the IC (pin 1) is the collector of an NPN transistor in the IC that drives it to SLPE (pin 18) according to the block diagram of the IC. With R34, only 43Ω, this modulates VLINE. So VLINE is also used for voice feedback.

The Zener diode V31 does not get and DC voltage fed through any resistor, so this is purely for limiting the voltage that goes to the speaker.

The circuitry R31, C31, R32, C32, C41, R33, R35, C34, R36 is purely feedback loop and is to limit the output of the microphone voice to the speaker.

R39 is the supply voltage for the IC (VCC, pin 15) and for the microphone and is stabilized by C36. As the IC is modulating this VLINE voltage, it's important to keep this stable for the IC itself.

License
-------
All original content in this repository (photos, schematics, PCB layouts, and documentation) is licensed under Creative Commons Attribution-NonCommercial-Share-Alike 4.0 International (CC BY-NC-SA 4.0).

This repository contains independently created reverse-engineering documentation.
All trademarks and original designs belong to their respective owners.
