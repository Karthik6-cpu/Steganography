import cv2  #open cv image
import numpy as np # array
import string
import os
import matplotlib.pyplot as plt
# ACII conversion
d = { chr(i) : i for i in range(255)}    # character to ascii
c = { i : chr(i) for i in range(255)}    # ascii to character

# load the image
image_path ="/content/KARTHIKEYA.jpg"
x=cv2.imread(image_path)

# Add a check to see if the image was loaded successfully
if x is None:
    print(f"Error: Could not load image from {image_path}")
    # Exit or handle the error appropriately, e.g., by not proceeding with image processing
else:
    xrgb=cv2.cvtColor(x,cv2.COLOR_BGR2RGB)
    plt.imshow(xrgb)
    plt.axis('off')
    plt.show()    # size of the array for this x.shape
    key ="123"
    text = "Karthikeya"
    text_ascii = [d[ch] for ch in text]  # list of ascii value of text character
    key_ascii = [d[ch] for ch in key]    # list of ascii values of key
    print(text_ascii)
    print(key_ascii)
    # encrypt using pixel modification
    x_enc =x.copy()
    n=0 # number of rows
    m=0 # number of coloumns
    z=0 # color panel
    l= len(text)
    kl=0
    for i in range(l):
      orig_val = x_enc[n,m,z]
      new_val = d[text[i]] ^d[key[kl]] 
      x_enc[n,m,z]=new_val 
      n=n+1
      m=m+1 
      z=(z+1)%3
      m = (m+1)%3
      kl = (kl+1)%len(key) 
cv2.imwrite("encrypt.jpg",x_enc)
plt.imshow(cv2.cvtColor(x_enc,cv2.COLOR_BGR2RGB))
plt.title("Encrpyt_Image")
plt.axis('off')
plt.show() 

# decrypt using pixel modification
n,m,z=0,0,0
kl=0
decrypt=""
for i in range(l):
   val = x_enc[n,m,z]
   orig_char= c[val^d[key[kl]]]
   decrypt += orig_char
   n=n+1
   m=m+1
   m=(m+1)%3
   z=(z+1)%3
   kl = (kl+1)%len(key)
print(decrypt)  
