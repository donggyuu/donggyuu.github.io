---
title: "When Pessimistic Lock Did Not Work as Expected"
excerpt: "Some cases that pessimistic lock woking with some condition"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/when-we-sould-retry-in-rest-template_RestException.png
categories:
  - Development
tags:
  - DB 
last_modified_at: 2020-11-15T08:06:00-05:00
---

## Overview
Check which rows will be locked with index, join and composite key when pessimistic locking.
Should be more more careful in below cases for it locks quite differently as expected.
- ranged index
- non unique value

## Preparing test data
*※ Using InnoDB engine*  
**student_info**
- personal information for student
- unique index : id  

![pesslock_1](/assets/images/pesslock_1.png)  

<br>

**student_score**  
- score of each subject for student
- composite key(複合主キー) : student_number, subject_code
![pesslock_2](/assets/images/pesslock_2.png)  


## Locking with Unique Index
**Lock a row selected by unique index.** On below, student_number(=20200000) is unique value so return only one row. Only this row will be locked.  
> For a unique index with a unique search condition, InnoDB locks only the index record found, not the gap before it.  
https://dev.mysql.com/doc/refman/5.6/en/innodb-locks-set.html
  
![pesslock_3](/assets/images/pesslock_3.png)  

## Locking with Ranged Unique Index
Locks the index range scanned, using gap locks or next-key locks to block insertions by other sessions into the gaps covered by the range. On below, **not only range from 2 to 4 but also 5 is in target of locking.**  
*※ rangeの前(今回だとid=1)もlockingの対象になるケースもあるそうなので検証が必要*  
> For other search conditions, and for non-unique indexes, InnoDB locks the index range scanned, using gap locks or next-key locks to block insertions by other sessions into the gaps covered by the range.  
https://dev.mysql.com/doc/refman/5.6/en/innodb-locks-set.html  

More about gap locks and next-key locks, check this page : https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html

![pesslock_4](/assets/images/pesslock_4.png)  


## Locking with Non Unique Index
MySQL site said
> when non-unique indexes, locks the index range scanned, using gap locks or next-key locks same as ranged unique index.  

But it seems locking all tables(*※ 検証が必要*).  
![pesslock_5](/assets/images/pesslock_5.png)  

## Locking with Unique Index and Join
**Lock a row in all joined tables selected by unique index.** On two pictures below, join 2 tables(student_info, student_score) then select by unique value(student_number=20200001).

**※ Lock the row in student_info**  
![pesslock_6](/assets/images/pesslock_6.png)  

**※ Lock rows in student_score**  
![pesslock_7](/assets/images/pesslock_7.png)  


## Locking with Composite Key
For example, composite key is consist of two column - student_number and subject_code. If only use student_number for locking then all rows selected by student_number will be locked.  
![pesslock_8](/assets/images/pesslock_8.png)  
