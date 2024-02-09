# BiggerPockets project: NLP (Natural Language Processing) x Sentimenet analysis  
Welcome to this repository and dashboard about using NLP (natural language processing) to summarize YouTube comments from one of our favorite shows! The Bigger Pockets is a producer of Youtube, Podcast, and other products focusing on real estate investments and financing; it is a roadmap for financial freedom through real estate investment. This is one of our favorite shows! The YouTube channel of Bigger Pockets (https://www.youtube.com/c/biggerpockets) has been producing informational videos since 2016, with 1.16M subscribers and 3K videos released so far! (Great contents BTW) As a big fan of the channel, we utilized NLP and sentiment analyses to draw insights into the audience's responses to each episode of the Bigger Pockets release. 

![image](https://github.com/HD013/BiggerPockets_NLP-/assets/80353259/459ae657-fad8-4198-9b95-4b5376795639)

## **Authors:**
This is a collaborative project featuring two data scientists: Dr. David Henderson (see profile here - https://github.com/HD013) & Dr. Yingtong "Amanda" Wu (see profile here - https://github.com/YingtongAamandaWu)

## **File directory:**

### 01_Codes: a folder containing jupyter notebooks of python codes showing the intermediate steps and exploratory data analyses

### 02_Results: a folder containing csv tables and figures produced from the file

### 03_Dashboards:

## **Result highlights:**

### **Highlight 1 - How positive are the Youtube comments for each video?**

![Plotly_polarity_1300videos](https://github.com/HD013/BiggerPockets_NLP-/assets/80353259/736c3e0e-8f4f-4b2f-8338-878f5e366082)

This is an interactive figure from ["Video_polarity_mean_max_min_plotly.html"]([url](https://github.com/HD013/BiggerPockets_NLP-/blob/main/02_Results/Video_polarity_mean_max_min_plotly.html)), where you can hover over the html file and see the mean, max, and min comment polarity on every video of Bigger Pocket youtube. Note: we excluded youtube videos with less than 30 mins and with less than 10 comments for this analysis. 

### **Highlight 2 - How long of a video should BiggerPockets make to achieve the highest cost-effectiveness (view counts = profits)?**

![60abbbc9-087a-4027-9d62-fc0fb4b1097a](https://github.com/HD013/BiggerPockets_NLP-/assets/80353259/e7ba7ef3-7ae4-4806-9f5d-ccbc3bd25fa4)

We found that a video length of 260 seconds (4 min 20 secs) has the most view count per second -- This seems to be a "sweet spot" of attracting view counts with the minimum efforts spent on making video productions, without compromising the view counts and contents :) 

### **Highlight 3 - What are the main sentiments of this recent video on low-interest rate loan?**
![SingleVideoWordCloud_videoID_IVK5vQg1Uv](https://github.com/HD013/BiggerPockets_NLP-/assets/80353259/e019b801-8fb3-455e-8e28-42f217356664)

As a case study, we produced a wordcloud image based on 124 Youtube comments from this recent video "New Rental Property Mortgages with 3% Interest Rates, 5% Down" (Link https://www.youtube.com/watch?v=IVK5vQg1UvY). Most of the comments are positive, as you can in the figure below: all comments are mainly positive, as indicated by the histogram below. From the wordcloud image above, "Thank" and "Great" are two main keywords that popped up consistently in the comments -- meaning that the audience is very thankful for the information shared about low-interest rate mortgages in 2023!
![image](https://github.com/HD013/BiggerPockets_NLP-/assets/80353259/27e43b04-8874-4457-8a59-38400811e06e=250x250)



