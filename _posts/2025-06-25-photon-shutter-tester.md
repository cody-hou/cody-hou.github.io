---
title: "Creating a Simple Shutter Tester with the Particle Photon"
date: 2025-06-25
last_modified_at: 2025-06-25
tags:
  - Guides
  - Photography
header:
   image: /assets/images/headers/2025-06-25-photon-shutter-tester-header.jpg
   teaser: /assets/images/headers/2025-06-25-photon-shutter-tester-header.jpg
---

Having amassed some old film cameras, I wanted to test the shutter speeds to determine if they are in dire need of servicing. I had a Particle Photon development kit that conveniently includes a resistor and phototransistor, which is all that is needed to set this up.

I based the wiring off of [this tutorial](https://docs.particle.io/getting-started/hardware-tutorials/hardware-examples/) on Particle's website. I wired the short leg of the phototransistor to the 3.3V pin and the long leg to the A5 pin. On the A5 rail, a transistor connects to ground.

{% include figure image_path="https://docs.particle.io/assets/images/phototransistor-photon.jpg" alt="Phototransistor Wiring" caption="This image shows a general gist of how things are wired, except with the phototransistor to A0 instead of A5. Ignore the LED wiring. Image sourced from [Particle Docs](https://docs.particle.io/getting-started/hardware-tutorials/hardware-examples/)." %}

Using Particle's Cloud IDE, I modified [code by user hiroshootsfilm](https://projecthub.arduino.cc/hiroshootsfilm/8ffb045a-c17c-4c81-b100-065718f7fb1d) to work specifically with the Photon.

~~~ c++
// Shutter Tester for Particle Photon
// Derived from hiroshootsfilm
// https://projecthub.arduino.cc/hiroshootsfilm/shutter-speed-tester-for-film-cameras-8ffb04

// Include Particle Device OS APIs
#include "Particle.h"

// Let Device OS manage the connection to the Particle Cloud
SYSTEM_MODE(AUTOMATIC);

// Show system, cloud connectivity, and application logs over USB
// View logs with CLI using 'particle serial monitor --follow'
SerialLogHandler logHandler(LOG_LEVEL_INFO);

const int phototransistor = A5;
const int threshold = 200; // Calibrate to phototransistor and ambient light
const int timeout = 8000000; // In microseconds
const int min_duration = 500; // In microseconds

// setup() runs once, when the device is first turned on
void setup() {
  // Put initialization like pinMode and begin functions here
  pinMode(phototransistor, INPUT);
  pinMode(status_led, OUTPUT);
  
  // Turn off status LED
  LEDSystemTheme theme;
  theme.setColor(LED_SIGNAL_CLOUD_CONNECTED, 0x00000000);
  theme.apply();
}

// loop() runs over and over again, as quickly as it can execute.
void loop() {
    unsigned long duration = measure_shutter(phototransistor, timeout, threshold);
    
    if (duration > min_duration) {
        double msecs = duration / 1000.0;
        String text = String("Time: ") + msecs + String(" msecs");
        Log.info(text);
        delay(1000);
    }  
}

// Shutter duration measuring function
unsigned long measure_shutter(int pin, unsigned long timeout, int threshold) {
    unsigned long start_time = micros();
    
    // Wait until the previous pulse stops.
    while (true) {
        unsigned int value = analogRead(phototransistor);
        if (value < threshold) {
            break;
        }
        if (micros() - start_time >= timeout) {
            return 0;
        }
    }

    // Wait until the pulse starts.
    while (true) {
        unsigned int value = analogRead(pin);
        if (value >= threshold) {
            Log.info("Pulse detected, waiting for shutter to close.");
            break;
        }
        if (micros() - start_time >= timeout) {
            return 0;
        }
    }

    unsigned long time0 = micros();

    // Wait until the pulse stops.
    while (true) {
        unsigned int value = analogRead(pin);
        if (value < threshold) {
            Log.info("Shutter closed.");
            break;
        }
        if (micros() - start_time >= timeout) {
            return 0;
        }
    }

    unsigned long end_time = micros();
    return (end_time - time0);
}
~~~

For SLRs, I found it best to mount the phototransistor/board at the lens mount area, and place the light source at the film back as shown below. 

{% include figure image_path="/assets/images/posts/2025-06-25-photon-shutter-tester-1.jpg" alt="Shutter Tester Placement" caption="Here I'm testing a Nikon Nikkormat FT2." %}

Choosing a light source is also really important. I was getting constant pulses of 4.5 milliseconds when using some Energizer flashlights and wasn't able to measure anything longer. It turns out that some flashlights cycle between light and dim every couple of milliseconds, far too fast for human eyes to see. I used my phone flashlight instead and have had good results.

The Photon is plugged into the computer running the serial monitor over the CLI with `particle serial monitor --follow`. Here is some sample output when I tested a Nikon Nikkormat FT2 shutter at 1/1000 s:

~~~
0000236496 [app] INFO: Pulse detected, waiting for shutter to close.
0000236498 [app] INFO: Shutter closed.
0000236498 [app] INFO: Time: 1.123000 msecs
0000241727 [app] INFO: Pulse detected, waiting for shutter to close.
0000241729 [app] INFO: Shutter closed.
0000241729 [app] INFO: Time: 1.123000 msecs
~~~

We can see that it's measuring at 1.123 milliseconds with an expected shutter length of 1 millisecond. The formula `log₂(expected/measured)` should give us the number of stops that we are off by the target. Doing the math, I calculate that `log₂(1/1.123)` is approximately -0.167, so this shutter is underexposing by roughly 1/6th of a stop of 1/1000. That's great for a camera this age; long live the Copal shutter!

However, there are some nuances when it comes to interpreting these speeds because of how the shutter curtains work, particularly at higher speeds like 1/1000. A laser-based shutter tester system or something that accounts for the curtains would be better. At any rate, this simple shutter tester gives me a great starting point to figure out which cameras are in serious need of CLA. 
