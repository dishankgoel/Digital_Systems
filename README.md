# COLOR DETECTION USING FPGA

## Introduction

In this project, we built a classifier in Verilog which is able count the occurrences of circles based on their colours.
Also, we are classifying the circles based on their sizes by determining their radius. ​

The general pipeline of our implementation involves pre-processing an image file to its COE format and then loading this into the block ram of the FPGA.​

After running the implementation on BASYS3, it is expected that the output LEDs show the count of the occurrence based on their colours along with separate outputs which tell the number of circles belonging to the small, medium and large range.
The small range is from 0 to 15 pixels, medium range comprise of 16 to 30 pixels and large range has circles greater than 30 pixels radius.​

## Installation

You will need Vivado xilinx ISE suite to test the verilog code and implement it on FPGA. [here](https://www.xilinx.com/support/download.html) is the link to download Vivado.

To convert your image to coefficient file, run Coe_Converter.py and give the name of your image. The image should be in the same directory as the python script. You will need pillow module of python to run this script.
To install pillow, run the command, `sudo pip install pillow`

#### Important note:
The image size should be 430X200. You need to use the IP core generator of Vivado to instantiate the block RAM.
If you wish to run only simulations, then work in the `color_detection_SIMULATION` folder, otherwise if you want to implement the verilog code on BASYS3, then use `color_detection_ON_FPGA`

## Workflow

First, the image is converted into its coefficients file (.COE) using python. During this step, the 24-bit true color coding is changed to 8 bit color.​
The coefficients file of the image is then loaded into the Block RAM on the FPGA in continuous memory locations. A single port BRAM is used for this purpose.​

We have a single module called "Color_Detection" that processes data from the BRAM and applies our own algorithm to classify the circles based on their color as well size.

The module has a single always block which maintains a sensitivity list of all the changing variables like the count of colors, size of differently coloured circles etc.
There are flag variables to shift the control flow from detecting the top most point to calculate the radius of circles to erase the circle afterwards.

We use case statements to classify the circles based on the 8-bit color.

#### Here are the details of the algorithm that this module implements:

* For counting the circles, the image data is traversed row wise to find the first pixel which has a color that is different from the background color of the image. This pixel is the top-most point of some circle. The count of that color is increased by one.​

* For detection of radius, we go one pixel down ​one pixel right at each​ step until we get a pixel​ that is different from the​ color of the top-most​ pixel of the circle.​

* The distance travelled downwards from the​ top-most is taken as the​ radius of the circle. ​

* The count of the appropriate size for the color is increased by one.​

* Now that we have detected the circle and we don’t want to detect it again. Henceforth, we traverse back to the top-most pixel and start coloring the square whose mid-point of top edge is the top-most point of the circle to the background color. ​

* Appropriate tolerances are given because the pixelated image does not have a sharp, single pixel top.​

* This process continues until the traversing pointer reaches the last pixel of the image.​ 

## Simulation Graphs

![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/Simulation_Graphs/Black_Background_Simulation.PNG)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/Simulation_Graphs/Green_Background_Simulation.PNG)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/Simulation_Graphs/Red_Circles_Simulation.PNG)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/Simulation_Graphs/Same_Radius_White_Background.PNG)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/Simulation_Graphs/White_Background_Simulation.PNG)



## Test Images

![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/Final_test.png)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/Final_test_black.png)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/Final_test_green.png)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/Red.png)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/Test_final.png)


![alt text](https://github.com/antimattercorrade/Digital_Systems/blob/master/images/test.png)

​
