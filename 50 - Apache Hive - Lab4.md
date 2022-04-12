# 1.Hive Beeline Client
In this tutorial, we will verify the connection to Hive using Beeline client



## 1.1 Create a Connection to Beeline Client
+ **Connect to HADOOP cluster using SSH.**
>![image-1.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.1.png)

+ **Initialize a Kerberos ticket.**
>![image-2.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.2.png)

+ **Type the command `beeline` in the terminal prompt, when prompted press enter as username and as password.**
>![image-6.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.3.png)

+ **Connect to the Hive using the command**  
>` 
    !connect jdbc:hive2://hadoop-master01.efrei.online:2181,hadoop-master02.efrei.online:2181,hadoop-master03.efrei.online:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2
`  
>![image-7.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.5.png)

+ **Type help command for list of beeline commands**
>![image-8.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.4.png)

+ **Which command allows you to view the jdbc connection used to connect to HiveServer2?**  
> `!list`
>![image-9.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.6.png)

+ **List all databases.**  
>`show databases;`  
> 
>![image-10.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.7.png)

+ **If not exists, create a database using your username**  
>Use `create database wang_ye;` to create a database.

+ **Use your database.**  
>`use wang_ye;`

+ **List the tables**  
>`show tables;`  
> 
>![image-18.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.8.png)

+ **Create table called *temp* with a column called col of String type.**  
>`create table if not EXISTS temp (col string);`

+ **Confirm the table creation.**  
> Use `show tables` to confirm  
> 
>![image-17.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.9.png)

+ **List the columns (name, data type, etc) of temp table**
>We can use `DESCRIBE FORMATTED temp;`or just`DESCRIBE temp;`  
>>when we use `DESCRIBE FORMATTED temp;` we can all of tables which is named '*temp*' in the whole Hive 
>>![image-15.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.10.png)  
> 
>>when we use `DESCRIBE temp;` we just can show the *temp* in In the current database  
>>
>>![image-16.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.11.png)

+ **Remove the table.**  
>`DROP TABLE IF EXISTS temp;`  
> 
>![image-19.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.12.png)

+ **Type !quit to exit the beeline shell.**
>`!quit`
> 
>![image-20.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.1.13.png)

## 1.2 Create tables
You are going to write some *Hive SQL queries* on the remarkable trees of Paris
using this dataset. If you got issues with TEZ, you must use the MapReduce
engine for Hive USE `set hive.execution.engine=mr;`.

+ **Create an external table called trees external. ** 
  ```
    CREATE TABLE `trees_external`(
    `GEOPOINT`  STRUCT<Long:DECIMAL, Lat:DECIMAL>,
    `ARRONDISSEMENT` INT,
    `GENRE` String,
    `ESPECE` String,
    `ANNEE_PLANTATION` DATE,
    `HAUTEUR` INT,
    `CIRCONFERENCE` INT,
    `ADRESSE` String,
    `NOM COMMUN` String,
    `VARIETE` String,
    `OBJECTID` INT,
    `NOM_EV` String
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ';' 
    COLLECTION ITEMS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE;
    ```
> ![image-29.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.1.png)  
> THE RESAULT  
>![image-24.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.2.png)

+ **Create an internal table called trees internal.**  
>Just Use the same method as above but change the table name to *`trees_internal`*  
>The most different between these is ***external*** 
> 
>`CREATE  TABLE IF NOT EXISTS `  
> 
>![image-25.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.4.png)


+ **Import data to the internal table using the external table.**  
>Fristly,Import data to external table
>>Use  
>>`load data local inpath '/home/y.wang/data/hive/trees.txt' overwrite into TABLE trees_external;`  
>>to laod data from Linux to Hive  
>>![image-27.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.5.png)  
>>However, there are something wrong with my node. So I try to aod data from HDFS to Hive:  
>>use`load data inpath "//efrei/user/y.wang/data/Hive/trees.txt" overwrite  into table trees_external;`  
>>![image-28.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.6.png)  
>
>After than that, Import data to the internal table using the external table.
>> Use `insert into trees_internal select * from trees_external;`  
>>
>>![image-30.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/insertaftersetMR.png)

+ **Verify that each table got the same lines count.**  
>Use this command to both two tables respectively  
>>`select count(*)  from trees_internal;`  
>>![image-31.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.8.png)  
>
>>`select count(*)  from trees_external;`
>>![image-33.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.2.9.png)  
> 
> As we can see, both tables have 98 rows.

## 1.3 Create queries
In this part, you are going to do the same queries as MapReduce ones using the
internal table created before. I will create queries that:
+ **displays the list of distinct containing trees;**  
>Use ` select distinct ESPECE from trees_external;`  
> 
> ![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.1.png)  

+ **displays the list of different species trees;**  
>Use `select ESPECE,count(*)from trees_external group by ESPECE;`  
> 
> ![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.2.png)  

+ **the number of trees of each kind;**  
>Use `select VARIETE,count(*)from trees_external group by VARIETE;` 
> 
> ![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.3.png)  

+ **calculates the height of the tallest tree of each kind;**  
>Use `select VARIETE,max(HAUTEUR)from trees_external group by VARIETE;`
> 
> ![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.4.png)

+ **sort the trees height from smallest to largest;**  
>Use `select VARIETE,min(HAUTEUR)from trees_external group by VARIETE;`
> 
>![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.5.png)

+ **displays the district where the oldest tree is;**  
>Use `select ARRONDISSEMENT,2021-max(ANNEE_PLANTATION)from trees_external group by ARRONDISSEMENT;`  
> 
>![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.6.png)

+ **displays the district that contains the most trees;**  

```
    select 
       ARRONDISSEMENT,count(ESPECE),row_number() over(order by count(ESPECE) desc) 
    from 
       trees_external 
    group by 
        ARRONDISSEMENT
    limit 1;
    
```

> 
>![image.png](https://raw.githubusercontent.com/ye-WANG-Efrei/img_house/main/Framework/Hive_lab/1.3.7.png)  