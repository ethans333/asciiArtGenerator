# asciiArtGenerator


### Here's the code:
```python
#created by @ethans33 8/1/2020

from PIL import Image
import math
import numpy as np
import os

im = Image.open("/Users/ethan/Documents/Python/brownLab.jpeg")
im = im.convert("L")
pixels = im.load()
symbols = "-!@#$%?&*:~"

symbolsAndValues = {}

tempLastNum = 0
for i in range(1, len(symbols)):
    if i + 1 < len(symbols):
        symbolsAndValues[symbols[i]] = tempLastNum, math.floor(i * 255 / len(symbols))
        tempLastNum = math.floor(i * 255 / len(symbols))
    else:
        symbolsAndValues[symbols[i]] = (tempLastNum, 255)
    
symbolArray = np.empty((im.size[0],im.size[1]), str)
for x in range(im.size[0]):
    for y in range(im.size[1]):
        for symbol in symbolsAndValues:
            if pixels[x,y] >= symbolsAndValues[symbol][0] and pixels[x,y] <= symbolsAndValues[symbol][1]:
                symbolArray[x][y] = symbol

with open(os.path.expanduser("~/Desktop/ascii_art.txt"), "w") as file:
    file.truncate(0)
    for y in range(len(symbolArray)):
        for x in symbolArray[y]:
            if x != len(symbolArray[y]):
                file.write(x)
            else:
                file.write(x + "\n")
```
In theory it works, but whenever you see the ascii ```symbolArray``` extracted to a text file it doesn't look quite like the image due to the textfile's way of formatting text.

### The Breakdown
```python
im = Image.open("/Users/ethan/Documents/Python/brownLab.jpeg")
im = im.convert("L")
pixels = im.load()
symbols = "-!@#$%?&*:~"

symbolsAndValues = {}

tempLastNum = 0
for i in range(1, len(symbols)):
    if i + 1 < len(symbols):
        symbolsAndValues[symbols[i]] = tempLastNum, math.floor(i * 255 / len(symbols))
        tempLastNum = math.floor(i * 255 / len(symbols))
    else:
        symbolsAndValues[symbols[i]] = (tempLastNum, 255)
```
In this block of code I do a couple of useful things; I converted the images's color to black and white and then loaded in that pixel data using the PIL library, specified which symbols I'd be using for the ascii art, and then built the ```symbolsAndValues``` dictionary. The ```symbolsAndValues``` dictionary consists of black and white color values, which is an RGB value represented by a number between 0 and 255, with their associated symbol. This is what the dictionary looks like if you are using ten different values:

```console
(0, 23) !
(23, 46) @
(46, 69) #
(69, 92) $
(92, 115) %
(115, 139) ?
(139, 162) &
(162, 185) *
(185, 208) :
(208, 255) ~
```
