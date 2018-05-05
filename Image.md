# <center>**_MyImage_**</center>
## <center>**_ImplementImageIO_**</center><br>
### **_`myRead()`_**
> - Implement<br>
> 
>> Read the picture with byte, read the key information from the file head (54 byte at the head of file, such as width, height, color). The code as that:<br>
>
```
			FileInputStream fileStream = new FileInputStream(file);  
            //the head of the file
            byte head[] = new byte[14];  		 	
            //read the head of file
            fileStream.read(head, 0, 14);  		 	
            //read the size of file 
            int fileSize = this.getIntFromByte(head, 2, 5); 	
            //the information of the file
            byte infor[] = new byte[40];  		  		
            //read the information of the file
            fileStream.read(infor, 0, 40);  	  			
            //read the width of the bmp
            int bmpWidth = this.getIntFromByte(infor, 4, 7); 
            //read the height of bmp
            int bmpHeight = this.getIntFromByte(infor, 8, 11); 
            //read the size of the bmp
            int bmpSize = this.getIntFromByte(infor, 20, 23);  		
            //read the byte of color
            int bmpByte = this.getIntFromByte(infor, 14, 15);  	
```
>> And then, process the picture, read the RGB of picture and save the color in a array  as int. The way of this: `pixel = 0xff < 24 + Red < 16 + Green < 8 + Blue`. And then use the function `Toolkit.getDefaultToolkit().createImage((ImageProducer) new MemoryImageSource()` to create a image. The key code as that: <br>
> 
```
				if (bmpByte == 24){ 
                //empty byte 
            	int emptyByte = bmpSize / bmpHeight - 3 * bmpWidth;	
                if(emptyByte == 4) {  					
                	emptyByte = 0;						
                }
                int currentPixelByte = 0;
                int pixel[] = new int [bmpWidth * bmpHeight];			
                //PIXEL INFORMATION
                byte bmpPixel[] = new byte[bmpSize];			
                fileStream.read(bmpPixel, 0, bmpSize);				
                for(int i = bmpHeight - 1; i >= 0; i--) {			
                    for( int j = 0; j < bmpWidth; j++) {			
                    	pixel[bmpWidth * i + j] = 0xff << 24 | this.getIntFromByte(bmpPixel, currentPixelByte, currentPixelByte + 2);		
                    	currentPixelByte += 3;	
                    }
                    currentPixelByte += emptyByte;
                }
                myImage = Toolkit.getDefaultToolkit().createImage((ImageProducer) new MemoryImageSource(bmpWidth, bmpHeight, pixel, 0, bmpWidth));
```

### **_myWrite()_**
> - Implement<br>
>  
>> Create a new buffer image and write in a image, and return it.<br>
>
```
			File saveFile = new File(filePath + ".bmp");
    		BufferedImage buffer = new BufferedImage(saveImage.getWidth(null), saveImage.getHeight(null), BufferedImage.TYPE_INT_RGB);
    		Graphics2D graph = buffer.createGraphics();
    		graph.drawImage(saveImage, 0, 0, null);			//NOSONAR
    		graph.dispose();
    		ImageIO.write(buffer, "bmp", saveFile);
    		return saveImage;
```

### **_getIntFromByte()_**
>  
>> Read the byte use this function to simple the process to create a int from RGB byte.<br>
> 
```
		public int getIntFromByte (byte[] stream, int start, int end) {
    	int size = end - start + 1;				//NOSONAR
    	int result = 0;							//NOSONAR
    	for (int i = 0; i < size; i++) {		//NOSONAR
    		result = result | ((stream[end - i] & 0Xff) << (8 * (size - i - 1)));		//NOSONAR
    	}
    	return result;
    }
```

--------

## <center>**_ImplementImageProcessor_**</center><br>

- Implement<br>
>
> Use the `RGBImageFilter` to change the RGB.<br>
> 
### **_Color_**
> - Implement
>  
>> The enum to save the status to process.<br>
>  
```
enum Color {
	ShowRed, ShowGreen, ShowBlue, ShowGray			//NOSONAR
}
```

### **_MyFilter_**
> - Implement<br>
> 
>> To change the RGB to implement the color change. Extends the class `RGBImageFilter`, and rewrite the function `filterRGB`. Get the arg `Color` and change color with arg. <br>
> 
```
    public int filterRGB(int uselessFir, int uslessSed, int rgb) {   
        if(color == Color.ShowRed) {  
            return (rgb & 0xffff0000);    	//NOSONAR
        } else if(color == Color.ShowGreen) {  
            return (rgb & 0xff00ff00);    	//NOSONAR
        } else if(color == Color.ShowBlue) {  
            return (rgb & 0xff0000ff);    	//NOSONAR
        } else {  
            int g = (int)(((rgb & 0x00ff0000) >> 16) * 0.299 + ((rgb & 0x0000ff00) >> 8) * 0.587 + ((rgb & 0x000000ff)) * 0.114); 	//NOSONAR 
            return (rgb & 0xff000000) + (g << 16) + (g << 8) + g;  																	//NOSONAR
        }  
    }  
```

### **_showChanelR()_**<br>

> - Implement<br>
>  
```
    public Image showChanelR(Image sourceImage){  
    	MyFilter redFilter = new MyFilter(Color.ShowRed);  
        Toolkit toolKit = Toolkit.getDefaultToolkit();  
        return toolKit.createImage(new FilteredImageSource(sourceImage.getSource(), redFilter)); 
    }
```

-----------

## <center>**_Run_**</center><br>

```
public class Run {
    public static void main(String args[]){
        ImplementImageIO myImageIO = new ImplementImageIO();
        ImplementImageProcessor myImageProcessor = new ImplementImageProcessor();
        Runner.run(myImageIO, myImageProcessor);
    }
}
```

------------

## <center>**_ImplementProcessorTest_**</center>
- Implement<br>
> 
> Use the function to compare two picture. Get test picture and get processed picture and compare with goal picture.<br>
### **_compareImage()_**
> - Implement<br>
>  
>> To compare the width and height of two picture. And then get the RGB to compare. (Use `bufferedImage` to compare).<br>
>
```
	public boolean compareImage (Image img1, Image img2) {
		
		int weight1 = img1.getWidth(null);
		int height1 = img1.getHeight(null);
		int weight2 = img2.getWidth(null);
		int height2 = img2.getHeight(null);
		
		//compare the weight and height
		if (weight1 != weight2 || height1 != height2) {
			return false;
		}
		
		int rgb1[] = new int[weight1 * height1];
		int rgb2[] = new int[weight2 * weight2];
		
		BufferedImage buffer1 = toBufferedImage(img1);
		BufferedImage buffer2 = toBufferedImage(img2);
		
		//get the RGB
		buffer1.getRGB(0, 0, weight1, height1, rgb1, 0, weight1);
		buffer2.getRGB(0, 0, weight2, height2, rgb2, 0, weight2);
		
		//computer the color of every cell
		for (int i = 0; i < weight1 * height1; i++) {
			if (rgb1[i] != rgb2[i]) {
				return false;
			}
		}
		return true;
	}
```

### **_toBufferedImage()_**<br>

> - Implement<br>
>  
>> Get the buffered image use a image. Use the API. <br>
>
```
	public static BufferedImage toBufferedImage(Image img)
	{
	    if (img instanceof BufferedImage)
	    {
	        return (BufferedImage) img;
	    }

	    // Create a buffered image with transparency
	    BufferedImage bimage = new BufferedImage(img.getWidth(null), img.getHeight(null), BufferedImage.TYPE_INT_ARGB);

	    // Draw the image on to the buffered image
	    Graphics2D bGr = bimage.createGraphics();
	    bGr.drawImage(img, 0, 0, null);
	    bGr.dispose();

	    // Return the buffered image
	    return bimage;
	}
```

### **_test()_**

> - Implement<br>
>  
>> Test the two test picture, every picture test four times, which include R, G, B, Gray.<br>
>
```
	@Test
	public void test() {
		ImplementImageIO io = new ImplementImageIO();
		Image testFir = io.myRead("bmptest/1.bmp");
		
		//test first picture
		//test Blue
		Image goal = io.myRead("bmptest/goal/1_blue_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelB(testFir)));
		
		//test Red
		goal = io.myRead("bmptest/goal/1_red_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelR(testFir)));
		
		//test Green
		goal = io.myRead("bmptest/goal/1_green_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelG(testFir)));
		
		//test Gray
		goal = io.myRead("bmptest/goal/1_gray_goal.bmp");
		assertTrue(compareImage(goal, pro.showGray(testFir)));
		
		//test second picture
		Image testSec = io.myRead("bmptest/2.bmp");
		
		goal = io.myRead("bmptest/goal/2_blue_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelB(testSec)));
		
		goal = io.myRead("bmptest/goal/2_red_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelR(testSec)));
		
		goal = io.myRead("bmptest/goal/2_green_goal.bmp");
		assertTrue(compareImage(goal, pro.showChanelG(testSec)));
		
		goal = io.myRead("bmptest/goal/2_gray_goal.bmp");
		assertTrue(compareImage(goal, pro.showGray(testSec)));
	}
```
