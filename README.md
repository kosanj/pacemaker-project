## Pacemaker Simulation Project

This project involved simulating a **rate-adaptive pacemaker** on the **FRDM-K64F hardware**. The repo includes the MATLAB file with the pacemaker logic that interfaces with the board.

The four core pacing modes **(AOO, VOO, AAI, and VVI)** were enhanced to adjust the cardiac cycle based on **metabolic need**, measured using an **accelerometer**. This produced four new rate-adaptive modes: **AOOR, VOOR, AAIR, and VVIR**.

| Mode  | Description                           |
|-------|---------------------------------------|
| AOO   | Atrial asynchronous pacing             |
| VOO   | Ventricle asynchronous pacing          |
| AAI   | Atrial demand pacing                   |
| VVI   | Ventricle demand pacing                |
| AOOR  | Rate-adaptive AOO                      |
| VOOR  | Rate-adaptive VOO                      |
| AAIR  | Rate-adaptive AAI                      |
| VVIR  | Rate-adaptive VVI                      |

# Overview of System
There are various subsystems including: safety layer, pacemaker logic layer, parameter input layer, output to hardware layer. Below in Figure 1 is the overall pacing modes workflow.

<p align="center">
  <img width="500" height="407" alt="image" src="https://github.com/user-attachments/assets/d78cdebf-36fa-4b5f-8a3c-1c8d18527943" />
</p>

<p align="center">
  <em>Figure 1: Pacemaker simulation showing different pacing modes.</em>
</p>

**For example, AOO (Atrial Asynchronous Pacing):**  
Continuously paces the atrium at a rate set by the **Lower Rate Limit (LRL)**. The interval between pulses is `30000 / LRL` to match the desired beats per minute. Pulse duration and duty cycle are determined by the pulse amplitude. The atrial pulse width (APW) is the time we deliver each pulse for, so for a full cycle to have a duration of `30000 / LRL`, we subtract the APW during the time we wait between pulses.

<p align="center">
  <img width="454" height="121" alt="image" src="https://github.com/user-attachments/assets/91f0491e-86d3-4131-ac8b-e213848d2f2d" />
</p>

<p align="center">
  <em>Figure 2: Pacemaker logic for AOO mode. We can see the how outputs to the hardware are being modified at different states.</em>
</p>

<p align="center">
  <img width="219" height="149" alt="image" src="https://github.com/user-attachments/assets/6620df51-a00d-4220-9d67-aa136e701424" />
</p>

<p align="center">
  <em>Figure 3: Output visualized on Heartview software. Pulse Amplitude = 3.5 V, APW = 0.4 ms, Heartview settings = default
Expect to see that the amplitude averages around 3.5 V and the pulse width is very low at 0.4 milliseconds.  </em>
</p>

**Programmable parameter subsystem**
<p align="center">
  <img width="175" height="298" alt="image" src="https://github.com/user-attachments/assets/a0d8cdd1-29f1-41a3-a360-b08e3fcdf46e" />
</p>

<p align="center">
  <em>Figure 4: How the inputs to the system are manipulated.</em>
</p>

**Activity-dependent Pacing**
<p align="center">
  <img width="456" height="127" alt="image" src="https://github.com/user-attachments/assets/0a6e821e-78a9-4999-858e-bc5d0546f1ae" />
  &nbsp;&nbsp;
  <img width="223" height="211" alt="image" src="https://github.com/user-attachments/assets/baa6bcd5-b531-458c-aea8-ca126b38c194" />
</p>

<p align="center">
  <em>Figure 5: Rate-adaptive pacing subsystem. Accelerometer data is mapped to an activity level (via coordinate conversion and rolling average) and used to update the pacing rate limit. This enables new rate-adaptive modes (AOOR, VOOR, AAIR, VVIR) by dynamically adjusting the pacing rate based on user activity.</em>
</p>










