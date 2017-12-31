## Ambari
* Ambari provides Dashboard:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-ad606f52a85c1a84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* Enable Admin Users:
> ssh yourname@127.0.0.1 -p 2222
> su root
> ambari-admin-password-reset

## Pig Concept
![image.png](http://upload-images.jianshu.io/upload_images/9147346-eee4622905d9e08b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Usage of Pig:
1. Grunt
2. Script
3. Ambari

### example -> find the oldest 5-star movie
* New Script in Pig View

![image.png](http://upload-images.jianshu.io/upload_images/9147346-d509a02e9c7ae9d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Load data:
>ratings = LOAD 'ml-100k/u.data' AS (userID:int, movieID:int, rating:int, ratingTime: int);


> metadata = LOAD 'ml-100k/u.item' USING PigStorage('|') AS (movieID:int, movieTitle:chararray, releaseDate:chararray,
videoRelease:chararray, imdbLink:chararray);


* FOREACH/GENERATE:
> nameLookup = FOREACH metadata GENERATE movieTitle, ToUnixTime(ToDate(releaseDate, 'dd-MM-yyyy')) AS releaseTime;

* Group By
> ratingsByMovie = Group ratings BY movieID;

*Return Result:
>   avgRatings = Foreach ratingsByMovie Generate group AS movieID, AVG(ratings.rating) AS avgRating;
> fiveStarMovies= Filter avgRatings By avgRating > 4.0;
> fiveStarsWithData = join fiveStarMovies by movieID, nameLookup by movieID;
> oldestFiveStarMovie = order fiveStarsWithData by nameLookup::releaseTime;

> dump oldestFiveStarMovie;

* Result: (Runtime - several minutes)
![image.png](http://upload-images.jianshu.io/upload_images/9147346-6fa241eeb35a2aed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* With Tez, it can shrink into 1 minute
![image.png](http://upload-images.jianshu.io/upload_images/9147346-c40f06f64841b668.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
