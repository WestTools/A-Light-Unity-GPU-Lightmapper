<p align="center">
  <img src="docs/banner.png" alt="A-Light — GPU lightmapper for Unity" width="100%">
</p>

<p align="center">
  <img alt="Unity 2022.3+" src="https://img.shields.io/badge/Unity-2022.3%2B-222?logo=unity">
  <img alt="GPU: AMD, NVIDIA, Intel" src="https://img.shields.io/badge/GPU-AMD%20%7C%20NVIDIA%20%7C%20Intel-ff8a3d">
  <img alt="Status: beta" src="https://img.shields.io/badge/status-beta-8a6cff">
</p>

DOES NOT USE ANY BAKERY SCRIPTS, AND DOES NOT CONTAIN A CRACKED VERSION OF BAKERY.

# A-Light

a GPU lightmapper for Unity that actually works on AMD 😉

i have an AMD gpu and couldnt find a good lightmapper that would bake on it, so i made my own. it runs on compute shaders instead of CUDA so it works on AMD, NVIDIA and Intel. if youre coming from Bakery theres a transfer button that turns all your Bakery lights into A-Light lights so you dont have to redo your whole map

its still in beta so expect some weird stuff. if something breaks pls open an issue

## what it does

- bakes lightmaps on basically any gpu (DX11 works fine)
- hardware ray tracing if youre on Unity 6 / 2023.1+ with DX12 (AMD RX 6000+, RTX, Intel Arc)
- **Bakery Transfer**: converts Bakery point, direct, sky and light mesh lights, and copies your Bakery bake settings too
- direct light, bounce light (GI), sky light, emissive stuff, area lights, AO, shadowmask, light probes, all the normal stuff
- soft shadows + a denoiser so it doesnt look crusty
- reads material colours through each shader's Meta pass, so custom shaders bake with the right colours
- light filters (stained glass style coloured light) and skinned mesh shadows
- transparency that actually lets light through glass, either cutout or partial

## how to install

1. grab `A-Light.unitypackage` from [releases](../../releases)
2. in unity go **Assets › Import Package › Custom Package** and import it
3. open **A-Light › Render Lightmaps** in the top menu bar

you need **Unity 2022.3 or newer**. thats what i use

## switching from Bakery

1. **keep Bakery installed** for this part, the transfer tool needs to read its components (it still compiles on AMD, it just cant bake)
2. go to **A-Light › Bakery Transfer** and hit **Transfer**
3. open the lightmapper and hit **Render**

it doesnt touch your Bakery lightmaps, A-Light saves to `Assets/ALightLightmaps/` so you can always go back

| Bakery | A-Light |
|---|---|
| Bakery Point Light | A-Light Point Light |
| Bakery Direct Light | A-Light Directional Light |
| Bakery Sky Light | A-Light Sky Light |
| Bakery Light Mesh | A-Light Area Light |
| Bakery Light Filter | A-Light Light Filter |

## starting from scratch

1. make your map **Contribute GI** static and set Receive GI to **Lightmaps**
2. add lights from **GameObject › Light › A-Light**
3. hit **Render**

## stuff that doesnt work yet

- RNM / SH / MonoSH directional modes and light volumes. these need special shaders that normal Unity shaders cant read, so use Dominant Direction or Baked Normal Maps instead
- skinned meshes cast shadows in their current pose but can't be lightmapped themselves. thats a Unity limit, they use light probes instead

## contributing

PRs are welcome, especially if you can test on other gpus or URP/HDRP. bug reports with screenshots help the most

## disclaimer

im not affiliated with Bakery or its dev in any way. Bakery is only mentioned so you know what this is compatible with. all trademarks belong to their owners

this will ALWAYS be a free unity package. if anyone sells it to you, you got scammed. feel free to promote and share this public package.
