# YARN & MapReduce 2
## 1 MapReduce JAVA
+ **Pre-work:**
  + OpenJDK 8
  + Git
  + IntelliJ IDEA  
  

+ A JAVA project hosted on github    
> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.1.png)

+ **Maven a JAR package**  

> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.2.png)
+ **Send the JAR to the edge node**
  + >Cause I use the `Xshell` as my workshop,so I use `Xftp` which is his sub-product as my 'FileZilla'
    ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.3.png)
+ **Run the job** 
  +  **First, the delimiter of dataset is ';'**  
    `StringTokenizer itr = new StringTokenizer(value.toString(),";");`
  + >Use this command   
    `hadoop jar /home/y.wang/hadoop-examples-mapreduce-1.0-SNAPSHOT.jar com.opstty.job.WordCount   ./data/lab2/*  ./resualt`  
  + >`hadoop jar` ->Run a JAR  
    > 
    > `/home/y.wang/hadoop-examples-mapreduce-1.0-SNAPSHOT.jar` ->The location of JAR  
    > 
    > `com.opstty.job.WordCount` ->is the full path to the main class WordCount.java, right click in IDEA USING *copy reference* to get  
    >
    > `./data/lab2/*  ./resualt`   -> input & output
    > 
    > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.4.png)
    > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.5.png)
## 2 Remarkable trees of Paris  
  
### **Firstly, I need to change some code in order to I can test in Windows or somewhere.**  
  >So the WordCount will add followings:  
  > BTY, I'll just change the mapper and reducer to accomplish all EXs.  
  >```
  >//I want to check the logic of my code in windows
  >//Let the framework know is windows heterogeneous platform running
  >conf.set("mapreduce.app-submission.cross-platform","true");
  >//conf.set("mapreduce.framework.name","local");
  >//Commit a JAR
  >String jarpath= '';
  >//job.setJar(jar_path);
  > ```
  >overview:  
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.4.png)  
 



### **Now, Start experiment**  
  
+ **Districts containing trees**  
  >Do `.toString()` in an iterator.     
  > Then Split it and pick the second value as KEY  
  > ``` 
  > public void map(Object key, Text value, Context context)
  >          throws IOException, InterruptedException {
  >      StringTokenizer itr = new StringTokenizer(value.toString(),"\n");
  >      while (itr.hasMoreTokens()) {
  >
  >          word.set( itr.nextToken().split(";")[2]);
  >
  >          context.write(word, one);
  > ```  
  >  
  > overview about Mapper:  
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.3.0.png)
  > Finally, Run it.
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.2.png)  
  >   

+ **Show all existing species**
  >Do `.toString()` in an iterator.     
  > Then Split it and pick the third value as KEY
  > ``` 
  > public void map(Object key, Text value, Context context)
  >          throws IOException, InterruptedException {
  >      StringTokenizer itr = new StringTokenizer(value.toString(),"\n");
  >      while (itr.hasMoreTokens()) {
  >
  >          word.set( itr.nextToken().split(";")[3]);
  >
  >          context.write(word, one);
  > ```  
  > overview about Mapper:  
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.2.0.png)
  > Finally, Run it.
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.3.png)
+ **Number of trees by kinds**
  >I think I have already done that in second part
  >>In Mapper:
  >>Do `.toString()` in an iterator.     
  >>Then Split it and pick the third value as KEY
  >>``` 
  >>public void map(Object key, Text value, Context context)
  >>         throws IOException, InterruptedException {
  >>     StringTokenizer itr = new StringTokenizer(value.toString(),"\n");
  >>     while (itr.hasMoreTokens()) {
  >>
  >>          word.set( itr.nextToken().split(";")[3]);
  >>
  >>          context.write(word, one);
  >> ```  
  >> overview about Mapper:  
  >> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.2.0.png)
  >
  >>In reducer:  
  >>
  >>```
  >>public class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
  >>    private IntWritable result = new IntWritable();
  >>    public void reduce(Text key, Iterable<IntWritable> values, Context context)
  >>        throws IOException, InterruptedException {
  >>    int sum = 0;
  >>    for (IntWritable val : values) {//11111111
  >>        sum += val.get();
  >>    }
  >>    result.set(sum);
  >>    context.write(key, result);
  >>    }
  >>
  >>```
  >> overview about Reduce:  
  >>
  >> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.2.1.png)
  > Finally, Run it.  
  > 
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.3.png)

+ **Maximum height per kind of tree**
  >We need modify Both the Mapper the Reducer
  >>In Mapper:
  >>Do `.toString()` in an iterator.  as usual   
  >>Then Split it and pick the third value as KEY
  >>And put the 6th value as Value
  >>``` 
  >>public void map(Object key, Text value, Context context)
  >>         throws IOException, InterruptedException {
  >>     StringTokenizer itr = new StringTokenizer(value.toString(),"\n");
  >>     while (itr.hasMoreTokens()) {
  >>
  >>          word.set( itr.nextToken().split(";")[3]);
  >>          one.set(Integer.parseInt(itr.nextToken().split(";")[6]));
  >>          context.write(word, one);
  >> ```  
  >> overview about Mapper:  
  >> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.5.png)
  >
  >>In reducer:
  >>add a `IF {}`
  >>```
  >>public class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
  >>    private IntWritable result = new IntWritable();
  >>    public void reduce(Text key, Iterable<IntWritable> values, Context context)
  >>        throws IOException, InterruptedException {
  >>    int sum = 0;
  >>    for (IntWritable val : values) {//11111111
  >>          if (sum <= val.get()){
  >>              sum=val.get();
  >>          };
  >>      }
  >>    result.set(sum);
  >>    context.write(key, result);
  >>    }
  >>
  >>```
  >> overview about Reduce:
  >>
  >> ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/2.4.png)
  > Finally, Run it.
  >
  > ![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Java_MR/1.4.png)
  

