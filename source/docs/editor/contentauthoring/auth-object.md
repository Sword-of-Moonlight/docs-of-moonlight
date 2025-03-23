# Authoring Custom Objects
Objects are going to be the meat of your project... If you're looking to create a very custom experience, you'll quickly run in to a need to create some. Barrels? Objects. Chests? Objects. Big weird pillars with delicate patterns? Ye, objects.

SoM supports both static and animated objects, and we'll cover the creation of both.

## Prerequisites
1. [The 3D content tool kit](https://serve.swordofmoonlight.com/Tools/MM3D_SomEx_V100.7z)
2. [HwitVlf's PRF Tool](https://www.swordofmoonlight.com/404youbastard/)

## Static Model
Your first step will be up to you - that's creating an object. You're going to need to use Michael's MM3D fork to do that, since it supports the SoM feature set - or alternatively you can model in Blender or something, export as a .OBJ and then import it into MM3D for the final touches. 

!!! tip
    Make sure that you consider unit scale - which in SoMs case is 1m = 1 unit. If you're confused, there's a 2m/unit scale plane avaliable [here](https://serve.swordofmoonlight.com/Tools/2mTemplate.obj) that you can use as a template.

    You _can_ use other model formats in mm3d_mdl/mm3d_mdo, but the chance of them working is extremely slim.

After your model is complete, you will be able to convert it. Look inside the 3D content tool kit, you'll have access to a few bat files. 'mm3d_mdl', 'mm3d_mdo', 'mdl_mm3d', 'mdo_mm3d' - what we want is the 'mm3d_mdo' tool - which will convert a MM3D format model into an MDO one for SoM. Simply drag your model file onto the bat file, and it will automatically convert and spit out an MDO (and if you used a texture, TXR file) next to the source MM3D file.

## Animated Model
TO-DO

## Profile
