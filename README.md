# SBFI (Smooth Beefy Front Idlers)

![SBFI family render](Images/SBFI_2.4.png)
![SBFI family render](Images/SBFI_Trident.png)

### What is this?

These are replacement front idlers for Vorons (and Voron-based machines).
This design is available for Voron Trident and Voron 2.4.

### Improvements?

This is a version remixed/based on [BFI](https://github.com/clee/VoronBFI/tree/main)
my version has several improvements or things that I have changed to fit my needs.
#### Changes:
- They are now symetrical
- They now have a magnetic cover so you don't need to see those screws anymore
- They now have curved edges instead of chamfers (very similar to [Ramalama2](https://github.com/Ramalama2/Voron-2-Mods/tree/main/Front_Idlers))

### Some more things you need to know:
You can also find this model on:
- [Clee's usermods github](https://github.com/clee/VoronBFI/tree/main/usermods#other-bfi-forksvariants)
- [Printables](https://www.printables.com/model/890840-sbfi-smooth-beefy-front-idlers)

All the clips for the z belts on the voron 2.4 are the exact same as the BFI

The internal bearing stack holder is also the exact same as the BFI (so if you had the BFI and you want to change to SBFI you don't need to reprint that part)

The front magnetic covers can be printed with MMU/AMS or you can print them as separate gcodes without removing the build plate after each print, or you can just print them with the negative of the voron logo (single color)

## BOM

### SBFI for Voron 2.4
(It's the exact same BOM as BFI but you also need 8 6x3mm magnets (4 per idler) "it will also work fine with 4 magnets (2 per idler) but I still recomend you use 8 magnets"

For V2.4, you'll need:
- 4× M5×30 BHCS 
- 4× M5 hex nuts
- 2× 5mm-diameter pin, 18mm long
- 2× M3 t-nuts
- 2× M3×16 BHCS

...plus the same shim/bearing stack setup for each idler as the original stock front idlers.

### SBFI for Trident
(It's the exact same BOM as BFI but you also need 8 6x3mm magnets (4 per idler) "it will also work fine with 4 magnets (2 per idler) but I still recomend you use 8 magnets" (and the exception that you need to change the M5x40 SHCS for M5x40 BHCS)

For Trident, you'll need:
- 4× M5×40 BHCS
- 4× M5 hex nuts
- 2× 5mm-diameter pin, 18mm long

...plus the same shim/bearing stack setup for each idler as the original stock front idlers.

## Belt / Idler Skew Tuning

BFI has a tendency to wear down belts, a known issue that occurs when the idlers are misaligned. This is also discussed in an upstream issue: https://github.com/clee/VoronBFI/issues/29.

You can use the macro below to move the tool head in a circular pattern within its range of motion. This ensures the belts keep moving consistently while you adjust the idlers.

```
[gcode_macro _TEST_BELTS]
gcode:
  {% if 'x' not in printer.toolhead.homed_axes %}
  {% if 'y' not in printer.toolhead.homed_axes %}
  G28 X Y 
  {% endif %}
  {% endif %}
 
  {% set circles = params.S|default(1)|int %}
  {% set rate = params.F|default(18000)|int %}
  {% for i in range(circles) %}
      G1 X{printer.toolhead.axis_minimum.x + 15} Y{printer.toolhead.axis_minimum.y + 15}  F{rate}
      G1 X{printer.toolhead.axis_maximum.x - 15} Y{printer.toolhead.axis_minimum.y + 15}  F{rate}
      G1 X{printer.toolhead.axis_maximum.x - 15} Y{printer.toolhead.axis_maximum.y - 15} F{rate}
      G1 X{printer.toolhead.axis_minimum.x + 15} Y{printer.toolhead.axis_maximum.y - 15}  F{rate}
      G1 X{printer.toolhead.axis_minimum.x + 15} Y{printer.toolhead.axis_minimum.y + 15}  F{rate}
    {% endfor %}
    G1 X{ printer.toolhead.axis_maximum.x / 2  } Y{printer.toolhead.axis_maximum.y  /2}  F{rate}
```

Call this macro with the parameter S to set the number of circles. For example, use `_TEST_BELTS S=20` to run 20 circles.

As the belts move, you can better observe if they are riding up on the flanges. To adjust, loosen the side you want the belt to shift toward—if it's riding up on the top flange, loosen the top bolt; if it's on the bottom flange, loosen the bottom bolt.
<br/>
<br/>
[!(https://github.com/CloakedWayne/Monolith_Gantry_V2-VT/blob/main/Images/kofi_short_button_white.png)](https://ko-fi.com/radiotbo)
<br/>
# Contributors
I want to thank everyone who has helped to develop this project.

- [@clee](https://github.com/clee) (Original BFI)
- [@Ramalama2](https://github.com/Ramalama2) (Curved edges design)
- [@Shaly](https://github.com/Apstarkdev) (Improvement suggestions and testing)
- [@TheVoronModder](https://www.youtube.com/@TheVoronModder) (Improvement suggestions) (check out his [Chunky Alpaca Idlers](https://www.printables.com/model/678823-voron-24-trident-chunky-alpaca-tensioners))
- @Micky (Curved edges suggestion)
- [@Vajonam](https://github.com/vajonam) (Feedback and Macro Development)

And If someone has any suggestion for improving the design or maybe some cool features I could add, please don't hesitate to tell me.

You can either contact me through Github or through Discord "radiotbo" (I'd probably answer you faster through discord)
