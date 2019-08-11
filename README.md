# Description
A 9-patch consists of a resizable texture with the following structure:
![square](https://user-images.githubusercontent.com/3154879/62836726-1cf0c680-bc5e-11e9-8f31-6997b74109c1.png)
- Regions 1, 3, 5, 7 and 9 are scaled to fit the end image (scaled from patch coordinates to end image coordinates)
- Regions 2, 4, 6 and 8 can either be scaled to fit or repeated - STRETCH mode and TILE mode respectively

# Modes

9-patch texture (input) | STRETCH | TILE
--------------- | ------- | ----
![square](https://user-images.githubusercontent.com/3154879/62836756-6d682400-bc5e-11e9-9527-b9a0fc4fa3f5.png) | ![stretch](https://user-images.githubusercontent.com/3154879/62836767-a1434980-bc5e-11e9-914c-93506eb2ff8d.png) | ![tile](https://user-images.githubusercontent.com/3154879/62836763-938dc400-bc5e-11e9-808b-df3505e6df6d.png)

# Usage
1. Update L#0 with the desired 9-patch texture.
2. Update L#3 with render mode (TILE / STRETCH)
3. Update L#7 with 9-patch texture size (preferably power of 2)
4. Update padding and chunk with your 9-patch cut coordinates
5. Render using https://github.com/stevensona/shader-toy

# Motivation
iOS provides a resizable image class which supports both TILE and STRETCH mode, however Android only supports STRETCH mode. This fragment was an attempt to provide a third party replacement for 9-patch rendering in android.

This approach has serious limitations, as it will only work on devices with OpenGL support (some only support Vulkan or none at all). Given this I will not be developing/maintaining this fragment any further. It was a great weekend project to grasp what shader programming is. 

# Challenges
1. In order to use this shader in Android, one would have to implement a SurfaceView or TextView and manage an OpenGL context to render inside them. Each has its own (dis)advantages. This can be tricky and I'm not sure if it would have good performance in situations where multiple 9-patch views in the same layout are needed.

2. The current approach relies on cut coordinates which are represented as variables `padding` and `chunk`. Android Studio offers a tool to generate a 9-patch chunk which contain all cut information. A true third party replacement would be able to interpret that data directly.

3. Also, on a different approach, Android offers BitmapShaders which might ease out this OpenGL context management a bit, as it would not require any shader program at all. From this shader implementation one could extrapolate a few matrixes to apply in a BitmapShader. Maybe another weekend I'll give this a shot :) 

4. And a bunch more that I am not able to forecast!

# Conclusion
It was nice getting to know a bit more about shader programs and GLSL in particular. Also, getting down to this level helped me understand how difficult it is to support such wide range of brands/makes/versions/etc that are available out there.

If you happen to have thought about this or have faced this android limitation, please ping me so we can discuss how we eventually did overcome this problem (or not) :) 
