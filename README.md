pll-playground
==============
[让频率提升几十倍的电路！锁相环的工作原理！ - YouTube](https://www.youtube.com/watch?v=YtlwIFsiWtE)
https://www.cppsim.com/manuals.html#one

Wi-Fi-style Charge-Pump Fractional-N PLL


No, the exact multiplier-based analog PLL shown in that template is not what a modern Wi-Fi module uses.
While the fundamental feedback concept is identical, a Wi-Fi module operates at millimeter-wave and microwave frequencies (2.4 GHz, 5 GHz, or 6 GHz). At these extreme speeds, pure analog multipliers are too noisy, drift too much with temperature, and cannot easily perform precision channel selection.
Instead, modern Wi-Fi chips use highly optimized Charge-Pump Fractional-N Digital/Mixed-Signal PLLs (often integrated into a Radio Frequency Integrated Circuit, or RFIC).

What a Wi-Fi PLL Actually Looks Like

If you open up the RF synthesizer section of a modern Wi-Fi chip (like a Broadcom, Qualcomm, or Realtek module), the PLL consists of these specific blocks:
                  +-------------------------------------------------+

                  |                   Wi-Fi RFIC                    |
                  |                                                 |
+-----------+     |  +---------+     +--------+     +-------------+ |     +-----------+

|  40 MHz   |---->|  | Phase/  |---->| Charge |---->| Analog Loop | |--->|  2.4/5GHz |
| Reference |     |  | Freq Det|     |  Pump  |     |   Filter    | | |   | LO Signal |
|  Crystal  |     |  |  (PFD)  |     +--------+     +-------------+ | |   +-----------+
+-----------+     |  +---------+                           |        | |

                  |       ^                                v        | |
                  |       |                          +------------+ | |
                  |       |                          | Analog VCO | |-+
                  |       |                          |  (LC Tank) | |
                  |       |                          +------------+ |
                  |  +-------------------+                 |        |
                  |  |    Fractional     |<----------------+        |
                  |  | Divider (N / N+1) |                          |
                  |  +-------------------+                          |
                  +-------------------------------------------------+

1. The VCO is an LC Tank (Not a Ring Oscillator)

• What it uses: An inductor-capacitor (LC Tank) oscillator using a varactor diode (voltage-controlled capacitor).
• Why: At 5 GHz, ring oscillators have terrible phase noise (jitter). An LC tank acts like a high-quality tuning fork, keeping the Wi-Fi signal sharp so it doesn't bleed into neighboring channels.

2. The Phase Detector is Digital (PFD + Charge Pump)

• What it uses: Flip-flop-based Phase Frequency Detectors (PFD) connected to a Charge Pump.
• Why: The multiplier-based analog PD from the template can get confused if the frequencies are too far apart (it has a narrow capture range). A digital PFD measures both the edge timing (phase) and the speed (frequency), allowing the Wi-Fi module to instantly adapt when you change channels.

3. Fractional-N Frequency Dividers

• What it uses: A digital multi-modulus divider controlled by a Sigma-Delta (ΣΔ) Modulator.
• Why: Your reference clock is usually a stable, cheap 40 MHz quartz crystal. To step up 40 MHz to exactly 5.180 GHz (Wi-Fi Channel 36), the PLL must multiply the reference by 129.5. A pure analog circuit cannot multiply by a fraction. A digital divider rapidly toggles its division ratio between 129 and 130 to average out to exactly 129.5.

Why do engineers still study the "Pure Analog" model?

If Wi-Fi doesn't use it, why simulate the pure analog multiplier version?
• Mathematical Equivalence: When a Wi-Fi PLL is very close to locking, the digital switches and charge pump average out. Mathematically, it begins to behave exactly like a continuous-time analog multiplier.
• Macro-Modeling: Simulating a 5 GHz digital divider in SPICE requires tracking billions of steps, which crashes or freezes computers. Engineers substitute the digital blocks with analog equations (like the template) to simulate the overall system stability, lock time, and filtering in seconds rather than weeks.
Are you designing a circuit for a specific frequency band (like RF/Wi-Fi), or are you looking to understand how to model the digital Phase Detector and Charge Pump inside Ngspice to get closer to a real chip architecture?

### Tutorials
- [锁相环 (PLL) 瞬态响应 | Video | TI.com](https://www.ti.com/video/6346291003112)
- https://www.analog.com/media/en/training-seminars/tutorials/MT-086.pdf
- http://www.metrology.pg.gda.pl/full/2005/M&MS_2005_427.pdf
- [Basic operation of PLL (Phase Locked Loop)](https://gate.ruru.ne.jp/rfdn/technote/baseplltech.asp)
- [What is Voltage Controlled Oscillator? Definition, types and working of Voltage Controlled Oscillator - Electronics Coach](https://electronicscoach.com/voltage-controlled-oscillator.html)
- [Building a Voltage-Controlled Oscillator (VCO) - element14 Community](https://community.element14.com/technologies/open-source-hardware/b/blog/posts/building-a-voltage-controlled-oscillator-vco)
- [Phase Locked Loop Tutorial: the basics of PLLs - YouTube](https://www.youtube.com/watch?v=A9qt0JYdvFU&t=157s)
- [How to Get Phase From a Signal (Using I/Q Sampling) - YouTube](https://www.youtube.com/watch?v=Ev3lZClnLhQ)
- atan2
- [Determining Phase Shift Between Sinusoidal Signals - YouTube](https://www.youtube.com/watch?v=9-JTCULUqjI)
- [Phase Lock Loop (Basics, Block Diagram, Parameters, Working, Capture Range & Lock Range) Explained - YouTube](https://www.youtube.com/watch?v=LCfFhAz6U2E&list=PLgwJf8NK-2e4jg67BwscGFSvpjJ_3-Yrx)
- [Phase Locked Loop Tutorial: the basics of PLLs - YouTube](https://www.youtube.com/watch?v=A9qt0JYdvFU&t=157s)

