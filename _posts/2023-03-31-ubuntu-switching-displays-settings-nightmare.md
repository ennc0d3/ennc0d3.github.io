# My ubuntu displays settings nightmare

When it comes to using Ubuntu, one of the most important aspects is getting the
display settings just right. This includes choosing the right resolution,
refresh rate, and scale factor. In this guide, we'll walk you through the
basics of display settings and customization in Ubuntu.

## Understanding Display Settings
Before we dive into how to customize your display settings in Ubuntu,
it's important to understand some basic concepts.

### Resolution
The resolution of a display refers to the number of pixels that make
up the screen. The higher the resolution, the sharper and clearer the image.
However, increasing the resolution can also make everything on the screen
smaller.
To adjust the resolution on Ubuntu, you can go to **Settings > Display**
and choose from the available options.

### Refresh Rate
The refresh rate refers to how often the screen is redrawn. A higher refresh
rate can make the display look smoother and reduce eye strain. To adjust the
refresh rate on Ubuntu, you can use the xrandr command in the terminal.

### Scale Factor
The scale factor allows you to adjust the size of everything on the screen,
including icons, text, and UI elements. This can be especially useful if you
have a high-resolution display and everything appears too small.
To adjust the scale factor on Ubuntu, you can use the **gsettings** command in
the terminal or **Fractional Scaling** setting.

## Customizing Display Settings in Ubuntu
Now that you understand the basic concepts of display settings, let's look at
how to customize them in Ubuntu.

### Adjusting Resolution
To adjust the resolution on Ubuntu, go to **Settings > Display**.
You'll see a list of available resolutions for your display. Choose the one
that works best for you. If you don't see the resolution you want, you can create
a custom resolution using the **xrandr** command.

### Adjusting Refresh Rate
To adjust the refresh rate on Ubuntu, use the **xrandr** command in the terminal.
First, run the following command to see the available modes:

```
xrandr -q
```
This will display a list of available modes for your display.
Look for the one you want to use and note its name. Then, use the
following command to set the refresh rate:

```
xrandr --output <display> --mode <mode> --rate <refresh rate>
```
Replace <display> with the name of your display, <mode> with the mode you want
to use, and <refresh rate> with the refresh rate you want to set.

### Adjusting Scale Factor
To adjust the scale factor on Ubuntu, use the gsettings command in the
terminal. First, run the following command to see the available scale factors:

```
gsettings list-recursively org.gnome.desktop.interface | grep scale-factor
```
This will display a list of available scale factors. Choose the one you want
to use and note its value. Then, use the following command to set the scale factor:

```
gsettings set org.gnome.desktop.interface scaling-factor
```
<value> Replace <value> with the value of the scale factor you want to use.
Note that the scale factor value should be an integer, such as 1, 2, or 3.

### Creating Custom Resolutions
If you don't see the resolution you want in the available options, you can
create a custom resolution using the **cvt** and **xrandr** commands.

First, use the **cvt** command to generate a new modeline:

```
cvt <width> <height> <refresh rate>
```
Replace <width> and <height> with the desired resolution and <refresh rate>
with the desired refresh rate.

### Fractional Scaling:

Another option that can help with scaling on high-resolution displays is
fractional scaling. It's a relatively new feature that was added to Ubuntu in
recent versions, and it allows you to scale your screen at a finer granularity,
such as 125% or 150%.

To use fractional scaling, you need to have the right graphics driver and a
compatible version of Ubuntu. If your system meets these requirements, you can
enable fractional scaling from the Settings app under the Display section.

When you enable fractional scaling, you'll have the option to set a custom
scaling factor that can be any value between 1.0 and 2.0, with increments of
0.1. This gives you more fine-grained control over the scaling, and it can help
you achieve a better balance between the size of the elements on your screen
and their clarity.

## Summary

In this article, we covered some of the basic concepts of screen resolution,
pixels, and refresh rates. We also discussed how to set your screen resolution
and refresh rate, and how to customize your display settings using xrandr and
gsettings.

Finally, we talked about the fractional scaling option, which can be a useful
tool for users who need to adjust their screen scaling on high-resolution
displays. With this feature, you can achieve a better balance between the size
of the elements on your screen and their clarity, and it can help you avoid the
need for external tools and complex configurations.

Overall, Ubuntu provides a variety of tools and settings that can help you get
the most out of your display. By understanding the basics of screen resolution
and using the right tools and settings, you can customize your display to fit
your needs and preferences.
