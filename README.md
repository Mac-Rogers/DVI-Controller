# DVI Controller on Xilinx Zynq-7000 SoC
The objective of this project was to create a graphical output on a monitor using my ZyboZ7-10 FPGA.

## Step 1- Understanding DVI
DVI is an older display protocol which can be implemented over a more modern HDMI cable. The timing shown below mirrors that of the old CRT monitors, containing active periods for sending pixel data and blanking periods for maintaining syncronisation.
![image](https://github.com/user-attachments/assets/01cbed56-74db-472c-b39f-cfdac01d2f58)

## Determining resolution
Resolution was mostly determined by what is supported by the monitors I had access to and the maximium clk frequency of the ZyboZ7-10.
The following calculator was extremely useful:
https://tomverbeure.github.io/video_timings_calculator
![image](https://github.com/user-attachments/assets/5219c087-37e7-4c70-a6fb-b4a1c4aaa953)

## Maximum clk frequency
Each colour channel (RGB) is 8b/10b encoded, thus each pixel requires 10 clk cycles. The total frame size of a 640x480 resolution is 800x525, so at 60Hz this is:

800 x 525 x 60 x 10 = 252,000,000 clk cycles/s

This requires a system clk of 252Mhz, anything beyond this quickly became too fast for my system without specialised hardware.

## Control
Further research determined that the blue channel is responsible for carrying the vsync and hsync signals, and thye must be TDMS encoded in order to eb received correctly. 

Differential output is required for HDMI transmition, and my original attempt at implementing this in verilog was unsucessful, leading me to discover that a specific primative exists (OBUFDS) for doing just this.

## Results
![IMG_8901](https://github.com/user-attachments/assets/a1ebf046-03c4-4adc-8e62-3638740a1b17)

