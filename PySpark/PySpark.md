## Spark - Concept:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-fe3aebc6c5c427c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/9147346-20229a63adc68e3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Component :
![image.png](http://upload-images.jianshu.io/upload_images/9147346-777e92a31d2ee8eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* Python vs. Scala
![image.png](http://upload-images.jianshu.io/upload_images/9147346-20add4363afe73a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## RDD Concept:
* SparkContext:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-92053055216a795d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/9147346-7f65372a7844211a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* Transform RDD's
![image.png](http://upload-images.jianshu.io/upload_images/9147346-26b32f8969b66f16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Example:
> rdd= sc.parallelize([1,2,3,4])
> squareRDD = rdd.map(lambda x:x*x)

* Lazy evaluation:
No changes until actions been called
* Config:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-4e054206feb19a42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* PySpark -> Worst Movie：
```
from pyspark import SparkConf, SparkContext

# This function just creates a Python "dictionary" we can later
# use to convert movie ID's to movie names while printing out
# the final results.
def loadMovieNames():
    movieNames = {}
    with open("ml-100k/u.item") as f:
        for line in f:
            fields = line.split('|')
            movieNames[int(fields[0])] = fields[1]
    return movieNames

# Take each line of u.data and convert it to (movieID, (rating, 1.0))
# This way we can then add up all the ratings for each movie, and
# the total number of ratings for each movie (which lets us compute the average)
def parseInput(line):
    fields = line.split()
    return (int(fields[1]), (float(fields[2]), 1.0))

if __name__ == "__main__":
    # The main script - create our SparkContext
    conf = SparkConf().setAppName("WorstMovies")
    sc = SparkContext(conf = conf)

    # Load up our movie ID -> movie name lookup table
    movieNames = loadMovieNames()

    # Load up the raw u.data file
    lines = sc.textFile("hdfs:///user/maria_dev/ml-100k/u.data")

    # Convert to (movieID, (rating, 1.0))
    movieRatings = lines.map(parseInput)

    # Reduce to (movieID, (sumOfRatings, totalRatings))
    ratingTotalsAndCount = movieRatings.reduceByKey(lambda movie1, movie2: ( movie1[0] + movie2[0], movie1[1] + movie2[1] ) )

    # Map to (rating, averageRating)
    averageRatings = ratingTotalsAndCount.mapValues(lambda totalAndCount : totalAndCount[0] / totalAndCount[1])

    # Sort by average rating
    sortedMovies = averageRatings.sortBy(lambda x: x[1])

    # Take the top 10 results
    results = sortedMovies.take(10)

    # Print them out:
    for result in results:
        print(movieNames[result[0]], result[1])
```
Then Submit:
> spark-submit LowestRatedMovie.py 

Result:
> ('3 Ninjas: High Noon At Mega Mountain (1998)', 1.0)                            
('Beyond Bedlam (1993)', 1.0)
('Power 98 (1995)', 1.0)
('Bloody Child, The (1996)', 1.0)
('Amityville: Dollhouse (1996)', 1.0)
('Babyfever (1994)', 1.0)
('Homage (1995)', 1.0)
('Somebody to Love (1994)', 1.0)
('Crude Oasis, The (1995)', 1.0)
('Every Other Weekend (1990)', 1.0)

