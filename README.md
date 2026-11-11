<p align="center">
  <img src="docs/banner.png" alt="A-Light — GPU lightmapper for Unity" width="100%">
</p>

<p align="center">
  <img alt="Unity 2022.3+" src="https://img.shields.io/badge/Unity-2022.3%2B-222?logo=unity">
  <img alt="GPU: AMD, NVIDIA, Intel" src="https://img.shields.io/badge/GPU-AMD%20%7C%20NVIDIA%20%7C%20Intel-ff8a3d">
  <img alt="Status: beta" src="https://img.shields.io/badge/status-beta-8a6cff">
</p>

<p align="center">
  <img src="docs/languages.svg" alt="Languages: C# 83.4%, HLSL 10.0%, ShaderLab 6.6%" width="100%">
</p>

DOES NOT USE ANY BAKERY SCRIPTS, AND DOES NOT CONTAIN A CRACKED VERSION OF BAKERY.

# A-Light

a GPU lightmapper for Unity that actually works on AMD 😉

i have an AMD gpu and couldnt find a good lightmapper that would bake on it, so i made my own. it runs on compute shaders instead of CUDA so it works on AMD, NVIDIA and Intel. if youre coming from Bakery theres a transfer button that turns all your Bakery lights into A-Light lights so you dont have to redo your whole map

its still in beta so expect some weird stuff. if something breaks pls open an issue

## what it does

- bakes lightmaps on basically any gpu (DX11 works fine)
- hardware ray tracing if youre on Unity 6 / 2023.1+ with DX12 (AMD RX 6000+, RTX, Intel Arc)
- **Bakery Transfer**: converts Bakery point, direct, sky and light mesh lights, light filters and volumes, and copies your Bakery bake settings too
- direct light, bounce light (GI), sky light, emissive stuff, area lights, AO, shadowmask, light probes, all the normal stuff
- soft shadows + a denoiser so it doesnt look crusty
- reads material colours through each shader's Meta pass, so custom shaders bake with the right colours
- light filters (stained glass style coloured light)
- transparency that actually lets light through glass, either cutout or partial
- each scene gets its own lightmap folder, so baking one scene never messes up another
- works on Quest/Android and mobile too, lightmaps import the same way Bakery's do so they look right with RGBM encoding
- handles meshes made of quads, not just triangles
- saves the scene for you after baking so your lightmaps dont get scrambled when you reopen unity (you can turn it off)
- **volumetric lights**: light beams, point glows, floating dust, flicker and pulse effects, and beams that get cut off by walls. all under **GameObject › Light › A-Light › Volumetric Lights**

## how to install

1. grab `A-Light.unitypackage` from [releases](../../releases)
2. in unity go **Assets › Import Package › Custom Package** and import it
3. open **A-Light › Render Lightmaps** in the top menu bar

you need **Unity 2022.3 or newer**. thats what i use

when updating, delete the `Assets/A-Light` folder first, then import the new version

### good starting settings

the defaults are what i use: **Texels per unit 10**, **Bounces 3**, **GI samples 80**, **Emissive boost 0**. if your shadows look too bright or washed out, check **Emissive boost** is on 0. want sharper shadows? raise **Texels per unit**, it just takes longer to bake

## switching from Bakery

1. **keep Bakery installed** for this part, the transfer tool needs to read its components (it still compiles on AMD, it just cant bake)
2. go to **A-Light › Bakery Transfer** and hit **Transfer**
3. open the lightmapper and hit **Render**

it doesnt touch your Bakery lightmaps, A-Light saves to `Assets/ALightLightmaps/` so you can always go back. theres also **Use Bakery** / **Use A-Light** buttons in the transfer window to swap between them whenever you want

| Bakery | A-Light |
|---|---|
| Bakery Point Light | A-Light Point Light |
| Bakery Direct Light | A-Light Directional Light |
| Bakery Sky Light | A-Light Sky Light |
| Bakery Light Mesh | A-Light Area Light |
| Bakery Light Filter | A-Light Light Filter |
| Bakery Volume | A-Light Light Volume |

## starting from scratch

1. make your map **Contribute GI** static and set Receive GI to **Lightmaps**
2. add lights from **GameObject › Light › A-Light**
3. hit **Render**

## SH, MonoSH, RNM, light volumes and skinned meshes

these need the **A-Light/Lit** shader on your materials, or `#include "ALight.cginc"` in your own shader

- set **Directional mode** to **MonoSH**, **SH** or **RNM** for sharper normal-map lighting
- add **GameObject › Light › A-Light › Light Volume** to bake a 3D grid of lighting for moving objects. objects using A-Light/Lit inside the box use it instead of light probes
- **skinned meshes get lightmaps** 🔥 unity normally cant do this at all. A-Light bakes them in the pose theyre in when you hit Render, so its best for stuff that doesnt move much

## volumetric lights

fake light beams and glows, like light shining through dust or fog. go to **GameObject › Light › A-Light › Volumetric Lights**

- **Volumetric Spot Light**: a spot light with a beam that matches its colour, angle and range
- **Volumetric Point Light**: a point light with a glowing ball of light around it
- **Volumetric Beam (3D)** / **(2D)**: just the beam. 2D is a cheaper flat one that always faces the camera
- **Point Glow**: just the glow
- **Add To Selected Lights**: already got lights? select them and hit this. spot lights get a beam, point lights get a glow
- **Dust**: little floating specks inside the beam
- **Occlusion**: cuts the beam off when it hits a wall (the wall needs a collider)
- **Flicker** / **Pulse**: makes the light flicker or slowly pulse in play mode. also works on a normal Unity Light on the same object

beams have soft edges, glare when you look into the light, fade where they touch walls and when the camera gets close, optional 3D fog noise, and colour gradients. they dont affect baking at all, theyre just for looks

works on the built-in render pipeline. soft edges where the beam hits walls need the camera depth texture, A-Light turns that on for you

## addons

addons are separate unitypackages that add extra stuff to A-Light. import one and it shows up in the **Addons** tab of the A-Light window. you need A-Light installed, without it they just do nothing

- **Lightmap Switching**: bake a few lighting setups (day and night, lights on and off) and switch between them in game. put each setup's lights in its list, hit **Bake All Sets**, then call `SetLighting(index)`, `SetLightingByName(name)`, `Next()` or `Previous()` on the switcher, for example from a button
- **Live Bake Preview**: bakes a rough quick version of your lighting while you move lights around, so you dont have to do a full bake every time. hit **Stop Preview** and your real bake comes back

## contributing

PRs are welcome, especially if you can test on other gpus or URP/HDRP. bug reports with screenshots help the most

## disclaimer

im not affiliated with Bakery or its dev in any way. Bakery is only mentioned so you know what this is compatible with. all trademarks belong to their owners

this will ALWAYS be a free unity package. if anyone sells it to you, you got scammed. feel free to promote and share this public package.
