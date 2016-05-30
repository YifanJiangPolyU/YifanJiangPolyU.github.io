---
title: "UAV Communication Relay for another UAV"
categories:
  - post
tags:
  - tech

layout: single
---

This project is my undergraduate final year project, and I wrote my undergraduate thesis out of it. Hopefully this short article will give you a brief idea about what I've done. This project was a teamwork, I could not complete it without my team mates Cai Lingfeng and Wan Hiu Fung. Credits also go to our supervisor Prof Wen Chih Yung.

## The Idea

<strong>Why do we need radio communication ?</strong>
{: style="text-align: center;"}

If your UAV goes out for a mission, you want to make sure that you can contact the UAV at any time, to send commands to it, and to know what it is doing. There are two reasons for this. The first one is also the most obvious: if you cannot even contact your UAV, how can you do the mission with it? The second one is, if you lose contact with your UAV, your UAV may do unexpected actions without you knowing it, such actions can damage properties, and can ever hurt or kill somebody.

The choice of radio over other technologies such as WiFi or 3G/4G mobile network is made for 2 reasons:
<ul>
  <li>Radio has longer range than WiFi </li>
  <li>Radio has shorter (and more predictable) delay than 3G/4G </li>
</ul>

Although radio generally has lower data rates.

<strong>Why relaying radio communication ?</strong>
{: style="text-align: center;"}

If your UAV does a mission in areas where you cannot directly, does it mean you have to abort the mission? With traditional UAV systems, yes. But with radio communication relay, NO! With the help of radio communication relay, the UAV can fly into areas where direct radio communication with you is blocked.

This idea is illustrated in Fig.1. If radio communication is blocked by, say, a mountain, then the use of relay can easily help us get arround the obstacle. In addition, relay can also help extend the range of communication, so you can go further with a radio transceiver of smaller power.

<figure>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/idea.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/idea.png"></a>
    <figcaption>Fig.1 UAV Radio Communication Relay</figcaption>
</figure>


## System Architecture

To demonstrate radio communication relay, a small UAV was constructed using 2 UAVs and a Ground Control Station, which we call "Mini GCS". As shown in Fig.2, names are given to the UAVs: Mom is the one that relays communication at the middle, and Son is the one that actually executes the mission. Fig.3 shows the critical subsystems of the demonstration UAV system.

<figure class="half">
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mom_son.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mom_son.png"></a>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/sys_block.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/sys_block.png"></a>
    <figcaption>Fig.2 (Left) Mom, Son, and Mini GCS</figcaption>
    <figcaption>Fig.3 (Right) Subsystems</figcaption>
</figure>

<strong> Communication </strong>
{: style="text-align: center;"}

915MHz Radio telemetry by 3DR (3DR Radio) was used to construct the communication network. A total of 4 3DR Radios were used to establish 2 independent radio data links, one between <span style="color:#008000"> Mom and GCS</span>, and the other between <span style="color:#008000"> Mom and Son</span> (Fig.4). The 2 radio links are kept interference-free using the Frequency Hopping (FHSS) technique, which is a built-in feature of 3DR Radio. The 2 radio links use different frequency hopping sequence, which are determined by a unique "Net ID" (shown in Fig.5).

<figure class="third">
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/radio.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/radio.png"></a>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/radio_setting.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/radio_setting.png"></a>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/frame.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/frame.png"></a>
    <figcaption>Fig.4 (Left) Radio Communication Architecture</figcaption>
    <figcaption>Fig.5 (Mid) Frequency Hopping Settings</figcaption>
    <figcaption>Fig.5 (Right) Format of Data Frame</figcaption>
</figure>

In order to detect errors in data transmission, a Cyclic Redundancy Checking (CRC) error checking stage was added. To do this, data is organized into frames, and the frames have fixed format (Fig.5). At then end of each frame, a 4-byte CRC code is attached. When a frame is received by UAV or GCS, the receiver will first record the CRC code received over radio (CRC\_remote), and then compute a new CRC code based on the received data (CRC\_local). If CRC\_remote is equal to CRC\_local, then the frame is error-free. Otherwise, the received frame has error(s) in it and has to be discarded. More details about CRC can be found <a href="https://en.wikipedia.org/wiki/Cyclic_redundancy_check">here</a>.

<strong> Mission Control </strong>
{: style="text-align: center;"}

<strong> Flight Control & Aircraft </strong>
{: style="text-align: center;"}

<strong> Ground Control Station (GCS) </strong>
{: style="text-align: center;"}


## System Construction & Testing
