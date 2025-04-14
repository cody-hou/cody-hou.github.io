---
title: "The Elder Scrolls V: Skyrim - Special Edition Modding Guide"
date: 2025-04-08
last_modified_at: 2025-04-08
tags:
  - Guides
header:
  image: /assets/images/headers/2025-04-08-skyrim-se-modding-guide-header.jpg
  teaser: /assets/images/headers/2025-04-08-skyrim-se-modding-guide-header.jpg
gallery1:
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-1.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-1.jpg
    alt: "Foggy Mountains"
    title: "Viewing west in The Reach, just outside of Bthardamz."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-2.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-2.jpg
    alt: "Snowy Path"
    title: "Intense snowstorm just outside of Morthal."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-3.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-3.jpg
    alt: "Aurora"
    title: "Snow-covered trees and aurora, south of Red Road Pass in The Pale"
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-4.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-4.jpg
    alt: "Morning Sun"
    title: "Morning sun rising over the southern route into Riften."
gallery2:
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-5.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-5.jpg
    alt: "Honeyside"
    title: "Warm light streaks into the windows of Honeyside in Riften."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-6.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-6.jpg
    alt: "Icy Interior"
    title: "In contrast, icicles and cool light streak inside of Castle Volkihar."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-7.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-7.jpg
    alt: "Smoky Inn"
    title: "You can smell the smoke inside of inns now. Can't remember which one this one is.."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-8.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-8.jpg
    alt: "Skylight"
    title: "More visual candy, this time in the form of a skylight in the Chantry of Auri-El within the Forgotten Vale."
gallery3:
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-9.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-9.jpg
    alt: "Whiterun Day"
    title: "Warm sunny weather with the statue of Talos and Gildergreen in Whiterun."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-10.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-10.jpg
    alt: "Solitude"
    title: "Looking toward the Blue Palace with flurries in Solitude."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-11.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-11.jpg
    alt: "Winterhold"
    title: "The Statue of Azura overlooks what is left of Winterhold."
  - url: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-12.jpg
    image_path: /assets/images/posts/2025-04-08-skyrim-se-modding-guide-12.jpg
    alt: "Whiterun Night"
    title: "An alternate view of Whiterun's Gildergreen, this time with Dragonsreach and a night sky."
---

I have been playing The Elder Scrolls V: Skyrim on and off ever since its launch in 2011. Despite its age, the game continues to be loved by many fans, including a very large and active modding community. Although I upgraded to the Special Edition (SE) in 2020, which includes improved textures and graphics, the game has started to feel a bit dated. Along with starting a battlemage playthrough (a departure from my typical sneak archer playstyle), I decided to mod the game's visuals.

{% include gallery id="gallery1" layout="half" caption="Modding adds new character to the various landscapes of Skyrim." %}

Modding the game visually has really enhanced my gaming experience. Though I have been playing with a self-imposed no fast-travel rule, the enhanced lighting, textures, and weather mechanics makes it really enjoyable to walk through the different climates of Skyrim, and I find myself taking countless screenshots of the game. Especially on a 4K OLED screen and controller, the game becomes a relaxing couch game.

{% include gallery id="gallery2" layout="half" caption="Interior lighting gets a huge facelift and is more realistic (and dark; bring a torch or Magelight spell)." %}

While there are also many mods to drastically change the gameplay and feel of the game, my mods focus on making the game visually impressive and immersive. I can easily run these at 60 FPS on an AMD RX 6950 XT at 1440p, and can also achieve a solid 55-60 FPS depending on the landscape (grass seems to tank FPS) on an Nvidia RTX 3080 Ti at 4K.

{% include gallery id="gallery3" layout="half" caption="Cities also get a boost of character; some mods do complete overhauls. I decided to stick with a vanilla feel." %}

## Initial Setup

1. Download and install [Mod Organizer 2 (MO2)](https://github.com/ModOrganizer2/modorganizer/releases). On first startup, create a new global instance and browse to the Skyrim SE executable folder (by default on Steam, `C:\Program Files (x86)\Steam\steamapps\common\Skyrim Special Edition`). Select the Steam game edition. Click `Next` through the following dialogs and link your Nexus account, if desired. Select "yes" to the dialogs which ask to import Nexus mod categories and associating `.nxm` links.

2. Download the Anniversary Edition build of [Skyrim Script Extender (SKSE)](https://skse.silverlock.org/). (Unless the version of Skyrim SE that you are using is specifically downgraded to 1.5.97, use the AE build. You can confirm the game version by hovering the mouse cursor over the `Skyrim.exe` game executable.) Extract the contents containing `skse64_loader.exe` and associated folders/files into the Skyrim SE executable folder as mentioned in step 1.

3. Add a new executable in MO2 to launch SKSE. On the right hand side, click the dropdown with `Skyrim Special Edition`, then `<Edit...>`. On the left hand side of the new dialog, click the plus icon, then `Add from file...`. Select `skse64_loader.exe`. Change the title as you wish. Under `Start in`, select the Skyrim SE executable folder, then click OK to apply these settings. Try launching SKSE to confirm the install works.

Skyrim is now set up to be modded. For the following steps, I like to install the mods in the following order. At various points you may want to try launching the game to make sure that nothing is broken.

## User Interface Mods

* [SkyUI](https://www.nexusmods.com/skyrimspecialedition/mods/12604)
    * [Difficulty Persistence Fix](https://www.nexusmods.com/skyrimspecialedition/mods/106418)
    * [SkyUI 5.2 SE Plugin with Master](https://www.nexusmods.com/skyrimspecialedition/mods/67166)
    * [Fix Note Icon](https://www.nexusmods.com/skyrimspecialedition/mods/32561)
    * [Ghost Item Bug Fix](https://www.nexusmods.com/skyrimspecialedition/mods/49106)
* [Immersive HUD](https://www.nexusmods.com/skyrimspecialedition/mods/12440): I install this to protect my OLED from burn-in and add immersion.

## Patches

* [Address Library](https://www.nexusmods.com/skyrimspecialedition/mods/32444): required for other mods listed below. Use the all in one 1.6.X version.
* [SSE Engine Fixes](https://www.nexusmods.com/skyrimspecialedition/mods/17230): install part 1 through MO2; install part 2 by copying the contents into the Skyrim SE executable folder.
    * [SSE Bug FIxes](https://www.nexusmods.com/skyrimspecialedition/mods/33261)
* [Unofficial Skyrim Special Edition Patch](https://www.nexusmods.com/skyrimspecialedition/mods/266): patches lots of bugs still not patched in the many, many updates to the game.

## ENB Setup

* [Particle Patch](https://www.nexusmods.com/skyrimspecialedition/mods/65720): stick with default settings.
* [Less Distracting Blowing Snow](https://www.nexusmods.com/skyrimspecialedition/mods/36198)
* [Improved Sky Mesh](https://www.nexusmods.com/skyrimspecialedition/mods/58263)
* [ENB Light Inventory Fix](https://www.nexusmods.com/skyrimspecialedition/mods/66411)
* [ENB Helper SE](https://www.nexusmods.com/skyrimspecialedition/mods/23174)
* [Community Shaders](https://www.nexusmods.com/skyrimspecialedition/mods/86492)
* [Auto Parallax](https://www.nexusmods.com/skyrimspecialedition/mods/79473)
* [ENB Terrain Blending Fix](https://www.nexusmods.com/skyrimspecialedition/mods/140041)
* [ENBSeries](http://enbdev.com/download_mod_tesskyrimse.html). In the archive's `WrapperVersion` folder, copy `d3d11.dll`, `d3dcompiler_46e.dll`, and `enblocal.ini` into the Skyrim SE executable folder.

## Lighting

* [Static Mesh Improvement Mod](https://www.nexusmods.com/skyrimspecialedition/mods/659)
* [Obsidian Weathers and Seasons](https://www.nexusmods.com/skyrimspecialedition/mods/12125)
* [Moon and Stars](https://www.nexusmods.com/skyrimspecialedition/mods/73336)
* [ENB Light](https://www.nexusmods.com/skyrimspecialedition/mods/22574): check `Full install`.
* [Storm Lightning](https://www.nexusmods.com/skyrimspecialedition/mods/29243): for the halos, check the `Halo Dim (Level 2)` settings.
* [Splashes of Storms](https://www.nexusmods.com/skyrimspecialedition/mods/72985)
* [Enhanced Volumetric Lighting and Shadows](https://www.nexusmods.com/skyrimspecialedition/mods/63725): also install the optional underside mod.
* [Enhanced Lights and FX](https://www.nexusmods.com/skyrimspecialedition/mods/2424): check `SMIM Meshes`.
    * [Enhanced Lights and FX Shadows](https://www.nexusmods.com/skyrimspecialedition/mods/63790): check `Parallax meshes`. The Rudy ENB preset technically "requires" the additional `ELFX Enhancer` plugin checked, but I found that this setting leads to crushed blacks and caves that are too dark on my color-calibrated monitor.
    * [ELFX Enhancer - Brighter Living Spaces](https://www.nexusmods.com/skyrimspecialedition/mods/46909): optional, if the `ELFX Enhancer` plugin was installed.
    * [ELFX Consistency Fix](https://www.nexusmods.com/skyrimspecialedition/mods/68962)
    * [ELFX Dwemer Floor Footsteps Fix](https://www.nexusmods.com/skyrimspecialedition/mods/27084)
* [Embers XD](https://www.nexusmods.com/skyrimspecialedition/mods/37085): per Rudy ENB preset, select `Embers XD Flames (Burnt Orange)` flame style and `Optimized` particle light quality. Check `Reduced Fake Glow`, uncheck `Solitude Braziers`. Check `ELFX`, `ENB Light - Magic Hand FX`, and `Survival Mode`.
* [Water for ENB](https://www.nexusmods.com/skyrimspecialedition/mods/37061): can keep the default checked options (`Shades of Skyrim`).
* [Rudy ENB for Obsidian Weathers, Lux, ELFX](https://www.nexusmods.com/skyrimspecialedition/mods/4796): in the FOMOD installer for MO2, check `Rudy ENB Vanilla Clouds 4K`, `Rudy ENB Water for ENB FIX`, `Rudy ENB Night Sky Textures`, and `Rudy ENB ELFX FIX`. Make sure to download the other archive and copy the contents into the Skyrim SE executable folder. 
    * [Rudy Fix for Splashes of Storms and ENB](https://www.nexusmods.com/skyrimspecialedition/mods/72985)

## Textures

* [Skyland AIO](https://www.nexusmods.com/skyrimspecialedition/mods/34179): check all cities. Under night skies, check `None`. 
* [Skyland Bits and Bobs](https://www.nexusmods.com/skyrimspecialedition/mods/95032)
* [Skyland Landscapes Complex Parallax](https://www.nexusmods.com/skyrimspecialedition/mods/86821)
* [Skyland LODs](https://www.nexusmods.com/skyrimspecialedition/mods/87412)
* [Skyrim Flora Overhaul SE](https://www.nexusmods.com/skyrimspecialedition/mods/2154): install the no grass version. The only gripe I have about this mod is the random redwood trees that seem to be on the tops of mountains which makes no sense. There doesn't seem to be an option to disable this.
* [Northern Grass SE](https://www.nexusmods.com/skyrimspecialedition/mods/25459) 
    * [Northern Grass Aliased Grass Fix](https://www.nexusmods.com/skyrimspecialedition/mods/38354)

## Quality of Life

* [A Quality World Map](https://www.nexusmods.com/skyrimspecialedition/mods/5804)
