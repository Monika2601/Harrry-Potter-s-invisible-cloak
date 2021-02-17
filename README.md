# Harrry-Potter-s-invisible-cloak

import numpy as np #for arrays
import cv2    #computer vision
import time   #for learning time
cap=cv2.VideoCapture(0)  #0 bcz we are going to use only one camera
time.sleep(3) #time to set camera in sec
background=0  #to capture original background
#capturing the background
for i in range(30):  #iterations to capture best background
    ret, background = cap.read()  #show background captured + it will return the true value eg. if it return the false value then it will not show any kind of visibility
#capturing the image
while(cap.isOpened()): #capture video till we are there it cant capture when we are not there
    ret , img = cap.read()

   if not ret:
    	break
    hsv=cv2.cvtColor(img,cv2.COLOR_RGB2HSV) #red green blue HSV( Hue saturation and value)
    #HSV values
    lower_red=np.array([0,120,70]) #120-255 gives us darkness of color 70-255 brightness 0-10 color 
    upper_red=np.array([10,255,255]) 

   mask1=cv2.inRange(hsv,lower_red,upper_red)
    	#cloak camera if it find any value b/w upper and lower range
    lower_red=np.array([170,120,70])
    upper_red=np.array([180,255,255])

   mask2=cv2.inRange(hsv,lower_red,upper_red)
    mask1=mask1+mask2
    mask1=cv2.morphologyEx(mask1,cv2.MORPH_OPEN,np.ones((3,3),np.uint8),iterations=2) #extract pic from  noise  (3,3)mul  pixels and move

   mask1=cv2.morphologyEx(mask1,cv2.MORPH_DILATE,np.ones((3,3),np.uint8),iterations=1)

   mask2=cv2.bitwise_not(mask1)
    
   result1=cv2.bitwise_and(background,background,mask=mask1)
    result2=cv2.bitwise_and(img,img,mask=mask1)

   final_output=cv2.addWeighted(result1,1,result2,1,0)

   cv2.imshow('Harry !',final_output)
    k=cv2.waitKey(10)
    if k==27:
    	break

cap.release()
cv2.destroyAllWindows()    	
