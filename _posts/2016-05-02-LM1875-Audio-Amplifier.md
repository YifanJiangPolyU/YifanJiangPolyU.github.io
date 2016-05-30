---
title: "Audio Amplifier based on LM1875"
categories:
  - post
tags:
  - tech
  - DIY

layout: single
---

I am not (yet) a big fan of Hi-Fi audio. However, driven by some interests and some willingness to explore new things, I decided to build my own in-house audio amplifier, using the famous LM1875 audio power amplifier IC. LM1875 is a (supposedly) easy-to-use audio amplifier IC, with numerous reference designs available from the internet. For a first-time try-out, using this IC should guarantee reasonable sound quality without too much debugging efforts. Staying within beginner-friendly region, at least for the first few times, is always smart.

## Design

<strong>The Amplifier Circuit</strong>
{: style="text-align: center;"}

As I mentioned earlier, LM1875 is a pretty easy-to-use audio amplifier IC. According to its manufacturer, Texas Instruments, LM1875 is a high-performance and low-distortion amplifier designed to operate with minimum external components. To work out my own design, I referred to a number of designs made by other audio hobbyists as well as the reference design provided in LM1875's datasheet. The following sources have been valuable:
<ul>
  <li> LM1875 datasheet, available at <a href="http://www.ti.com/product/LM1875">here</a> </li>
  <li> An audio hobbyist's design, unknown author, available at <a href="https://sites.google.com/site/amplificatoare/lm1875-audio-amplifier-20w">here</a> </li>
</ul>

Fig.1 shows the schematic of the amplifier circuit. The supply voltages, V+ and V-, are +16V and -16V respectively. The closed loop gain of the circuit is about 26.4dB. Resistor R6 and capacitor C9 form a low-pass filter, screening out high-frequency noise from input. Inductors L1 and L2 and resistor R12 form another low-pass filter, taking out high-frequency noise from amplifier output. Audio signal input and feed-back GND is isolated from the real power ground by a 10-ohm resistor, R7. This design further reduces noise. 

<strong>The Power Supply Circuit</strong>
{: style="text-align: center;"}

Fig.2 shows the schematic of the power supply circuit. The power supply unit takes in 12VAC, rectifies and filters it, giving out +-16VDC outputs. I may have added more filtering capacitors than actually needed. But it helps reduce output voltage ripples.

<figure class="half">
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/amplifier-circuit.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/amplifier-circuit.png"></a>
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/power-supply-circuit.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/power-supply-circuit.png"></a>
    <figcaption>Fig.1 (Left) Amplifier circuit schematic</figcaption>
    <figcaption>Fig.2 (Right) Power supply circuit schematic</figcaption>
</figure>

<strong>Others Circuits</strong>
{: style="text-align: center;"}

The amplifier also features a number of other circuits:

1. A simple in-rush current limiter (ICL) circuit, to slow down the charging process of power supply capacitors on power-up (Fig.3).
2. A manual-controlled speaker protection circuit, protecting the speakers from the start-up transients. The speakers are simply disconnected from amplifier output when the amplifier is powering up. After full power-up, the speakers are re-connected. This is realized using 2 relays, which are in turn controlled manually, using a switch (Fig.4).
3. A potentiometer for volume adjustment. It in fact builds 2 potentiometers on the same axis, allowing volume of both Left and Right channels to be adjusted equally. On the same axis there is also a rotary switch, which I use to control the speaker protection circuit.

<figure class="half">
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/ICL-circuit.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/ICL-circuit.png"></a>
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/speaker-protection-circuit.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/speaker-protection-circuit.png"></a>
    <figcaption>Fig.3 (Left) Inrush current limiter (ICL) circuit schematic</figcaption>
    <figcaption>Fig.4 (Right) Speaker protection circuit schematic</figcaption>
</figure>

The working principle of the ICL circuit is simple. As shown in Fig.3, on power-up, the relay SW4 is in OFF state, and AC current has to flow through the 56-ohm resistor R-ICL, which limits the charging current of the power supply capacitors. When the power supply output voltage rises to a level (around 12V), SW4 is activated (into ON state), short-connecting R-ICL. The power supply then finishes charging, and no current flows through R-ICL in normal amplifier operation. The 2 terminals, V-IN and PE1, are used to sense the power-supply output voltage. With the ICL circuit, the power supply fully starts up in around 8 seconds.

<strong>The PCBs</strong>
{: style="text-align: center;"}

Fig.5 and Fig.6 shows the PCB layouts of the amplifier circuit and power supply circuit. I quickly sketched them using Eagle, and am not sure whether everything is perfect. A few useful guidelines in the amplifier circuit PCB layout:
<ul>
  <li> Separate small-signal (audio input) and large-signal (amplifier power supply and output) GND. The 2 joins together only at a single point. </li>
  <li> Use star-grounding whenever possible. </li>
  <li> Try not to let signal and power paths cross one another in different layers, to minimise cross-coupling of noise (in fact I tried to layout all the traces in the same layer). </li>
</ul>

<figure class="half">
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/amp-pcb.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/amp-pcb.png"></a>
    <a href="/images/2016-05-02-LM1875-Audio-Amplifier/power-supply-pcb.png"><img src="/images/2016-05-02-LM1875-Audio-Amplifier/power-supply-pcb.png"></a>
    <figcaption>Fig.5 (Left) Amplifier circuit PCB</figcaption>
    <figcaption>Fig.6 (Right) Power supply circuit PCB</figcaption>
</figure>

to be continued.

