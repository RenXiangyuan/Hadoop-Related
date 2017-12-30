## MapReduce Concept
MapReduce is natively Java, allowed to interface with Python under Streaming

* Like dictionary in data structure
![image.png](http://upload-images.jianshu.io/upload_images/9147346-0fa6696ad6f993bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Shuffle Keys and Sort Values
![image.png](http://upload-images.jianshu.io/upload_images/9147346-6260d80aba23a7fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Reducer
![image.png](http://upload-images.jianshu.io/upload_images/9147346-30f205c68757b43e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* So overall operation is like:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-7fde17747d2e66c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Handle of failure
![image.png](http://upload-images.jianshu.io/upload_images/9147346-843688676e8209bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## MapReduce Coding
* Problem:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-8b150fdc21c2b538.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Map Function:
> def mapper_get_ratings(self, _, line):
       &nbsp;&nbsp;&nbsp; (userID, movieID,rating,Timestamp)=line.split('\t')
       &nbsp;&nbsp;&nbsp;&nbsp; yield rating,1

* Reduce Funtion:
> def reducer_count_ratings(self, key, values):
       &nbsp;&nbsp;&nbsp; yield key, sum(values)

* After putting them together:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-94b2ef1af04a6314.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Installation & Preparation
* After log into the command line interface:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-1b1b1e6a17256fa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Run locally:
> python RatingsBreakdown.py u.data 
* Run with Hadoop:
> python RatingsBreakdown.py -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar u.data 
* Result should be :
![image.png](http://upload-images.jianshu.io/upload_images/9147346-47fa7ac9304f7996.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* Plus, codes for more complex problems (sorted for movie numbers):
![image.png](http://upload-images.jianshu.io/upload_images/9147346-fc508b01bb8cdf7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
