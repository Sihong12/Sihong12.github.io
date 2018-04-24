---
layout: "post"
title: "Window Function 범위 지정"
redirect_to:
- https://hongsii.github.io/2017/12/12/window-function-range/
description: ""
date: "2017-12-12 01:07"
categories:
- Database
tags:
- Database
- Window function
---

## Window function

보통 테이블에서 특정 값을 집계해야할 때, GROUP BY 절을 이용해 집계를 합니다. <br/>
하지만, GROUP BY 절로 쿼리를 조회하게 되면 결과값의 ROW수가 줄어들며 여러 컬럼을 조회하고 싶을 때에도 제약이 많습니다. <br/>
윈도우 함수를 이용하면 SELECT로 조회된 데이터들을 가지고 집계 함수 사용할 수 있습니다. <br/>

윈도우 함수는 집계 함수 말고도 여러 함수가 존재합니다.
> **RANK, DENSE_RANK, ROW_NUMBER** : 순위 관련 함수 <br/>
> **AVG, SUM, MAX, MIN, COUNT** : 집계 함수 <br/>
> **FIRST_VALUE, LAST_VALUE, LAG, LEAD** : 순서 관련 함수

위에서 나열한 함수 말고도 더 많은 함수가 존재하지만, 빈번하게 사용되는 함수 위주로 적었습니다. <br/>

다음은 윈도우 함수를 사용하는 방법입니다.
> 윈도우함수(args) **OVER** (<br/>
> [ **PARTITION BY** column ] <br/>
> [ **ORDER BY** column [**ASC** | **DESC**] ] <br/>
> [ **[ROWS|RANGE]** **BETWEEN** UNBOUNDED PRECEDING[CURRENT ROW] <br/>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**AND** UNBOUNDED FOLLOWING[CURRENT ROW] ] <br/>
> )

윈도우 함수로 사용 되는 파라미터(args)는 함수마다 다르며 0~N개가 지정될 수 있습니다.<br/>

OVER 괄호 안의 내용은 전부 생략 가능한 내용입니다. <br/>
하나씩 살펴보면 PARTIONTION BY 는 입력한 컬럼별로 프레임을 나누고, 나눠진 프레임별로 윈도우 함수를 적용합니다. <br/>
GROUP BY 절을 이용해 컬럼별로 묶는 것과 같다고 생각하시면 됩니다. <br/>
어떤 함수를 적용하기전 정렬이 필요하다면 ORDER BY 절을 이용하면 됩니다. <br/>

윈도우 함수를 적용할 때, 조회된 모든 데이터가 아닌 **특정 행의 범위를 지정**하고 싶다면 BETWEEN-AND를 사용하면 됩니다. <br/>

> **ROWS** : 물리적인 단위로 행 집합을 지정 <br/>
> **RANGE** : 논리적인 단위로 의해 행 집합을 지정 <br/>
> **BETWEEN ~ AND** : 윈도우의 시작과 끝 위치를 지정 <br/>
> **UNBOUNDED PRECEDING** : 윈도우 시작 위치가 첫 번째 로우임을 의미 <br/>
> **UNBOUNDED FOLLOWING** : 윈도우 마지막 위치가 마지막 로우임을 의미 <br/>
> **[ROW수] PRECEDING** : 윈도우 시작 위치가 ROW수만큼 이전이 시작 로우임을 의미 <br/>
> **[ROW수] FOLLOWING** : 윈도우 마지막 위치가 ROW수만큼 다음이 마지막 로우임을 의미 <br/>
> **CURRENT ROW** : 현재 로우까지를 의미 <br/>

ROWS와 RANGE의 차이는 ROWS는 **조회된 ROW 하나하나를 대상으로 연산**하며, <br/>
RANGE는 ORDER BY 를 통해 정렬된 컬럼에 같은 값이 존재하는 ROW가 여러 개일 경우, **동일한 컬럼값을 가지는 모든 ROW를 묶어서 연산**을 합니다. <br/>

아래는 ROWS를 이용한 예제입니다. <br/>
``` PostgreSQL
SELECT dept, id, salary,
       SUM(salary) OVER (PARTITION BY dept ORDER BY id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) col
  FROM (
          SELECT 20 dept, 100 id, 20000 salary UNION ALL
          SELECT 20 dept, 101 id, 30000 salary UNION ALL
          SELECT 20 dept, 101 id, 10000 salary UNION ALL
          SELECT 20 dept, 102 id,  9000 salary UNION ALL
          SELECT 20 dept, 103 id, 17000 salary
       ) D
```
<p class="text-align-center">ROWS를 사용해 조회한 쿼리 결과</p>

<table>
  <colgroup>
    <col style="width:25%"><col style="width:25%"><col style="width:25%"><col style="width:25%">
  </colgroup>
  <thead>
    <tr>
      <th class="table-header">dept</th><th class="table-header">id</th><th class="table-header">salary</th><th class="table-header">col</th>
    </tr>
  </thead>
  <tbody>
    <tr class="table-align-center"><td>20</td><td>100</td><td>20000</td><td>20000</td></tr>
    <tr class="table-align-center"><td>20</td><td>101</td><td>30000</td><td>50000</td></tr>
    <tr class="table-align-center"><td>20</td><td>101</td><td>10000</td><td>60000</td></tr>
    <tr class="table-align-center"><td>20</td><td>102</td><td>9000</td><td>69000</td></tr>
    <tr class="table-align-center"><td>20</td><td>103</td><td>17000</td><td>86000</td></tr>
  </tbody>
</table>

col에 사용한 ROWS 범위는 첫 번째 로우부터 현재 로우까지의 합을 표시합니다. <br/>
물리적인 ROW로 연산하기 때문에 **ORDER BY 컬럼으로 사용된 id 값이 같아도 col의 내용은 다릅니다**. <br/>

이제 RANGE를 이용한 쿼리와 비교해보겠습니다. <br/>
위에서 사용한 쿼리와 비교했을 때, 바뀐건 ROWS에서 RANGE로 바뀌었을 뿐 입니다.

``` PostgreSQL
SELECT dept, id, salary,
       SUM(salary) OVER (PARTITION BY dept ORDER BY id RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) col2
  FROM (
          SELECT 20 dept, 100 id, 20000 salary UNION ALL
          SELECT 20 dept, 101 id, 30000 salary UNION ALL
          SELECT 20 dept, 101 id, 10000 salary UNION ALL
          SELECT 20 dept, 102 id,  9000 salary UNION ALL
          SELECT 20 dept, 103 id, 17000 salary
       ) D
```
<p class="text-align-center">RANGE로 조회한 쿼리 결과</p>
<table>
  <colgroup>
    <col style="width:25%"><col style="width:25%"><col style="width:25%"><col style="width:25%">
  </colgroup>
  <thead>
    <tr>
      <th class="table-header">dept</th><th class="table-header">id</th><th class="table-header">salary</th><th class="table-header">col</th>
    </tr>
  </thead>
  <tbody>
    <tr class="table-align-center"><td>20</td><td>100</td><td>20000</td><td>20000</td></tr>
    <tr class="table-align-center"><td>20</td><td>101</td><td>30000</td><td>60000</td></tr>
    <tr class="table-align-center"><td>20</td><td>101</td><td>10000</td><td>60000</td></tr>
    <tr class="table-align-center"><td>20</td><td>102</td><td>9000</td><td>69000</td></tr>
    <tr class="table-align-center"><td>20</td><td>103</td><td>17000</td><td>86000</td></tr>
  </tbody>
</table>

RANGE로 조회된 결과를 보면 ORDER BY 컬럼으로 사용된 id 값이 같은 컬럼을 묶어서 연산을 합니다. <br/>
그래서 **id 값이 같은 경우는 col의 결과 값이 동일**하게 나오는 것을 알 수 있습니다.
