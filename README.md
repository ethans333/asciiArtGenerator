# asciiArtGenerator


###Here's the code:
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
    symbolsAndValues[symbols[i]] = tempLastNum, math.floor(i * 255 / len(symbols))
    tempLastNum = math.floor(i * 255 / len(symbols))

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
