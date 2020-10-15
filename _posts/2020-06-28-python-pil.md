---
layout: post
title: python PIL numpy 
date: 2020-06-28
tags: PIL numpy 
---

作業 ....
```
import PIL
from PIL import Image
from PIL import ImageEnhance
import numpy as np

def f(img,l):
    img = img*np.array([l])
    img = img.astype(int)
    return Image.fromarray(np.uint8(img))

# read image and convert to RGB
image=Image.open("readonly/msi_recruitment.gif")
image=image.convert('RGBA')
im2arr = np.array(image)
images = []
images.append(f(im2arr,[0.1,1,1,1]))
images.append(f(im2arr,[0.5,1,1,1]))
images.append(f(im2arr,[0.9,1,1,1]))

images.append(f(im2arr,[1,0.1,1,1]))
images.append(f(im2arr,[1,0.5,1,1]))
images.append(f(im2arr,[1,0.9,1,1]))

images.append(f(im2arr,[1,1,0.1,1]))
images.append(f(im2arr,[1,1,0.5,1]))
images.append(f(im2arr,[1,1,0.9,1]))


# create a contact sheet from different brightnesses
first_image=images[0]
contact_sheet=PIL.Image.new(first_image.mode, (first_image.width*3,first_image.height*3))
x=0
y=0

for img in images:
    # Lets paste the current image into the contact sheet
    contact_sheet.paste(img, (x, y) )
    # Now we update our X position. If it is going to be the width of the image, then we set it to 0
    # and update Y as well to point to the next "line" of the contact sheet.
    if x+first_image.width == contact_sheet.width:
        x=0
        y=y+first_image.height
    else:
        x=x+first_image.width

# resize and display the contact sheet
contact_sheet = contact_sheet.resize((int(contact_sheet.width/2),int(contact_sheet.height/2) ))
display(contact_sheet)
```
 <iframe src="./images/pil_assignment_1.html" width="1000px" height="2000px" frameborder="0" scrolling="no"></iframe>

輸出是 ...

<object data="/images/pil_assignment_1.pdf" type="application/pdf" width="100%" height="700px"> 
</object>

上面是作業  ...
