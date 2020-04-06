---
layout: post
title: MIT ID Reader
description: "RFID reader for 125 khz MIT IDs"
modified: 2020-04-03
tags: [hacking electronics circuit design rfid radio teensy arduino]
---

![Micro chip]({{ site.url }}/images/card-reader/teensy.jpg)
{: .image}

### Background

MIT's 6.002, Circuits and Electronics, has a [lab where they teach students how to build a radio circuit to read their ID cards.](https://6002.catsoop.org/F19/lab9)  Since I took the class in the fall of 2017, the circuit design has changed and the instructors now include software that can read and display the binary card data, but when I took the class this didn't exist so I built my own.  It's also a significantly different approach than what the class uses, but my design uses far less parts.  I built this in the summer of 2018 in between research projects and my work with MIT EMS.

### Circuit Details

MIT ID cards us a nominal 125khz RFID chip that recieves power at 125khz and transmits 1/2 and 3/2 of this drive frequency. Data is transmitted on this 62.5khz signal with phase shift keying and 16 cycles/bit.  To retrieve this data, we can use one coil to send a 125khz signal and a second coil to recieve the combined 125khz and 62.5khz signals.  I used a 62.5khz resonant circuit connected to the recieving coil, and connected this to my chip's analog input for processing. Here's the full breadboard layout:

![Card Reader Breadboard Layout]({{ site.url }}/images/card-reader/reader.jpg)
{: .image}

### Software

#### Initial plan: 
* Capture data at at least twice the carrier frequency, then use signal processing to analyze this
	* Exactly twice the carrier frequency may not capture the wave, so I probably need 4 samples/cycle to accurately reproduce it
	* Sampling with an Arduino at 250khz is hard, the base data rate for the ADC is 15.6 khz, as the default is to average 1024 samples together. Additionally, we run out of memory for 4 samples/cycle x 16 cycles/bit x 224 bits/sample 

Also, our data is noisy and signal processing is hard.  

However, I realized that if we provide the driving 125khz signal from our microchip, we can predict exactly when the peaks and valleys of the recieved signal are, and just measure these peaks.  

Since these cards use phase-shift keying, each analog measurement will either be at a peak or a valley for 16 consecutive samples, indicating a 1 or 0.  This was then implemented with 20,000 samples, providing enough space to record the complete card data about 5 times.  In order to sample each peak fast enough and to store all this data, I switched from an Arduino to a Teensy 3.2 I had on hand. Here's a sample of the waveform that the Teensy sends back to my computer:

![Data Waveform]({{ site.url }}/images/card-reader/waveform.png)
{: .image}

### Data Processing

Once we have analog measurements at each wave peak, converting this into binary data is simple:
1. Compute the average of the voltage readings
2. Assign a 0 to any voltage less than the average, and a 1 to any voltage greater than the average
3. Remembering that I have 16 data points per card bit, find a string of 30 0s in the card data. These are the start bits. 
4. Sample every 16 points thereafter to read off the card data

I originally sent the raw data from my chip to my computer and processed this all in Matlab, but it can be done on the chip itself, or with any other software program.

### Future

I wanted to be able to write this to a chip so I could have a copy of my card in my phone case or my watch - however, the complexity of writing this data to a chip is much higher than just reading it and I ran out of time before the summer ended.

![Card Reader with card]({{ site.url }}/images/card-reader/reader-with-card.jpg)
{: .image}


