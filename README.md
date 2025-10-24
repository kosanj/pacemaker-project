## Pacemaker Simulation Project

This team project involved simulating a **rate-adaptive pacemaker** on the **FRDM-K64F hardware**. The repo includes the **MATLAB Simulink** file with the pacemaker logic that interfaces with the board.

This implementation interfaces with the Python DCM the team developed, that can be accessed at: [Pacemaker-GUI](https://github.com/elmongya/Pacemaker-GUI).

The four core pacing modes **(AOO, VOO, AAI, and VVI)** were enhanced to adjust the cardiac cycle based on **metabolic need**, measured using an **accelerometer**. This produced four new rate-adaptive modes: **AOOR, VOOR, AAIR, and VVIR**.

<div align="center">

<table>
  <tr>
    <th>Mode&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th><th>Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th>
  </tr>
  <tr>
    <td>AOO</td>
    <td>Atrial&nbsp;asynchronous&nbsp;pacing</td>
  </tr>
  <tr>
    <td>VOO</td>
    <td>Ventricle&nbsp;asynchronous&nbsp;pacing</td>
  </tr>
  <tr>
    <td>AAI</td>
    <td>Atrial&nbsp;demand&nbsp;pacing</td>
  </tr>
  <tr>
    <td>VVI</td>
    <td>Ventricle&nbsp;demand&nbsp;pacing</td>
  </tr>
  <tr>
    <td>AOOR</td>
    <td>Rate-adaptive&nbsp;AOO</td>
  </tr>
  <tr>
    <td>VOOR</td>
    <td>Rate-adaptive&nbsp;VOO</td>
  </tr>
  <tr>
    <td>AAIR</td>
    <td>Rate-adaptive&nbsp;AAI</td>
  </tr>
  <tr>
    <td>VVIR</td>
    <td>Rate-adaptive&nbsp;VVI</td>
  </tr>
</table>

</div>

Below is a use-case diagram outlining the available modes for the patients, as well as what the DCM allows the physician to view:
<p align="center">
  <img width="700" height="500" alt="Screenshot 2025-10-23 at 11 42 16 PM" src="https://github.com/user-attachments/assets/b4b213ad-99d0-4756-8687-7f2eb52a334a" />
</p>

<p align="center">
  <em>Figure 1: Pacemaker simulation showing different pacing modes.</em>
</p>

# Overview of System
There are various subsystems including: safety layer, pacemaker logic layer, parameter input layer, output to hardware layer. 

### Pacing Subsystem
Below in Figure 2 is the overall pacing modes workflow.

<p align="center">
  <img width="500" height="407" alt="image" src="https://github.com/user-attachments/assets/d78cdebf-36fa-4b5f-8a3c-1c8d18527943" />
</p>

<p align="center">
  <em>Figure 2: Pacemaker simulation showing different pacing modes.</em>
</p>

**For example, AOO (Atrial Asynchronous Pacing):**  
Continuously paces the atrium at a rate set by the **Lower Rate Limit (LRL)**. The interval between pulses is `30000 / LRL` to match the desired beats per minute. Pulse duration and duty cycle are determined by the pulse amplitude. The atrial pulse width (APW) is the time we deliver each pulse for, so for a full cycle to have a duration of `30000 / LRL`, we subtract the APW during the time we wait between pulses.

<p align="center">
  <img width="454" height="121" alt="image" src="https://github.com/user-attachments/assets/91f0491e-86d3-4131-ac8b-e213848d2f2d" />
</p>

<p align="center">
  <em>Figure 3: Pacemaker logic for AOO mode. We can see the how outputs to the hardware are being modified at different states.</em>
</p>

<p align="center">
  <img width="219" height="149" alt="image" src="https://github.com/user-attachments/assets/6620df51-a00d-4220-9d67-aa136e701424" />
</p>

<p align="center">
  <em>Figure 4: Output visualized on Heartview software. Pulse Amplitude = 3.5 V, APW = 0.4 ms, Heartview settings = default
Expect to see that the amplitude averages around 3.5 V and the pulse width is very low at 0.4 milliseconds.  </em>
</p>

### Programmable Parameter Subsystem
<p align="center">
  <img width="175" height="298" alt="image" src="https://github.com/user-attachments/assets/a0d8cdd1-29f1-41a3-a360-b08e3fcdf46e" />
</p>

<p align="center">
  <em>Figure 5: How the inputs to the system are manipulated.</em>
</p>

### Activity-dependent Pacing Subsystem
<p align="center">
  <img width="456" height="127" alt="image" src="https://github.com/user-attachments/assets/0a6e821e-78a9-4999-858e-bc5d0546f1ae" />
  &nbsp;&nbsp;
  <img width="223" height="211" alt="image" src="https://github.com/user-attachments/assets/baa6bcd5-b531-458c-aea8-ca126b38c194" />
</p>

<p align="center">
  <em>Figure 6: Rate-adaptive pacing subsystem. Accelerometer data is mapped to an activity level (via coordinate conversion and rolling average) and used to update the pacing rate limit. This enables new rate-adaptive modes (AOOR, VOOR, AAIR, VVIR) by dynamically adjusting the pacing rate based on user activity.</em>
</p>







