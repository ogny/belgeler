* bu scripte gerek yok, bagliysa i3'u restart edince duzenlemeyi yapiyor
#!/bin/bash

multiple=`xrandr |grep "VGA-0 connected"`
if [ -z $multiple ]
then

```
xrandr --output VGA-0 --auto --right-of LVDS-0 --output LVDS-0 --auto
xrandr --newmode "1440x900_60.00"  106.50  1440 1528 1672 1904  900 903 909 934 -hsync +vsync
xrandr --addmode VGA-1 1440x900_60.00
xrandr --output VGA-1 --mode 1440x900_60.00 --right-of LVDS-1 --output LVDS-1 --auto
xrandr --output VGA-1 --auto --left-of LVDS-1 --output LVDS-1 --auto
randr --output HDMI-1 --auto --left-of LVDS-1 --output LVDS-1 --auto
xrandr --output HDMI-1 --on
xrandr --output LVDS-1 --off
xrandr --output eDP1  --mode 1366x768 --auto --left-of HDMI2 --auto
```
