---
title: "Building My First Custom Mechanical Keyboard"
date: 2022-09-19
last_modified_at: 2022-09-19
tags:
  - Guides
  - Mechanical Keyboards
header:
  image: /assets/images/headers/2022-09-19-building-custom-mech-header.jpg
  teaser: /assets/images/headers/2022-09-19-building-custom-mech-header.jpg
---

A coworker introduced me to the obsessive world of custom mechanical keyboards—and I've been hooked since. While I've been using mechanical keyboards since 2014 (currently the [Varmilo VA108M Sea Melody](https://en.varmilo.com/keyboardproscenium/subject_product_detailed?subjectid=110) with Cherry MX Blue switches), the prospect of putting together my own mechanical keyboard seemed daunting and not worth the effort in the past. 

To be fair, there was a kernel of truth in that assumption. First, the selection of switches (the part under each key that registers each keystroke) was much sparser then, and prebuilt keyboards were largely dominated with switches manufactured by Cherry (creators of the [Cherry MX](https://www.cherrymx.de/en/cherry-mx.html) switch line). Nowadays, the switch market is booming with different types and feels of switches, and finding a non-Cherry MX switch is all too common. Second, putting together a mechanical keyboard used to require soldering switches onto a PCB. Now, "hot swap" PCBs can be purchased with sockets that allow for easy installation and switching of different types of switches.

My reasons for building a custom mechanical keyboard (or rather, how I convinced myself to justify my purchases) were two-fold:

1. For my Sarnoff research year, I work on my computer 8+ hours a day, which means the keyboard is the primary means by which I'm conducting research. While the Broad gives us Apple keyboards to use, I was unsatisfied by their scratchy sound and flat keyboard profile. Good tools improve the overall work experience and make it fun.
2. It looks and sounds cool lol

## Component Selection

After doing some quick searches on YouTube, it was easy to just order a setup that looked or sounded cool; however, I came up with the following checklist of personal wants for my keyboard.

* Form factor: I don't use the number pad often at work, but I do use the function keys frequently for adjusting brightness/volume on my Macbook Pro, as well as the delete, home/end, and page up/down keys while editing manuscripts. Based on this, a 75% form factor board was best for me.
* Switches: I knew that I type best on tactile switches, but also needed them to be quiet (most tactile switches are loud and noisy) as to not distract my coworkers. 
* Aesthetics: I wanted the keyboard to match aesthetically. 
* Features: For whatever reason I wanted a rotary knob on my keyboard since that's the new cool keyboard fad.
* Availability: I wanted something in stock (which isn't the case for a lot of keyboard components).

I eventually opted for the following components for a silent office keyboard build (here is a good [overview on the anatomy of a keyboard](https://www.youtube.com/watch?v=1NpNygIrnaQ)):

* Frame/PCB: [Akko Mod 007S V2](https://en.akkogear.com/product/mod-007s-v2/), a south-facing[^1] 75% keyboard kit with a knob in a sleek space gray colorway; comes with aluminum and polycarbonate plates, as well as lots of foam to dampen sound
* Switches: [Gazzew Boba U4](https://www.gazzew.com/), an extremely silent tactile switch that doesn't require lubing to sound/feel good
* Keycaps: [GMK Godspeed Armstrong](https://drop.com/buy/drop-mito-gmk-godspeed-custom-keycap-set?defaultSelectionIds=963920), to match the space gray of the keyboard (I had also purchased some [Osume Tsukimi](https://www.osumekeys.com/product/tsukimi) keycaps as well; I'll keep these for another build)
* Stabilizers: [Durock V2](https://ringerkeys.com/products/durock-stabilizers), a popular screw-in stabilizer on the market; honestly there are a lot of better options these days though[^2]
* Desk mat (for home): [Keycadets With Love, Cats edition](https://drop.com/buy/keycadets-with-love-deskmat?defaultSelectionIds=965798), to keep with the space theme

## Building the Keyboard

After a two-week wait, the keyboard kit finally arrived all the way from Shenzhen, China. The kit includes the aluminum top and bottom frames, the PCB itself, aluminum and polycarbonate plates, case foam (to put between the bottom frame and PCB), poron foam (to put on top of the PCB, but under switches), plate foam (to put between the PCB and plate, on top of the poron foam), a set of plate-mounted stabilizers, rubber and poron gaskets, USB-C connector daughterboard, a coiled USB-C cable, and screws with an Allen key.

{% include figure image_path="/assets/images/posts/2022-09-19-building-custom-mech-1.jpg" alt="Akko Mod 007S V2 parts" caption="The parts included with the Akko Mod 007S V2." %}

Since I wanted to use my own screw-in stabilizers and the softer polycarbonate plate, I disassembled the factory-assembled PCB-aluminum plate sandwich. Before installing my stabilizers, I performed the Holee mod[^3] to reduce rattle using films from [KPRepublic](https://www.amazon.com/KPrepublic-Stabilizer-Stickers-stabilizer-Sticker/dp/B09M3DGFWH) and lubed the stabilizers with [Krytox 205G0](https://ringerkeys.com/products/gpl-grease). Since the Durock V2 stabilizers are designed for 1.6mm PCBs and my PCB is 1.2mm thick, I had to also use some stabilizer stickers (also from KPRepublic) to reduce any wobble. The poron film went over this, and then the stabilizers screwed into the PCB.

{% include figure image_path="/assets/images/posts/2022-09-19-building-custom-mech-2.jpg" alt="Installed stabilizers" caption="The stabilizers installed onto the PCB. I actually forgot to put the poron foam underneath the stabilizers, so I had to reinstall them." %}

After I was happy with the stabilizers (tested with their respective keys), I placed the plate foam, polycarbonate plate, and installed the switches into the board. The plate fits tightly around the stabilizers so I'm not even sure if the stabilizer foams were necessary.

{% include figure image_path="/assets/images/posts/2022-09-19-building-custom-mech-3.jpg" alt="Installed switches" caption="I recommend installing the switches outside of the frame and holding the hotswap socket from the back as each switch is installed to prevent the socket from popping out." %}

The keyboard was almost ready to go into the frame. I applied two layers of masking tape to the back of the PCB (tape mod) to deepen the sound and reduce high-pitch noises, and then applied masking tape to where the top and bottom aluminum frames contact to reduce metal pinging noises (force break mod).

{% include figure image_path="/assets/images/posts/2022-09-19-building-custom-mech-4.jpg" alt="Force break mod" caption="Masking tape was applied at all the metal-on-metal contact points to reduce pinging in the keyboard." %}

Finally, I was able to install the keycaps and screw the top and bottom halves together. Below is the final result:

{% include figure image_path="/assets/images/posts/2022-09-19-building-custom-mech-5.jpg" alt="Final assembled keyboard" caption="Keyboard alongside the fun characters on my desk mat." %}

And as is typical with new keyboard builds, here is a sound test (note that the sound here is not exactly representative of what it sounds like in real life):

{% include video id="SbuseZBJMQ0" provider="youtube" %}

[^1]: See [this post on north versus south facing switches](https://keyboardable.com/north-facing-switches-vs-south-facing-switches/)
[^2]: See [this post for a comparison of popular stabilizers](https://www.keebsnstuff.com/blog/stabilisers-an-in-depth-review).
[^3]: See [this video for a showcase of various stabilizer mods](https://www.youtube.com/watch?v=4XCErBcn5lc)
