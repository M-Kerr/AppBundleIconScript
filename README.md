# AppBundleIconScript
Script to convert .png files into .icns for Apple app bundles

Credit:
This script was written by [Alexandre Strube](https://stackoverflow.com/users/907465/alexandre-strube?tab=profile)
in a [stack overflow response](https://stackoverflow.com/a/31883126/9137234)

"I made a small script that takes a big image and resizes it to all expected icon sizes for Mac OS, including the double ones for retina displays. It takes the original png file, which I expect to be as big as the maximum size, if not bigger, to make sure they are rendered at maximum quality.

It resizes and copies them to a icon set, and uses the Mac OS's 'iconutil' tool to join them into a .icns file.

For this script to run, you need your original icon file to be a png, and you have your bundle in more or less working order. You only need to touch the first three lines."

```
export PROJECT=Myproject
export ICONDIR=$PROJECT.app/Contents/Resources/$PROJECT.iconset
export ORIGICON=Mybigfile.png

mkdir $ICONDIR

# Normal screen icons
for SIZE in 16 32 64 128 256 512; do
sips -z $SIZE $SIZE $ORIGICON --out $ICONDIR/icon_${SIZE}x${SIZE}.png ;
done

# Retina display icons
for SIZE in 32 64 256 512; do
sips -z $SIZE $SIZE $ORIGICON --out $ICONDIR/icon_$(expr $SIZE / 2)x$(expr $SIZE / 2)x2.png ;
done

# Make a multi-resolution Icon
iconutil -c icns -o $PROJECT.app/Contents/Resources/$PROJECT.icns $ICONDIR
rm -rf $ICONDIR #it is useless now
```
