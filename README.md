# imageMagickTutor
This is my journey with imageMagick. Let's see how far we can go.

```
convert -size 40x20 xc:red  ./figure/red.gif
```
It creates the following figure

![Red](./figure/red.gif)

Let's see we want a bigger red

```
convert -size 400x200 xc:red  ./figure/red_big.gif
```
![Big Red](./figure/red_big.gif)

Now let's say I want to rotate it.

```
convert ./figure/red_big.gif  -rotate 90 ./figure/red_rot.gif
```
![Rotate Red](./figure/red_rot.gif)

