## HDFS Concept
* Cooperation
HDFS have several types of nodes to cooperate the handling of big data.
![image.png](http://upload-images.jianshu.io/upload_images/9147346-7241d83ff3dcc5fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* Resiliency
HDFS stored its data multiple copies in different Data nodes, so you can still retrieve your data when one nodes burnt down. Name Node is often backed up in local disk and NFS for resiliency. There are also secondary Name Node for backup, and Zookeeper tracks active name nodes for management.
* Federation
HDFS's Name node manages specific volume for the corresponding data node.
* Use of HDFS
![image.png](http://upload-images.jianshu.io/upload_images/9147346-f5a81c8a365d2975.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## HDFS Usage
* First lanching Hortonworks Docker :
![image.png](http://upload-images.jianshu.io/upload_images/9147346-d9ceea18c38a8a72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1)Web Interface
* Then log into Ambari port which is 127.0.0.1:8080, and we can get into HDFS virtual machine web interface:
![image.png](http://upload-images.jianshu.io/upload_images/9147346-a6bb8d89bbfefbeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Switching to the File View and we can create folder/ upload data/ view data
![image.png](http://upload-images.jianshu.io/upload_images/9147346-c2abc76ffe22c727.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](http://upload-images.jianshu.io/upload_images/9147346-3bffe8bae4c92086.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2)Command Line Interface
* Loggin using command: 
ssh "your user name"@127.0.0.1 -p 2222

* Code are similar with Linux shell: 
> hadoop fs -ls
> hadoop fs -mkdir ml-100k
> wget http://media.sundog-soft.com/hadoop/ml-100k/u.data
> hadoop fs -copyFromLocal u.data ml-100k/u.data
> hadoop fs -rm ml-100/u.data
> hadoop fs -rmdir ml-100k

You can also find command functions by typing:
> hadoop fs

Exit
> exit
