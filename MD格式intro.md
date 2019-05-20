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
##### Mathematical and logical operators:

![](https://upload-images.jianshu.io/upload_images/11075127-c82767e07c1edd4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/11075127-0886a09a05de2fbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The AND operator displays a record if all conditions separated by AND are TRUE.
The OR operator displays a record if any of conditions separated by OR is TRUE.
The NOT operator displays a record if the condition(s) is NOT TRUE.


#### Database structure/ table descriptions

Below is a list of the most commonly used tables (and views) in our sky survey database and a short description of them. You can click on the table names to read further information. 以下是该巡天数据库中常用的表和视图，以及对它们的简要介绍。你可以点击表名的链接阅读更多信息。

[***PhotoObj***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+PhotoObj+V#&&history=description+PhotoObj+V)：stores information about the images of every object, including run, rerun, camcol, field, ra, dec, magnitudes and object flags. 存储每个目标的图像信息，包括run，rerun，camcol，field，ra，dec，magnitude和object flags等字段。

[***PlateX***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+PlateX+U)： stores information on the aluminum plates that the sky survey uses to take spectra, including their exposure times and reddening information. You will need to find the Plate and MJD in this table to look up an object's spectrum in the Get Spectra tool. 存储巡天中用于拍摄光谱的铝板信息，包括曝光时间和红化信息。你需要在此表中查询Plate和MJD从而在Get Spectra工具中查找目标天体的光谱。


[***SpecObj***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Specobj+U#&&history=description+Specobj+U)：stores information on objects' spectra, including redshifts and spectroscopic classifications. 存储有关目标天体光谱的信息，包括红移和光谱分类。

[***Neighbors***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Neighbors+U#&&history=description+Neighbors+U
)： stores all PhotoObj pairs within 30 arcseconds. 在30角秒（arcsecond）内存储所有PhotoObj对。

[***Photoz***](http://skyserver.sdss.org/dr15/en/help/browser/browser.aspx?cmd=description+Photoz+U#&&history=description+Photoz+U)：stores the photometrically estimated redshifts for all objects in the GalaxyTag view. 存储GalaxyTag视图中所有目标的光度估计红移。


Our sky survey data also contains several subsets of the PhotoObj table. PhotoPrimary contains only the "best" measurements of each object. Generally, it's better to use PhotoPrimary rather than PhotoObj, which contains both good and bad data. Star contains only data for stars, Galaxy contains only data for galaxies, and Unknown contains only data for objects classified as "unknown." These subsets are actually views rather than tables; **you will get familiar with them later in through examples**
我们的巡天数据还包含PhotoObj表的几个子集。 PhotoPrimary仅包含每个对象的“最佳”测量值。通常，最好使用PhotoPrimary而不是PhotoObj因为后者的数据质量不一。 Star仅包含星星的数据，Galaxy仅包含星系数据，而Unknown仅包含被归类为“未知”的对象的数据。这些子集实际上是视图而不是表。**你将通过后面的示例进一步熟悉它们**

#### Sky survey basics

**ra**: right ascension (sky longitude) ;赤经

**dec**: declination (sky latitude) ;赤纬

**g magnitude**: model magnitude in g filter ;在g波段的星等