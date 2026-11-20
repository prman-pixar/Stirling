# Stirling
Stirling is an official Pixar asset derived from the Finn McMissile character in Pixar's *Cars 2* and designed to showcase MaterialX. The asset was authored using MaterialX 1.39 base building blocks and does not use monolithic materials such as Standard Surface and OpenPBR.  

<div align="center">
	<img src="renders/RenderMan/WideCam.png" width="100%" />
	<img src="renders/RenderMan/HeadLightCam.png" width="19%" />
	<img src="renders/RenderMan/TailLightCam.png" width="19%" />
	<img src="renders/RenderMan/InteriorCam.png" width="19%" />
	<img src="renders/RenderMan/TireCam_Front.png" width="19%" />
	<img src="renders/RenderMan/TireCam_Back.png" width="19%" />
</div>

<br><br>
Stirling comes in two openUSD variants, which share the same materials and represent the same colors:

Real Time: 
- Polys - High res mesh
- PNG raster textures
- Linear Rec.709 color space, with sRGB-encoded color textures
- 2K max resolution, with 4K seats and tire inscriptions
- Games | Web | Mobile

Offline:
- SubD - Low res mesh
- TEX Mipmap textures (tiled OpenEXR)
- Linear Rec.709 color space
- 4K max resolution
- Production rendering

RenderMan:
- SubD - Low res mesh
- TEX Mipmap textures (RenderMan format)
- Linear Rec.709 color space
- 4K max resolution
- Production rendering

<br><br>
Stirling running in Houdini 22 and RenderMan 28:

<img width="2555" height="1536" alt="Houdini-22-RenderMan-XPU" src="renders/RenderMan/Solaris_RenderMan_XPU.png" />

## Color Management
Stirling is authored in the Linear Rec.709 color space, and each color in its materials and light textures is tagged directly with its color space:

- Color constants are tagged `lin_rec709`, apart from eight saturated colors that keep their original ACEScg values and are tagged `acescg`.
- Color textures are tagged `lin_rec709` in the Offline variant, and `srgb_texture` in the Real Time variant.
- Light textures are tagged `lin_rec709` in both variants.
- Non-color data, such as normal, roughness, and displacement textures, is left untagged, apart from single-channel data read through a `color3` node, which is tagged `none` in the Offline variant and `srgb_texture` in the Real Time variant, where it is stored sRGB-encoded for 8-bit precision.

The materials declare no document-level color space, and the USD layers do not apply `ColorSpaceAPI`, for the following reasons:

- USD does not preserve the document-level color space of a referenced MaterialX file.
- A color space applied through `ColorSpaceAPI` is inherited by every attribute beneath it, including those that carry non-color data.
- Direct tags state the color space only where it applies, and survive referencing.

## Rendering
The dome light is oriented according to the UsdLux specification, which follows the OpenEXR convention for latitude-longitude maps, with the top of the map at +Y and its center facing +Z.  It is rotated -90 degrees about the Y axis, which most closely reproduces the lighting of the reference renders in renderers that follow this convention.

To match the tone mapping of the reference renders, view renders of the asset through the `ACES 1.0 - SDR Video` view of the `sRGB - Display`, as defined in the OCIO configuration bundled with Houdini 22.

## Trivia
Stirling also has some fun RenderMan historical trivia:

* A113 \- Tag (Number of the classroom at CalArts where many legendary Disney, Pixar, and indie animators studied)  
* Utah Teapot \- Rearview mirror prop (famous open source testing model from University of Utah)  
* Cornell Box \- Rearview mirror prop (inspired by the famous test scene and redone with primitive geometry)  
* MaterialX logo  
* OpenUSD Logo  
* 001988 \- Dashboard Mileage \- RenderMan RiSpec publishing year  
* RenderMan Tires \- Assorted RenderMan trivia on the tire sidewall  

## About the Authors
Stirling is an initiative by Pixar's Dylan Sisson and modeled by Andrew Schmidt. MaterialX Stirling was created by Pixar's Leif Pedersen and in collaboration with Pixar's Fran Gonzalez and Jonathan Stone from Lucasfilm.  
A special thanks to the technical direction and contributions of Arnon Marcus from OTOY.
