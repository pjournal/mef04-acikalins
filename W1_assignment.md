# Introduction
### About Me
I am currently working at Odeabank as Customer Analytics and Campaign Management Assistant Manager for nearly 1,5 years. I worked at several positions such as marketing, product/lifecycle management, product development roles until started to work in Analytics & Campaign Management. Have 8 years experience in total with the current one. I graduated from Boğaziçi University Economics Department. My short term target is to improve the skills  i will obtain through out the program by applying on my current work. My long term goal is to go deeper on both data science and computer science by having a work that makes it possible.

### Linkedin Profile

- [You may want to check my linkedin profile](https://www.linkedin.com/in/selçuk-açıkalın-8a18b361)

# Reviews
## Video Review 
### useR! 2020: Reactor: reactive notebooks for R (H. Susmann), lightening

In this video Herb Susmann is presenting his work named as "Reactor" that is explained as a new package that creates a new method for using R with a different user experience. He states that his work shows resemblance with previous examples such as Jupyter Notebook or Shiny or Observable however it is falling apart from them by the "reactiveness" feature. Briefly he defines reactor as a notebook interface for R. In other words he describes "Reactor" as a notebook application that enables using R code but if you change any variable in a cell, that causes an automatic update for other cells referenced with the changed variable directly or indirectly. It looks like an excel sheet/cell update mechanism. But in a form of R notebook. It also gives an opportunity use R code with packages plots, HTML, LayTex and markdown all in one in a reactive interface. There is a short demo in the video. The link for the video is provided below.

- [Youtube Video - useR! 2020: Reactor: reactive notebooks for R (H. Susmann), lightening](https://www.youtube.com/watch?v=hytdjtM6Chg)


## Article Reviews
### Hack: How to Install and Load Packages Dynamically

The author of this article George Pipis mentions about a practical solution that could help to load packages easily at times when you try to run an R script that you downloaded from another source. Not everyone use the same packages always for their works related with R script. Different people could use several different packages other than some common ones such as gglot2, dplyr etc. He suggests a short code chunk below to be added in the beginning of the whole code with the replacement of the packages with whichever you use.


```
 mypackages<-c("readxl", "dplyr", "multcomp")
 for (p in mypackages){
  if(!require(p, character.only = TRUE)){
    install.packages(p)
    library(p, character.only = TRUE)
  }
  }
  
Source: The work of George Pipis - https://www.r-bloggers.com/2020/10/hack-how-to-install-and-load-packages-dynamically/
```


He tells that this code chunk looks for the related packages to find out if it is already installed on the machine running the code at first. If it detects that it is already installed, then abandon the rest of this code chunk and starts to execute the rest of R code. If it can not detect relevant packages it automatically installs the packages itself. Sound really useful. If you felt more curious about you may find the original post in the below link.

- [Hack: How to Install and Load Packages Dynamically by George Pipis](https://www.r-bloggers.com/2020/10/hack-how-to-install-and-load-packages-dynamically/)


### “You Are Here”: Understanding How GPS Works

In this article the author narrates the basics of the Global Positioning System with the support of R script. The author mentions about how gps works and maths behind the system. He/she visualizes the basic principle with R by creating a 2D case of gps having the earth with 2 satellites. Shared an R code that solves intersection point of two circles of the two satellites' and matching it with the earth's radius. According to the article, the rationale behind taking circles is that when you receive a gps signal you also get the time data of the signal is processed. This gives the chance to calculate the distance of the point where the signal is made since you know the difference of both current time and satellites' time which the signal is created at. You also know the speed of the signal. Taking all of this you may come up with the distance of the satellite to the subject as the radius of a circle gives you all possible locations where it is produced. He/she defines a custom function to R named trillaterate using trillateration method to determine the exact unique position of the subject in the 2 axis universe. If you are interested about how it works you may check out below link.

- [“You Are Here”: Understanding How GPS Works by the blogger nicknamed Learning Machine ](https://www.r-bloggers.com/2020/10/you-are-here-understanding-how-gps-works/)


### How to Impute Missing Values in R

This article is suggesting an easy way to replace null values in the dataset through an R code that automatically detects the structure of the data whether it is a numerical or a categorical and replaces the null values with the mean or the median of the dataset according to the vector's structure type taking it as a decisive element. This makes it easy to get the data ready for the task we work on which could be setting a model. In fact in some situations it would not be a useful approach. Because it depends on the type of the work you carry out whether you need the imputation step or not. However if you needed this could work out as an easy solution as well. If you want to take a glance at the code chunk the link is below.

- [How to Impute Missing Values in R by George Pipis](https://www.r-bloggers.com/2020/04/how-to-impute-missing-values-in-r/)

Thanks for reading my reviews.
