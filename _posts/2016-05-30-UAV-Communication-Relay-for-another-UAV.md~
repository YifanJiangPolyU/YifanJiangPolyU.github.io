---
title: "UAV Communication Relay for another UAV"
categories:
  - post
tags:
  - tech

layout: single
---

This project is my undergraduate final year project, and I wrote my undergraduate thesis out of it. Hopefully this short article will give you a brief idea about what I've done. This project was a teamwork, I could not complete it without my team mates Cai Lingfeng and Wan Hiu Fung. I would also like to thank our supervisor Prof Wen Chih Yung, an absolutely nice and helpful prof.

This project has helped us win <strong>champion</strong> at the <a href="http://www.iaa.ncku.edu.tw/~whlai/uav/2016/index.html">2016 Taiwan Unmanned Aircraft Design Competition</a> (website in Chinese), in which we take great pride. we are also looking to publish a paper based on it.



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
    <figcaption>Fig.6 (Right) Format of Data Frame</figcaption>
</figure>

In order to detect errors in data transmission, a Cyclic Redundancy Checking (CRC) error checking stage was added. To do this, data is organized into frames, and the frames have fixed format (Fig.6). At then end of each frame, a 4-byte CRC code is attached. When a frame is received by UAV or GCS, the receiver will first record the CRC code received over radio (CRC\_remote), and then compute a new CRC code based on the received data (CRC\_local). If CRC\_remote is equal to CRC\_local, then the frame is error-free. Otherwise, the received frame has error(s) in it and has to be discarded. More details about CRC can be found <a href="https://en.wikipedia.org/wiki/Cyclic_redundancy_check">here</a>.

<strong> Mission Control </strong>
{: style="text-align: center;"}

This mission control unit is used to do a number of jobs:

1. It handles message routing and communication relay. 
2. It interfaces GCS to flight controller. User can send relatively simple commands from GCS, then mission control translates them into the low-level instructions to the flight controller.
3. It is supposed to control the mission with intelligence, sadly we still haven't implemented that (will we ?).

The mission control unit is in fact a software system, developed with <a href="http://www.ros.org/">ROS</a>, than runs on a <a href="https://www.raspberrypi.org/products/raspberry-pi-2-model-b/">Raspberry Pi 2B</a>. The mission control unit communicates with radio telemetries and flight controller through UART ports. So it means as long as you are using a UAV with supported flight controller (in our case, the <a href="https://pixhawk.org/choice">PixHawk Flight Controller</a>), then you can plug this mission control unit into it and turn it into an on-air communication relay station! The system is totally platform independent, which is cool and convenient.


<strong> Flight Control & Aircraft </strong>
{: style="text-align: center;"}

There is nothing very special with the actual aircraft used. For testing, we built 2 quad-rotor copters out of off-the-shelf components, and used them as Mom and Son. For flight controller, we selected <a href="https://pixhawk.org/choice">PixHawk Flight Controller</a> because it is powerful and plenty of resources are available online. Fig.8 shows the UAVs during a field flight test.

<figure class="half">
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mission-control.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mission-control.png"></a>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/test.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/test.png"></a>
    <figcaption>Fig.7 (Left) Mission Control Unit Mounted on UAV</figcaption>
    <figcaption>Fig.8 (Right) UAV Mom and Son, during a test</figcaption>
</figure>

<strong> Ground Control Station (GCS) </strong>
{: style="text-align: center;"}

We developed our own version of GCS software, which we call "Mini GCS". It was writen fully in-house using C#. A screen shoot is given in Fig.9 The Mini GCS has a number of cool features:

1. A UAV status window, which allows you to monitor BOTH UAVs at the same time.
2. A map display, that shows the location of both UAVs in real time.
3. A Linux command line style interface, in which you can type commands and send them to the UAV that you specify, Mom or Son.

A number of useful commands:

	arm              be ready to fly, engage motors
	takeoff	         takeoff and fly to specified height
	land             land immediately
	setmode          change to flight mode of the UAV
	rtl              return to home position, and land there

<figure>
    <a href="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mini-gcs.png"><img src="/images/2016-05-30-UAV-Communication-Relay-for-another-UAV/mini-gcs.png"></a>
    <figcaption>Fig.9 Mini GCS screen shoot (UAVs not yet displayed on map)</figcaption>
</figure>


## Testing and results


