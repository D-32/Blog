---
layout: post
title:  "Selecting a Random Record with PHP and MySQL"
date:   2012-08-13 00:00:00
categories: Web
---
Selecting a random record with MySQL can be achieved with one simple select:

	SELECT * FROM `table` ORDER BY RAND() LIMIT 1;

This solution is very slow, especially when the table contains many rows. The thing is that mysql will create a temporary table and asign each row a random number.

So how can we improve that? The best solution I found is to get the row-count of the table, create a random number between 0 and the size – 1 and than use this as offset.
This solution looks like this:
	
	$query = mysql_query("SELECT COUNT(*) FROM `table`;");
	$count = mysql_result($query,0);
	$rand  = mt_rand(0,$count - 1);
	 
	$query = mysql_query("SELECT * FROM `table` LIMIT ".$rand.",1");


So in the end we’ll have two queries, so it’s only faster after a certain table size. I did a small comparison, first with 1’200’000 rows, than 600’000 and 20’000. As you can see in the diagramm at 20’000 rows the execution time was roughly the same. But for bigger tables generating the random number with php is defently worthwhile.

![image]({{ site.baseurl }}/uploads/random_mysql.png)