---
layout: "post"
title: "SQL - COUNT(*), COUNT(column)의 차이"
redirect_to:
- https://hongsii.github.io/2017/09/20/count-function-in-sql/
date: "2017-09-20 23:28"
categories:
- Database
tags:
- SQL
- COUNT
---

# SQL
<br/>
## COUNT()
내가 생각하던 COUNT함수는 ROW의 수만 세는 줄 알았는데 생각보다 다양하게 사용할 수 있는 방법이 많았다. <br/>
COUNT(*)과 COUNT(column)을 사용했을 때, 차이점을 새롭게 알게되어서 정리하려고 한다.<br/>

> **COUNT(*)** <br/>
> \: 테이블의 모든 ROW의 수를 센다.<br/><br/>
> **COUNT(column)** <br/>
> \: 해당 COLUMN이 NOT NULL인 ROW의 수만 센다.<br/><br/>
> **COUNT(DISTINCT column)** <br/>
> \: 해당 COLUMN의 중복을 제거하고 NOT NULL인 ROW의 수를 센다.<br/><br/>
> **COUNT(DISTINCT CASE WHEN condition THEN result)** <br/>
> \: 조건에 해당하는 COLUMN의 NOT NULL인 ROW의 수를 센다.<br/>
