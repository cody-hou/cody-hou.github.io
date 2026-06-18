---
title: "Updating Firmware on the Olympus E-300 and E-500 Cameras"
date: 2026-06-17
last_modified_at: 2026-06-17
tags:
  - Guides
  - Photography
header:
  image: /assets/images/headers/2026-06-17-updating-olympus-e-300-e-500-header.JPG
  teaser: /assets/images/headers/2026-06-17-updating-olympus-e-300-e-500-header.JPG
gallery_update:
  - url: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-2.JPG
    image_path: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-2.JPG
    alt: "E-500 before"
    title: "Body firmware 1.0."
  - url: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-3.JPG
    image_path: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-3.JPG
    alt: "Updating E-500"
    title: "Card access lamp illuminated."
  - url: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-4.JPG
    image_path: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-4.JPG
    alt: "E-500 after"
    title: "Body firmware successfully updated to 1.3."
gallery_samples:
  - url: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-5.JPG
    image_path: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-5.JPG
    alt: "Gingko"
    title: "New gingko leaves. E-300, Zuiko Digital 40-150mm f/3.5-4.5 @ 108mm (216mm equivalent), f/4.2, ISO 100, 1/800s."
  - url: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-6.JPG
    image_path: /assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-6.JPG
    alt: "Sunset"
    title: "Sunset over the Mississippi. E-300, Zuiko Digital 40-150mm f/3.5-4.5 @ 62mm (124mm equivalent), f/8, ISO 100, 1/500s."
---

I've been sucked into the trend of CCD sensor cameras, and the Olympus E-300 and E-500 are well-known for their Kodak-made KAF-8300CE sensors. These cameras, released in 2004 and 2005 respectively, are noted to produce images straight out of camera with vibrant, punchy colors.

Through browsing my local Facebook Marketplace classifieds, I obtained one of each of these bodies very cheaply. The E-300 came with the respectable Zuiko Digital 14-42mm f/3.5-5.6 and 40-150mm f/3.5-4.5 kit lenses for $50, while the E-500 came with an attractive Zuiko Digital 12-54mm f/2.8-3.5 lens for $75. Each only had a shutter count of ~3500 and were in excellent condition after some cleaning.

{% include figure image_path="/assets/images/posts/2026-06-17-updating-olympus-e-300-e-500-1.JPG" alt="" caption="The E-500 (left) and E-300 (right), along with their lenses. I won't discuss the differences between the cameras in detail in this post, but the E-300 has a very distinctive chunky two-tone design that is a relic of the early analog-to-digital transition which is shed on the E-500." %}

Both of my bodies came with camera firmware version 1.0. While not strictly necessary, upgrading firmware can improve the overall shooting experience (e.g. in the case of the E-300, improved metering accuracy). This used to be possible through Olympus' OM Workspace software, but these files are unfortunately no longer hosted on their website. Fortunately, the firmware has been archived via the Wayback Machine, so updating is still possible.

Downloading the incorrect firmware or attempting to update with corrupted firmware can brick your camera. Proceed at your own risk! Reference the images below.
{: .notice--warning}

1. Confirm that the body firmware is not the newest version. On both E-300 and E-500, it is accessed via Menu → 🔧2 → FIRMWARE. Do not confuse the body firmware version with the lens firmware version.

2. Download the correct firmware from one of the following links:

    * E-300 camera firmware, version 1.5 download: [Wayback Machine](https://web.archive.org/web/20240628205426/https://dl01.om-digitalsolutions.net/OMDS/FIRMWARES/0001/0120/OLY_E_012_1501_0000_0000.BIN) (SHA256 checksum: `4ec107f7cc9bbeb04cb378e879627eb65efd7aadb3e0bb38d3d564ae6333dde4`)
    * E-500 camera firmware, version 1.3 download: [Wayback Machine](https://web.archive.org/web/20250201203957/https://dl01.om-digitalsolutions.net/OMDS/FIRMWARES/0001/0350/OLY_E_035_1303_0000_0000.BIN) (SHA256 checksum: `e9f5431cb47bf1c73395337256fba6eedf576c03b06c4ef7dea0d387ca7c69ae`)

2. Take a known good CompactFlash card and completely format it within the camera body.

    * E-300: Menu → 📷️2 → CARD SETUP → FORMAT
    * E-500: Menu → 📷️1 → CARD SETUP → FORMAT

3. Create a folder on the CompactFlash root directory named `DCOLYMP` and drag the firmware `.BIN` downloaded previously into this new folder.

    * For the E-300, rename the firmware file to `E0129999.BIN`.
    * For the E-500, rename the firmware file to `E0359999.BIN`.

4. Ensure the battery is fully charged before proceeding! Insert the CompactFlash card. While holding down the OK button, turn on the camera. The card access lamp located on the back will illuminate constant red while the firmware is updating. Once the light begins blinking on and off, turn off the camera; the firmware update is complete.

{% include gallery id="gallery_update" caption="The updating process: E-500 body firmware initially 1.0, updated to the latest 1.3." %}

And that's it; should be off to the races with those Kodak CCD colors.

{% include gallery id="gallery_samples" caption="Sample images from the E-300, all unedited and straight out of camera." %}
