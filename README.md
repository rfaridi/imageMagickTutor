# imageMagickTutor
This is my journey with imageMagick. Let's see how far we can go.

```
convert -size 40x20 xc:red  ./figure/red.gif
```
It creates the following figure

![Red](./figure/red.gif)

Let's see we want a bigger red

Now let's say I want to rotate it.

```
convert ./figure/red_big.gif  -rotate 90 ./figure/red_rot.gif
```
![Rotate Red](./figure/red_rot.gif)

Let's create a blue one

```
convert -size 40x20 xc:blue  ./figure/blue.gif
```
![Blue](./figure/blue.gif)

Now append red and blue

```
convert  ./figure/red.gif ./figure/blue.gif -append ./figure/app_rb.gif
```
![Append](./figure/app_rb.gif)

After appending then rotate


```
convert   ./figure/app_rb.gif -rotate ./figure/app_rb_rot.gif
```
![app_rot](./figure/app_rb_rot.gif)

Now let's say we want to first rotate then append


```
convert  ./figure/red.gif ./figure/blue.gif -rotate 90 -append rot_rp_app.gif
```
![rot app](./figure/rot_rp_app.gif)

Notice that in the above `-rotate 90` command was applied to both the figure. we did not need to put individual rotate for each of the figure.

** IMv6 command syntax

Following is the syntax

```
command {[-setting]... "image"|-operation }... "output_image"
```
With the part in {...} repeated as many times as you want. 

All command line options will be divided in two types

settings

image operators

Settings

command line options that only save information. It will used later by 'image operators'. they do not do anything except set some value to be used later. 

Many of the options have both '-' and '+' where '+' is used to turn off setting. 

Operator settings control how later operators function

Input settings  control the creation of images that are created or read in. 

Output settings are only used when writing or saving of the image

Image creation operators  are commands that will modify the image in some way

Image processing operators will modify all images that have already been read into memory

Let's see an example

```
convert snow.gif eyes.gif -append tree.jpg -background skyblue +append result.gif
```
![result](./figure/result.gif)

Resize tree.jpg before doing the above

```
convert snow.gif eyes.gif -append tree.jpg -resize 200x100 -background skyblue +append result.gif
```
![result](./figure/result.gif)

Now to show information about the image, we can do the following:

```
identify tree.gif
```

We can also use the convert command to do so - 
```
convert t*  -format 'image %f\n is of size  %G\n'   info:
```
Not exactly sure about what it does but it works

A simper method to do similar thing

```
convert t* -format 'image %f is of size  %G\n' -identify null
```

yes, the above also works


Another interesting use is the print command, you can do even some calculation

```
convert null: -print '%[fx: (50+25)/5 ]\n' null:

```
see the use of `null:` at the beginning and at the end. it means we are using the `convert` command but no input or output image is put. instead we are prining a calculation. any `%` operator seems to be in the `''` and not sure why the third bracket is required.


We can use the following series of commands to find information about a image

```
convert tree.jpg -format '%i\n'  -identify null:
convert tree.jpg -format '%f\n'  -identify null:
convert tree.jpg -format '%w\n'  info:
convert tree.jpg -format '%h\n'  info:

```

in the above `%i` gives the full directory path, `%f` gives the file name

`%w`  = width of the image
`%h` = height of the image

To convert a list of files into a directory of thumbnail, we can do the following:

```
convert *.jpg   -thumbnail 120x90 \
          -set filename:fname '%t_tn' +adjoin '%[filename:fname].gif'
```

`*.jpg` means any file with extension `jpg`
`-thumbnail` - special thumbnail command
`filename-fname` - setting filename patter
`%t_tn` - i think `%t` means the first part of a file name without the extension, on the other hand `%e` is the extension part i guess.

`+adjoin` - not sure what it does

`%[filename:fname]` puts the pattern in place

** Batch processing alternatives

```
mkdir thumbnails
  for f in *.jpg
  do   convert $f -thumbnail 200x90 thumbnails/$f.gif
  done
```
