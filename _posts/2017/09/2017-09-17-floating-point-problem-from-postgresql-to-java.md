---
layout: "post"
title: "부동소수점 표현 문제"
redirect_to:
- https://hongsii.github.io/2017/09/17/floating-point-problem-from-postgresql-to-java/
date: "2017-09-17 23:50"
categories:
- Programming
tags:
- PostgreSQL
- 부동소수점
- Numeric
- Double
- Java
- Database
---

# 부동소수점 표현 문제
PostgreSQL에서 double형을 Java로 넘겨줄 때, 아래와 같은 문제가 발생했다.

> 1.03 - 0.42 ==> output : 0.6100000000000001

PostgreSQL에서는 정확한 0.61로 표현이 되었으나, Java에서만 해당 문제가 계속 발생했다. <br/>
반올림, 올림을 해도 동일했다. 이유는 double형은 **부동소수점을 이용하여 근사치를 표현**하기 때문에 정확한 연산을 할 수 없다고 한다. <br/> 정확하게 소수점을 표현하고 싶다면 PostgreSQL의 `numeric`으로 형변환을 하거나, Java에서 `BigDecimal`로 받아야 한다.

PostgreSQL의 doc에서도 해당 내용을 확인할 수 있다.



Name    | Storage Size | Description                        | Range
--------|--------------|------------------------------------|-----------------------------------------------------------------------------------------
double precision    | 8 bytes	| variable-precision, inexact | 15 decimal digits precision
decimal | variable     | user-specified precision, exact    | up to 131072 digits before the decimal point ; up to 16383 digits after the
numeric | variable     | user-specified precision, exact    | up to 131072 digits before the decimal point ; up to 16383 digits after the decimal point

--------------------------------
<br/>
### 참고문서
* [PostgreSQL - Numeric Types](https://www.postgresql.org/docs/9.3/static/datatype-numeric.html)
* Effective Java 2/E - P294-296
