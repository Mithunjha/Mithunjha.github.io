# ITK Python Functions : Concept and Codes

# Image Adaptors

The purpose of an image adaptor is to make one image appear like another image, possibly of a different pixel type.

e.g.: Adapt an unsigned char image to make it appear as an image with pixel type float.

### **Casting Filter vs Image adaptor**

- Advantages of Image Adaptors over casting filter:
    1. Avoid extra memory resources required by filters (Float requires 4x the memory of the original Image)
    2. Computation is done on the fly during the operation, which leads to less computation time and less memory requirement. 
- Disadvantage of Image adaptor:
    1. Image adaptors are useful if there is infrequent pixel access. 
        
        If the operation is executed multiple times then adaptor will have to compute the cast every time, but the casting filter will cache the output and will not re-execute.
        

![ITK%20Python%20Functions%20Concept%20and%20Codes/Untitled.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/Untitled.png)

Pixel accessors : does the transformation and Image Iterator : iterates through every pixel of the image.

![ITK%20Python%20Functions%20Concept%20and%20Codes/Untitled%201.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/Untitled%201.png)

- Usage of Image Adaptors
    
    Pixel type conversion
    
    Extract one of the components of the vector image
    
    Extract channels of image with multiple components (RGB images)
    
    Threshold an image
    
    Add Constant to Every Pixel (Intensity Transformation)
    
- Limitations
    
    Not all the Iterators support image adaptor 
    
    Not all filter (neighbor filters) supports image adaptors
    

# Techniques

## Binary Thresholding

If the pixel value is inside the range defined by [Lower, Upper] the output pixel is assigned the Inside Value. Otherwise the output pixels are assigned to the Outside Value.

![ITK%20Python%20Functions%20Concept%20and%20Codes/1.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/1.png)

![ITK%20Python%20Functions%20Concept%20and%20Codes/2.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/2.png)

## General Thresholding

### Threshold Outside

![ITK%20Python%20Functions%20Concept%20and%20Codes/3.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/3.png)

Difference with binary thresholding : Inside Value is Unchanged.

### Threshold Below

![ITK%20Python%20Functions%20Concept%20and%20Codes/4.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/4.png)

### Threshold Above

![ITK%20Python%20Functions%20Concept%20and%20Codes/5.png](ITK%20Python%20Functions%20Concept%20and%20Codes%20216f197324114dc8a614ea988c9336c5/5.png)

## Edge Detection

### Canny Edge Detection

Multi Stage Algorithm

1. Filter image with derivative of Gaussian 
    - Noise reduction and gradient magnitude calculation to localize edge features
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/6.png](ITK%20Python%20Functions%20Concept%20and%20Codes/6.png)
        
    - Derivative Filters
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/10.png](ITK%20Python%20Functions%20Concept%20and%20Codes/10.png)
        
    - Gaussian Filter (Smoothing Filter)
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/11.png](ITK%20Python%20Functions%20Concept%20and%20Codes/11.png)
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/14.png](ITK%20Python%20Functions%20Concept%20and%20Codes/14.png)
        
    - Derivative of Gaussian
        
        Smoothed derivative removes the noise present in the image, and also blurs the edges. 
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/15.png](ITK%20Python%20Functions%20Concept%20and%20Codes/15.png)
        
2. Find magnitude and orientation of gradient and perform Non Maximum suppression
    - Thinning of edges/ to remove spurious features
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/7.png](ITK%20Python%20Functions%20Concept%20and%20Codes/7.png)
        
        Check if pixel is local maximum along gradient direction
        
3. Hysteresis : thresholding (to yield binary image) and Linking
    - High threshold to start a edge and low threshold to continue them
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/16.png](ITK%20Python%20Functions%20Concept%20and%20Codes/16.png)
        
        ![ITK%20Python%20Functions%20Concept%20and%20Codes/8.png](ITK%20Python%20Functions%20Concept%20and%20Codes/8.png)
        
- Final Output
    
    ![ITK%20Python%20Functions%20Concept%20and%20Codes/9.png](ITK%20Python%20Functions%20Concept%20and%20Codes/9.png)
    

## Gradients

The gradient of an image measures how it is changing. e.g : Finding edges

- Theory
    
    Gradient Magnitude tell how quickly the image is changing and direction tells the direction in which the image is changing.
    
    ![ITK%20Python%20Functions%20Concept%20and%20Codes/17.png](ITK%20Python%20Functions%20Concept%20and%20Codes/17.png)
    
- Example
    
    ![ITK%20Python%20Functions%20Concept%20and%20Codes/18.png](ITK%20Python%20Functions%20Concept%20and%20Codes/18.png)
    
- Gradient Magnitude : Computes the gradient magnitude of an image region at each pixel.
- Gradient with Smoothing : Computes the Magnitude of the Gradient of an image by convolution with the first derivative of a Gaussian.
- Derivative : Computes the directional derivative of an image. The directional derivative at each pixel location is computed by convolution with a derivative operator of user-specified order.

## Smoothing / Blurring

### Binomial Blurring

It computes a nearest neighbor average along each dimension. The process is repeated a number of times, as specified by the user. Computation time increase linearly with number of repetition.

1D : Applying 0.5*[1,1] averaging filter for number of times specified in the input.

## Reference

Image processing lecture slides

[Canny Edge Detection Step by Step in Python - Computer Vision](https://towardsdatascience.com/canny-edge-detection-step-by-step-in-python-computer-vision-b49c3a2d8123)