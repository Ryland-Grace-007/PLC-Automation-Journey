# Project #003 — Box Counter



## 1. Project Overview



This is the third project in my PLC \& Industrial Automation learning journey.



The project is a basic automated box-counting system developed using OpenPLC Editor V4 and tested using the OpenPLC Simulator.



The purpose of this project was to move beyond simple ON/OFF control and learn how a PLC can count repeated events using a sensor and a Counter Up (CTU) function block.



In this project, an object sensor represents a sensor positioned on a conveyor. Every time a box passes the sensor, the PLC registers the event and increases the box count.



---



## 2. Agenda



The main objectives of this project were:



- Understand how PLC counters work.

- Learn the purpose of a CTU (Count Up) function block.

- Use a sensor to represent boxes passing through a conveyor.

- Count repeated sensor events.

- Understand the difference between a sensor signal and a counted event.

- Use a preset count value to determine when the required number of boxes has been reached.

- Learn how the PLC scan cycle affects counting.

- Understand how a counter can be reset.

- Test and troubleshoot the counter using the OpenPLC Simulator.



---



## 3. Basic Idea



The basic idea is:



BOX PASSES SENSOR → SENSOR DETECTS BOX → PLC COUNTS THE EVENT → COUNT INCREASES



When the required number of boxes has been counted, the PLC can use the counter status to control the conveyor or another output.



### Simplified Concept



BOX 1 ──┐

BOX 2 ──┤

BOX 3 ──┤

BOX 4 ──┤

&#x20;       ↓

OBJECT SENSOR

&#x20;       ↓

&#x20;    PLC LOGIC

&#x20;       ↓

&#x20;  CTU COUNTER

&#x20;       ↓

&#x20;  CURRENT COUNT

&#x20;       ↓

&#x20; TARGET REACHED?

&#x20;       ↓

&#x20; CONVEYOR CONTROL



The important concept introduced in this project is that the PLC is not simply checking whether the sensor is ON.



Instead, it uses the sensor event to increase a numerical count.



---



## 4. PLC Variables



The project uses Boolean variables and a counter function block to represent the control system.



### Input / Control Variables



- `START` — starts the conveyor/control sequence.

- `RESET` — resets the counter and returns the counting sequence to its initial state.

- `BOX\_SENSOR` — represents the sensor detecting a box.






### Counter



- `COUNTER` — CTU (Count Up) function block used to count detected boxes.

- `PV` — preset value representing the required number of boxes.


- `CV` — current value representing the number of boxes counted.



### Output



- `CONVEYOR\_MOTOR` — represents the conveyor motor.



The exact variable names may be adjusted according to the OpenPLC project implementation.



---



## 5. What Is a CTU Counter?



CTU stands for:



\*\*Count Up\*\*



A CTU counter increases its current value whenever it receives a new counting event.



For example:



Box 1 detected → CV = 1  

Box 2 detected → CV = 2  

Box 3 detected → CV = 3  

Box 4 detected → CV = 4  

Box 5 detected → CV = 5



If the preset value is:



`PV = 5`



then the counter reaches its preset after the fifth counted event.



The counter can then provide a status signal indicating that the preset count has been reached.



---



## 6. Working Principle



The PLC continuously scans the programmed logic.



During each scan cycle:



1\. The PLC reads the current input states.

2\. The box sensor state is evaluated.

3\. The CTU counter processes the counting input.

4\. The counter current value (`CV`) is updated.

5\. The PLC compares the current count with the preset value (`PV`).

6\. The counter status changes when the required count is reached.

7\. The corresponding output logic is evaluated.

8\. The PLC updates the conveyor motor output.

9\. The process repeats continuously.



This demonstrates how a PLC can convert repeated physical events into a numerical process value.



---



## 7. Important Counting Concept



One important concept learned during this project is that:



\*\*A sensor being ON is not automatically the same as one box being counted.\*\*



The PLC counter needs to register a counting event.



For example:



Sensor OFF → Sensor ON  

&#x20;            ↓  

&#x20;       Box detected  

&#x20;            ↓  

&#x20;       Counter +1



If the sensor remains continuously ON while a box is in front of it, the PLC logic must ensure that the same box is not unintentionally counted multiple times.



This introduces the concept of edge/event-based counting, which is important in real industrial automation systems.



\---



\## 8. Ladder Logic Structure



The basic control structure can be represented as:



START  

&#x20; +  

BOX\_SENSOR  

&#x20; ↓  

PLC LOGIC  

&#x20; ↓  

CTU COUNTER  

&#x20; ↓  

CURRENT COUNT (CV)  

&#x20; ↓  

PRESET COUNT (PV)  

&#x20; ↓  

TARGET REACHED  

&#x20; ↓  

CONVEYOR CONTROL



A separate reset condition is used to return the counter to its initial state.



RESET  

&#x20; ↓  

CTU RESET  

&#x20; ↓  

CV = 0



The counter therefore becomes part of the decision-making process instead of directly connecting the sensor to the motor.



\---



\## 9. Development Process



The project was developed incrementally.



The general development process was:



1\. Create a new OpenPLC V4 project.

2\. Define the required Boolean variables.

3\. Create the conveyor control logic.

4\. Add the box detection sensor.

5\. Add a CTU counter.

6\. Configure the counter preset value.

7\. Connect the sensor logic to the counter.

8\. Add the reset condition.

9\. Connect the counter status to the required control logic.

10\. Compile the project.

11\. Test the logic using the OpenPLC Simulator.

12\. Observe the counter current value.

13\. Test multiple box-detection events.

14\. Test the reset operation.

15\. Troubleshoot unexpected counter behaviour.

16\. Modify and retest the logic.



\---



\## 10. Problems Encountered



The project required troubleshooting during development.



\### Problem 1 — Counter Did Not Initially Count as Expected



The CTU counter did not initially behave exactly as expected during simulation.



The input state and counter configuration had to be checked to understand why the current count was not increasing correctly.



\### Problem 2 — Understanding the Counting Event



A major concept that required attention was the difference between:



`Sensor = TRUE`



and:



`A new box has been detected`



A continuously active sensor signal can behave differently from a new detection event.



This helped demonstrate why industrial counting systems often use edge detection or appropriate sensor logic.



\### Problem 3 — Reset Behaviour



The counter reset condition also had to be tested carefully.



The reset must return the counter to its initial state so that a new counting cycle can begin.



\### Problem 4 — Testing the Complete Sequence



The counter, sensor, reset and motor-control logic had to be tested together rather than individually.



This demonstrated that a PLC program should be tested as a complete sequence and not only by checking whether individual components appear to work.



\---



\## 11. Troubleshooting Approach



The main troubleshooting method used was:



OBSERVE COUNTER  

&#x20;     ↓  

CHECK SENSOR STATE  

&#x20;     ↓  

CHECK CTU INPUT  

&#x20;     ↓  

CHECK CTU PARAMETERS  

&#x20;     ↓  

CHECK CURRENT VALUE (CV)  

&#x20;     ↓  

CHECK PRESET VALUE (PV)  

&#x20;     ↓  

CHECK RESET CONDITION  

&#x20;     ↓  

MODIFY ONE PART OF THE LOGIC  

&#x20;     ↓  

COMPILE  

&#x20;     ↓  

SIMULATE AGAIN



This approach made it easier to determine whether the problem was caused by the sensor input, counter configuration, reset condition or output logic.



\---



\## 12. Final Working Principle



The final system represents a PLC-controlled box counting process.



The conveyor represents a production or material-handling system.



As boxes pass the detection point:



BOX  

&#x20;↓  

SENSOR  

&#x20;↓  

PLC  

&#x20;↓  

CTU COUNTER  

&#x20;↓  

COUNT INCREASES



The PLC keeps track of the current number of detected boxes.



When the required number of boxes has been reached, the counter status can be used by the control logic to change the conveyor or process state.



The counter can then be reset to begin a new counting cycle.



The overall concept is:



Physical Event  

&#x20;     ↓  

Sensor  

&#x20;     ↓  

PLC Input  

&#x20;     ↓  

Counter  

&#x20;     ↓  

Decision  

&#x20;     ↓  

Output



This represents a basic industrial counting application.



\---



\## 13. Software Used



\- OpenPLC Editor V4

\- OpenPLC Simulator

\- Ladder Logic

\- CTU (Count Up) Function Block

\- GitHub

\- GitHub Desktop



\---



\## 14. What I Learned



Through this project I learned:



\- What a CTU counter does.

\- How PLCs can count repeated events.

\- How a sensor can be used as a counting input.

\- The difference between a sensor state and a detection event.

\- The meaning of `CV` and `PV`.

\- How a counter can be reset.

\- How counter status can be used in control logic.

\- Why PLC scan-cycle behaviour matters.

\- How to troubleshoot a PLC counter systematically.

\- How counting functions are useful in conveyor and production automation.

\- Why real industrial counting systems require careful consideration of sensor behaviour.



\---



\## 15. Project Status



\*\*Status:\*\* Completed



\*\*Platform:\*\* OpenPLC V4



\*\*Simulation:\*\* OpenPLC Simulator



\*\*Project:\*\* Box Counter



\*\*Project Number:\*\* 003



\---



\## 16. Learning Journey



This project is part of my ongoing hands-on PLC and Industrial Automation portfolio.



The progression so far is:



Project #001  

Motor Start/Stop  

&#x20;       ↓  

Project #002  

Object Detection Conveyor  

&#x20;       ↓  

Project #003  

Box Counter



Each project introduces a new PLC concept while building on the previous one.



The objective of this repository is not only to store completed projects, but also to document:



\*\*What I built → Why I built it → How it works → What went wrong → How I fixed it → What I learned.\*\*



---



## Project Evidence



!\[Project #003 - Box Counter](screenshots/Project\_003\_Box\_Counter.png)

