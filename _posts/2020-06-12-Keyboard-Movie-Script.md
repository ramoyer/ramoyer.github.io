---
layout: post
title: Keyboard I/O
description: "Teensy with keyboard output on command "
modified: 2022-07-16
tags: [hacking keyboard teensy]
image:
  src: ./images/HICE/HICEAssembly.png
  feature: 
  credit: 
  creditlink: 
---


"Flame wars"  - campus wide chaotic email chains - are an integral part of the MIT experience. They begin when some unlucky soul forgets to BCC all the dorms and end many reply-alls later with the bee movie script, the entire Wikipedia article on Vlad the Impaler, or a few of Tolstoy's works included, alongside many pleas to "please stop replying all with 'unsubscribe' it only makes things worse"


One day I had a spare Teensy 3.2, some spare time, and the knowledge that these boards have a USB Keyboard library that allows them to input characters one at a time to any computer they are plugged into. A few quick memory calculations later, I realized I could put an entire movie script on one of these keyboards and be able to plug it into a computer and have it output this text into any text field in short order.


![Microcontroller with 3 buttons]({{ site.url }}/images/keyboard/keyboard.jpg) 
{: .image}
Hardware
{: .center-text .caption}

Finally, I set up a python script to edit the text copied from the internet into something I can paste into an Arduino sketch, and copied the result into my editor:


	# If line available
	for line in readfile:
	if(len(line.strip())>2):
		writefile.write(line.strip())
		writefile.write("\\n")
	readfile.close()
	writefile.close()

### Limitations

Computer keyboard firmware is only designed to read key presses at human speeds, so my laptop cannot read more than 50-100 characters per second. It is functional but this results in the Bee Movie script taking about 10 minutes to fully type out as opposed to a few seconds to copy and paste. Shorter phrases are a much better fit, so this can be used to have snippets of text or keyboard sequences on hand at the press of a button