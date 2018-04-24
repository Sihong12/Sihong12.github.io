---
layout: "post"
title: "1149번 - RGB거리"
redirect_to:
- https://hongsii.github.io/2017/09/18/boj-solve-problem-1149/
date: "2017-09-18 23:17"
categories:
- 알고리즘
tags:
- DP
- BOJ
- Algorithm
- 알고리즘
---

# BOJ 다이나믹 프로그래밍 알고리즘 풀이
## [1149번 - RGB거리](https://www.acmicpc.net/problem/1149) <br/>

모든 집을 칠할 때 드는 비용의 최소값 구하기. <br/>
입력은 첫 째줄에 집의 개수(N)와 다음 줄부터 색깔별 비용이 주어진다. (N <= 1000) <br/>

#### 규칙
1. RGB거리에 사는 사람들은 집을 빨강, 초록, 파랑중에 하나로 색칠.
2. 그들은 모든 이웃은 같은 색으로 칠할 수 없다.
3. 집 i의 이웃은 집 i-1과 집 i+1이다. 처음 집과 마지막 집은 이웃이 아니다.

#### 해결
1. 색깔별로 모든 집을 칠할 때의 최소가 되는 경우의 수를 구한다.
2. 구한 경우의 수 중에 가장 작은 값을 선택한다.

### [풀이한 코드](https://github.com/Sihong12/Algorithms/blob/master/src/BOJ/DP/Problem_1149.java)

위 코드에서 min 메소드를 정의했지만, Math 클래스의 min 함수를 쓰는게 더 나을거 같다.
