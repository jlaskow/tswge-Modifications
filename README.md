# tswge-Modifications
tswge is a time series package for R developed by Dr. Wayne Woodward and Dr. Bivin Sadler of Southern Methodist University.  With a suite of functions, users have the ability to explore deep and nuanced characteristics of any time series data. 

This package is readily available for install on local and cloud-based R platforms, and its robust functionality makes it accessible to both experts in the field as well as newcomers to analytical topics. 


There is, however, room for improvement within the package. We aim to explore solutions to these pitfalls in this project, and to finally build an updated tswge package with improved functionality. 


# Part 1: roll.win.rmse.wge Output Bug

tswge offers a streamlined function for users to calculate the rolling window RMSE (Root Mean Square Error) for any predictions they make. Output will typically appear as a text box reporting:
    - The number of windows in the Rolling Window RMSE calculation
    - Summary statistics for the Rolling Window RMSE
    - A final mean Rolling Window RMSE

![Screenshot 2024-12-28 11 54 56 PM](https://github.com/user-attachments/assets/a8a3931f-797c-4a17-994c-f4afa8539329)



Occasionally, however, this output includes a series of 'y.arma' lines with seemingly random numbers associated with it. 

![Screenshot 2024-12-28 11 55 36 PM](https://github.com/user-attachments/assets/fb73076a-8085-4f20-a436-79ac9db60d04)



The quantity of these superfluous lines generated appears dependent on the size of the training set. While this putput isn't harmful, it can take up considerable space in a document, making it harder for readers and users to keep track of relevant infomration. 

We identify that this extra output only appears when forecasts are made using a model containing an s (seasonality) or d (unit circle) component. We further identify that the output is generated from a chunk of code in the roll.win.rmse.wge function referencing s and d components:



![image](https://github.com/user-attachments/assets/59b85708-be06-49fb-9b4a-3eda46c7ccc8)


Neither this chunk nor any other segment of the function possess a statement referencing y.arma; however, it does reference a function called fore.arma.wge2. Within this function, we have a cat() statement referencing y.arma:

![image](https://github.com/user-attachments/assets/747ac3f6-adcc-497d-9452-5e457f2e2067)


Rather than delete the cat() line and potentialy produce unwanted errors in other related functions, we chose to write a script that supresses the cat() line before generating the final output. 



![image](https://github.com/user-attachments/assets/3e3f1ab1-fa4b-4654-bd42-08912b0af4f4)

This results in a final output without the unwanted y.arma lines.

![image](https://github.com/user-attachments/assets/38254fb0-625f-4fa7-b735-8eb1abf21269)







# Part 2: Unwanted roll.winrmse.wge Rolling Window graphs (Ongoing)

The next issue we seek to address is the generation of unwanted forcast graphs whenever we run roll.win.rmse.wge with s or d components. In these cases, the function will produced a series of rolling window prediction graphs based on the size of your training set. 



![image](https://github.com/user-attachments/assets/4bc88b6f-76b0-461d-bc4f-9cb01cae09e6)

This bug can turn a report spanning 30 pages or less into one with multiple hundreds of pages, as the roll.win.rmse.wge function with spit out unwanted graph after unwanted graph. This can bury important data in-between dozens, even hundreds, of pages of meaningless graphs; or worse, the immense memory demands required for knitting such a needlessly large document could cause R to freeze or crash, wasting time and resources that could better be spent on other tasks. 


In this segment we exxplored several methods to address this bug without altering code in any subfunctions - which, again, could cause untoward errors in other functions. 




[To be updated]






