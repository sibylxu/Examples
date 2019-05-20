**Welcome! SQL is the Structured Query Language, a standard means of communicating with a database. SQL statements are used to perform tasks such as update data on a database, or retrieve data from a database. It is widely used to query sky survey data in astronomy research. This guide provides a brief overview of SQL and query examples are available with comments.**

**欢迎你学习使用巡天数据库中的SQL语言！SQL即结构化查询语言，是与数据库通信的标准方法。SQL语句用于执行更新数据库中的数据或从数据库检索数据等任务。在天文学研究中，它被广泛应用于巡天数据的查询和操作。本手册包含了SQL的简要介绍，你还可以学习一些包含注释的具体查询示例。**
-----
**Table of content**
## SQL Basics SQL基础知识
#### Database structure/ table descriptions 数据库与表的结构
#### Sky survey basics 巡天数据库常用字段
## Query Examples 查询示例
------
## SQL Basics

The data in the database is organized in Tables, organized in columns and rows. In SQL you write queries to the database. A query is compound of the table columns you want to retrieve (the SELECT part), the table or tables that store the data (the FROM part) and the conditions to restrict the data you will obtain (the WHERE part). 
数据库中的数据在**表（table）** 中按照**行(row)** 和**列(column)** 组织起来。通过SQL你通过**查询语句（query）** 从数据库中获取数据。查询语句包括要检索的表和列（SELECT子句）、存储数据的表（FROM子句）以及查询数据的限制条件（WHERE子句）。
E.g.
```
 SELECT <columns> FROM <tables> WHERE <conditions>
```
##### Mathematical and logical operators 数学与逻辑运算符

![](https://upload-images.jianshu.io/upload_images/11075127-c82767e07c1edd4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/11075127-0886a09a05de2fbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The AND operator displays a record if all conditions separated by AND are TRUE.
The OR operator displays a record if any of conditions separated by OR is TRUE.
The NOT operator displays a record if the condition(s) is NOT TRUE.
##### SQL Comments 注释
Comments are used to explain sections of SQL statements, or to prevent execution of SQL statements.
Single line comments start with -- .Any content between -- and the end of the line will be ignored (will not be executed).
注释用于解释SQL语句的各个部分，或使SQL语句不能运行。
单行注释以--开始，在“--”和行尾中间的所有内容都不会被运行。
E.g.
```
 --Select all:
SELECT * FROM Stars;
```

#### Database structure/ table descriptions

Below is a list of the most commonly used tables (and views) in our sky survey database and a short description of them. You can click on the table names to read further information. 以下是该巡天数据库中常用的表和视图，以及对它们的简要介绍。你可以点击表名的链接阅读更多信息。

[***PhotoObj***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+PhotoObj+V#&&history=description+PhotoObj+V)：stores information about the images of every object, including run, rerun, camcol, field, ra, dec, magnitudes and object flags. 存储每个目标的图像信息，包括run，rerun，camcol，field，ra，dec，magnitude和object flags等字段。

[***PlateX***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+PlateX+U)： stores information on the aluminum plates that the sky survey uses to take spectra, including their exposure times and reddening information. You will need to find the Plate and MJD in this table to look up an object's spectrum in the Get Spectra tool. 存储巡天中用于拍摄光谱的铝板信息，包括曝光时间和红化信息。你需要在此表中查询Plate和MJD从而在Get Spectra工具中查找目标天体的光谱。


[***SpecObj***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Specobj+U#&&history=description+Specobj+U)：stores information on objects' spectra, including redshifts and spectroscopic classifications. 存储有关目标天体光谱的信息，包括红移和光谱分类。

[***Neighbors***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Neighbors+U#&&history=description+Neighbors+U
)： stores all PhotoObj pairs within 30 arcseconds. 在30角秒（arcsecond）内存储所有PhotoObj对。

[***Photoz***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Photoz+U#&&history=description+Photoz+U)：stores the photometrically estimated redshifts for all objects in the GalaxyTag view. 存储GalaxyTag视图中所有目标的光度估计红移。


Our sky survey data also contains several subsets of the PhotoObj table. [***PhotoPrimary***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+PhotoPrimary+V#&&history=description+PhotoPrimary+V) contains only the "best" measurements of each object. Generally, it's better to use PhotoPrimary rather than PhotoObj, which contains both good and bad data. [***Star***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Star+V) contains only data for stars, [***Galaxy***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Galaxy+V#&&history=description+Galaxy+V) contains only data for galaxies, and Unknown contains only data for objects classified as "unknown." These subsets are actually views rather than tables; **you will get familiar with them later in through examples**
我们的巡天数据还包含PhotoObj表的几个子集。 PhotoPrimary仅包含每个对象的“最佳”测量值。通常，最好使用PhotoPrimary而不是PhotoObj因为后者的数据质量不一。 Star仅包含星星的数据，Galaxy仅包含星系数据，而Unknown仅包含被归类为“未知”的对象的数据。这些子集实际上是视图而不是表。**你将通过后面的示例进一步熟悉它们**

#### Sky survey basics

**ra**: right ascension (sky longitude) ;赤经

**dec**: declination (sky latitude) ;赤纬

**g magnitude**: model magnitude in g filter ;在g波段的星等

## Query Examples


**Example 1**
-- Find unique objects in an RA/Dec box. 查询一个赤经/赤纬坐标区域内具有唯一性的目标。
--Table used: PhotoObj (PhotoPrimary is a view created based on PhotoObj)

```sql
SELECT TOP 100 objID,  	    
-- to retrieve only the first 100 object ID  
-- the SELECT TOP clause is used to specify the number of records to return 
    field, ra, dec      	-- to get the field number and coordinates 
FROM PhotoPrimary       	
-- to select from PhotoPrimary which contains only unique objects in PhotoObj  
-- selecting from a view which contains only unique objects can avoid duplicates in the result 
WHERE
    ra > 185. and ra < 185.1 and dec > 15. and dec < 15.1
   
```

**Example 2**
-- Find all galaxies in a certain area of the sky that meet specific criteria in their measured parameters.  查询天区中参数符合特定判据的所有星系。
-- Table used: Galaxy
```sql
SELECT TOP 100 objID, ra, dec, cModelMag_g 
-- to get first 100 objectID, coordinates and g magnitude   --The cModelMag_g column contains “model magnitude in g filter” 
FROM Galaxy
WHERE ra BETWEEN 178 AND 180 
-- to select objects of which right ascension ranges between 178 and 180  --The BETWEEN operator selects values within a given range. 
 AND dec < 0
 AND cModelMag_g BETWEEN 18 AND 19 
 -- to select objects of which g magnitude ranges between 18 and 19


```

**Example 3**
--Look for spectra of quasars and the date and time at which each spectrum was taken.  查询类星体的光谱，并显示每个光谱的拍摄日期和时间。
-- Table used: SpecPhoto (a view which is a pre-computed join of the most commonly-searched fields in both the SpecObj view that contains spectroscopy data and the PhotoObj view that contains photometry data)
```sql
SELECT TOP 100
sp.objID, sp.ra, sp.dec,
px.taiBegin, px.taiEnd,    -- to select beginning time and ending time 
FROM specPhoto AS sp     
-- to give specPhoto an alias (nickname) as sp in this query  -- aliases (names) are used to give a table, or a column a temporary name
JOIN plateX AS px          -- to give plateX an alias (nickname)as px in this query 
ON sp.plateID = px.plateID  
-- to use JOIN…ON… to select objects that stored in both sp and px 
-- a JOIN clause is used to combine rows from two or more tables, based on key variable they have in common 
WHERE
(sp.class='QSO')           -- to select QSO from spectroscopic class (GALAXY, QSO, or STAR) 
           
```

**Example 4**
-- Count the number of spectra of each spectral classification (galaxy, quasar, star). 查询每种光谱分类（星系，类星体，恒星）的光谱数。
-- Table used: SpecObj
```sql
SELECT class, count(*)   
-- to retrieve class and the total number of records  
-- the count(*) statement returns the number of all records that meet specific search criteria 
FROM SpecObj 
GROUP BY class  
-- to group the number of records based on 'class' column which contains the spectral classification of the object  
-- The GROUP BY statement groups results into groups (categories) based on the value of a data column. 
  
```

**Example 5**
-- Find all objects within 30 arcseconds of one another that have very similar colors.查询彼此角距离小于30角秒且颜色相近的所有目标。
-- Table used: PhotoPrimary, Neighbors

```sql
SELECT TOP 10 P.ObjID	                               
FROM	PhotoPrimary AS P	                         -- to select primary objects from P
JOIN Neighbors AS N ON P.ObjID = N.ObjID           
JOIN PhotoPrimary AS L ON L.ObjID = N.NeighborObjID   -- to select lens candidate from L  
WHERE	
P.ObjID < L. ObjID                                    -- to avoid duplicates 
-- to select objects from L and P that have similar spectra, which means the color ratios u-g, g-r, r-I are less than 0.05m   
-- if two objects’ color ratios u-g, g-r, r-I are less than 0.05m, they have a similar spectra
and abs((P.u-P.g)-(L.u-L.g))<0.05         
and abs((P.g-P.r)-(L.g-L.r))<0.05      
and abs((P.r-P.i)-(L.r-L.i))<0.05
and abs((P.i-P.z)-(L.i-L.z))<0.05        -- The ABS() function returns the absolute value of a number.
  
```

**Example 6**
-- List the number of each type of object observed by each special spectroscopic observation program. 查询每个光谱观测项目观测到的每种目标的数量。
-- Table used: SpecObjAll, PlateX
```sql
SELECT plate.programname, class,
COUNT(specObjId) AS numObjs     
-- to count objects and name the result column as numObjs  
-- The COUNT() function returns the number of rows that matches a specified criteria. 
FROM SpecObjAll
JOIN PlateX AS plate ON plate.plate = specObjAll.plate
GROUP BY plate.programname, class
-- to categorize results based on program and class  
-- The GROUP BY statement sorts results into groups (categories) based on the value of a data column. 
ORDER BY plate.programname, class       
-- to sort the results based on program and in each program sort results based on class  
-- the ORDER BY keyword is used to sort the result-set in ascending or descending order 
     
 ```

