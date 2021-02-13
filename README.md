# Vivado-AXI-Timer-and-Interrupts
# Summary
The project analyses different functions of Vivado’s SDK IP Integrator. For this basic IP integrator was explored. A block design with two GPIO interfaces and AXI Timer block was created in the Vivado software. Enabled interrupt on one of the GPIO which was connected to buttons and AXI Timer. A template program was ran without modification to verify its performance. After that, programmed the system to behave certain way according to the inputs from Switches, Buttons and Timer based on the specification provided. The motivation for this analysis was to understand concept of Interrupts in Embedded Systems. The results were as expected. Successfully synthesized the block design and the system behaved the way desired.
# Introduction
The objectives of the project were to control the behavior of LEDs on the zybo board using the switches and buttons using Interrupts. The tasks to perform are described below:
1.	 Add a GPIO to interface the slide switches (SW) to the project without interrupts. SW0 when ON disables all push button interrupts from BTN1 and BTN2 as described below, resets the number of interrupts for LED count incrementation to the default of 3 and sets the LED count and display to 0000. This will require you to locate the parameter description of this added GPIO. You could consider that an input GPIO could have two channels and this could be used here. 
2.	Reconfigure the template file so that a BTN1 interrupt increments the current number of times the AXI Timer will interrupt the process before it increments the LED count autonomously (default of 3 interrupts) with a maximum count of 7 interrupts. In addition to SW0 OFF, SW1 must be ON for BTN1 to be an effective interrupt. 
3.	Reconfigure the template file so that a BTN2 interrupt decrements the current number of times the AXI Timer will interrupt the process before it increments the count autonomously (default of 3 interrupts) with a minimum count of 1 interrupt. In addition to SW0 OFF, SW2 must be ON for BTN2 to be an effective interrupt. 

4.	The number of interrupts (from 1 to 7, default 3) in use whenever changed with a BTN interrupt is to be outputted to the SDK console using the xil_printf function once to verify the process. 
5.	The process should not clear and enable BTN interrupts until that BTN is released to avoid a cascade on interrupts.

# Discussion
Created a block design consisting of ZYNQ7 processing system, processor system reset, AXI Interconnect, AXI Timer, Concat and two AXI GPIOs. The diagram of the design is shown below:
 
Figure 1. Diagram of the Hardware design
It can be observed that two GPIO blocks are created. One has interrupt enables with Buttons connected. The other one has LEDs and Switches connected to it. An AXI Timer was also included in the design. The interrupt from the Timer and GPIO were connected to the processor using a Concat block. Validated the design, HDL wrapper file was created and generated the bitstream. The design was then exported to SDK. Imported the template interrupt_controller_tut_2D.c file. The program was first run without any modifications and then modified it to perform the specified tasks.

Task 1:
The template program when run, displayed the LED with an increment of one. Depending on the button pressed the LED count was incremented. Looking at the program it was observed that it was using interrupts to do the above process. Two main components of the program were the button interrupt handler and Timer interrupt handler. The Timer interrupt handler was responsible for the default LED count increment by one. The Timer handler was set such that the program will increment the LED count after the AXI Timer interrupts the program 3 times. Meanwhile, the Button handler will interrupt to increment the LED count by the number of the button pressed each time. For example, if BTN1 is pressed the LED count will be added by 2. 
Task 2:
In this task, we were asked to disable the Button interrupts, reset the number of Timer interrupts before the LED count is incremented to default 3 and set LED count to 0 and display it. This was accomplished by the following code:
 
Figure 2. Code for Task 2
The code above checks if SW1 is ON. If it is ON, the Button interrupt is disable using the function shown above. Also, the Timer count is reset to 3 and LED count is set to -1 which will become 0 when the Interrupt is enabled. 
The image below shows the SDK console when this task in action:
 
Figure 3. Task 2 SDK console
The image below shows the zybo board while SW0 is turned ON:
 
Figure 4. Task 2 Hardware Implementation
A you can see from the image above, when SW0 is turned ON, all 0’s are displayed on LEDs  and Button interrupts are disabled. 
Task 3 & 4:
Task 3 was to interrupt the program using BTN1 and change the AXI Timer interrupt counter to 7. The image below shows the code to accomplish this Task:
 
Figure 5. Code for Task 3
The code above shows that when SW1 and BTN1 are ON, the Timer counter is changed to 7. Meanwhile, when SW2 and BTN2 are ON, the timer counter is set to 1. 
The images below shows the SDK console when these tasks are in action:
 
Figure 6. Task 3 SDK console
 
Figure 7. Task 4 SDK console
It can be observed that in figure 6, the timer count is from 0 to 7. Whereas, in figure 7 the timer count is 0 and 1. 
The images below show the conditions switches and buttons in zybo board:
 
Figure 8. Task Zybo board

 
Figure 9. Task 4 zybo board
Image below is the code for Timer interrupt handler which is our “Default” case:
 
Figure 10. Timer Interrupt Handler

# Conclusions
Successfully generated a hardware design using Vivado software and implemented the tasks on Zybo board by programming in C using SDK. The main object of the project was to use interrupts to accomplish tasks specified. Successfully implemented interrupt handlers and other functions to accomplish the task. The project results met the objectives as expected. 
