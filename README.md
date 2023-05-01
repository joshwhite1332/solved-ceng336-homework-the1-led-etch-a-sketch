Download Link: https://assignmentchef.com/product/solved-ceng336-homework-the1-led-etch-a-sketch
<br>
In this assignment, you will implement an application that you draw shapes on a 3×8 LED grid by pressing buttons connected to RB[0,1,2,3] pins. The LEDs on the rows are connected to the pins of PORT[A,C,D]. A video is also attached in which you can view the application in action.

<h2>2       Specifications</h2>

<h3>2.1       Application Behavior</h3>

The application has 3 phases. For ease of understanding , the buttons will be associated with the following names:

<table width="163">

 <tbody>

  <tr>

   <td width="46">Pin</td>

   <td width="117">Button Name</td>

  </tr>

  <tr>

   <td width="46">RB2</td>

   <td width="117">UP/RIGHT</td>

  </tr>

  <tr>

   <td width="46">RB1</td>

   <td width="117">DOWN/LEFT</td>

  </tr>

  <tr>

   <td width="46">RB0</td>

   <td width="117">TOGGLE</td>

  </tr>

  <tr>

   <td width="46">RB3</td>

   <td width="117">CONFIRM</td>

  </tr>

 </tbody>

</table>

Table 1: Button Names for Etch a Sketch.

The application should perform the designated action for a button only when that particular button is <strong>released</strong>. In other words, pressing/holding a button should not cause any change, only releasing that button should modify the application behavior.

A line refers to subsequent LEDs in a row being ON. This term does not specify anything regarding other LEDs in that row, they could be ON or OFF. A shape refers to the collection of lines etched in a row since the beginning of the application.

<h4>2.1.1        Start-Up</h4>

In this phase, initialize your application and then turn all of the LEDs on all of rows ON for 1000±50 msecs, and then turn them OFF. Your application should not be responsive to button presses/releases etc. during this period. Immediately move on to row selection phase after turning off the LEDs with the topmost row selected and no row has any shape drawn on (meaning they are all OFF).

<h4>2.1.2        Row Selection Phase</h4>

In this phase, the user selects which row to draw a line on. The LEDs on the selected row should blink with intervals of 200±50 msecs. The blinking should override any previously drawn shape on that row.

The user can use UP/DOWN buttons to make the row above/below the selected row the new selected row. If moving up/down is not possible (e.g. pressing UP when the selected row is the topmost row) the action should have no effect.

If a selected row has a previously drawn shape, switching from this row to another row should reveal this shape (which was overriden by blinking when the row was selected).

The TOGGLE button inverts the colors (toggles each LED) in the underlying shape of the selected row. Again, this should be only apparent when the selected row changes as the blinking overrides the shape seen on a row.

The CONFIRM button finalizes the decision on the selected row and transitions the application to drawing phase.

<h4>2.1.3       Drawing Phase</h4>

In this phase, a candidate line is added to the previously drawn shape in the selected row. This addition is similar to ”regular” drawing, the ON LEDs on the candidate line should turn previously OFF LEDs ON. The candidate line is always of length 1 and starts at the left edge of the row when drawing phase is entered, and should be visible in the row along with the previously drawn shape throughout the entire phase.

The TOGGLE button switches between the left or right edge of the line as the selected edge. The right edge is always selected by default when drawing phase is entered.

The RIGHT/LEFT buttons move the selected edge right or left in the row. However, the line length can never be shorter than 1 and the right edge can not move past the left edge / vice versa. As the line is modified, the change should be visible in the row.

The CONFIRM button permanently etches the line into the row shape and should transition the application back to row selection phase with the current selected row as the candidate selected row again (meaning the row should start blinking).

<h3>2.2       Button Debouncing</h3>

<a href="https://en.wikipedia.org/wiki/Switch#Contact_bounce">Contact Bouncing</a> is an issue that happens in real circuitry due to physical nature of the buttons. To guard against bouncing, implement features in your button handlers such that they do not respond to irregular pulses from bouncing (one way to do it is measuring how much time passed and eliminating very short pulses of period shorter than 10 msecs).

<h3>2.3       Debug Labels</h3>

Your application behavior will be blackbox checked by debugger scripts along with light glassbox checking for any errors that debugger scripts might miss. Some of these scripts are provided to you. Since the debugger scripts cannot know when to stop execution and check for values by themselves, you will need to add labels to certain locations in your program:

<ul>

 <li>init complete<strong>: </strong>Add this label right after your last initialization instruction. You should hit this label exactly once.</li>

 <li>sec passed<strong>: </strong>Add this label right after the start-up phase ends. You should hit this label exactly once.</li>

 <li>msec200 passed<strong>: </strong>You should start hitting this label every 200±50 msecs after you hit sec passed, regardless of the phase of your application/button states. This label should also drive your application in a way that if you are in row selection phase, the selected row leds should be ON after every 1st,3rd,5th… hit of this label (including hits in drawing phase), and they should be OFF after every 2nd,4th,6th… hit (including hits in drawing phase).</li>

 <li>rb[0,1,2,3] released<strong>: </strong>You should hit these labels when you detect a proper release that is not a bouncing noise for each button.</li>

 <li>main<strong>: </strong>This should be the label of your main loop in your application.</li>

</ul>

<h2>3      Setup</h2>

You should configure your board by opening PICSimlab and then:

<ul>

 <li>Click File <em>&gt; </em>Load Workspace and then pick the1 pzw.</li>

 <li>Click Modules <em>&gt; </em>Spare Parts , in the new window, File <em>&gt; </em>Load Configuration, and then pick the1 2021.pcf.</li>

</ul>

Create a project to be compiled with C18 and create a single source file the1.asm. You can fetch the config directives and other boilerplate from the recitation code.

To run the debug tests in Linux, you will need Python&gt;=3.6. mdb and make should already be in your PATH environment variable. Simply copy the tests directory under your project and within tests, run:

&gt; ./build and run all tests.sh

To run the debug tests in Windows, you will need Python&gt;=3.6 (also add it to your PATH environment variable). You will also need to add make and the debugger mdb to PATH, which come installed with your MPLAB X IDE, check the documentation to learn where they reside. After these steps, simply copy the tests directory under your project and within tests, run with cmd:

&gt; ./build and run all tests.bat 2&gt;nul

It is highly encouraged that you take apart debugger tests and write your own. You can check mdb documentation regarding the commands (you can skip on stimulus control language however, it is not necesarry at this stage). Two of the debugger tests are programmatically generated and since they are quite hard to read, the python scripts that use the generator are also provided for you to follow easily. It would simplify your job a lot if you try to programmatically generate your own tests as well (the generator itself is not shared with you as it spoils some implementation details for you to figure out).