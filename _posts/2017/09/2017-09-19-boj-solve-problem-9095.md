---
layout: "post"
title: "9095번 - 1, 2, 3 더하기"
redirect_to:
- https://hongsii.github.io/2017/09/19/boj-solve-problem-9095/
date: "2017-09-19 23:59"
categories:
- 알고리즘
tags:
- DP
- BOJ
- Algorithm
- 알고리즘
---

# BOJ 다이나믹 프로그래밍 알고리즘 풀이
## [9095번 - 1, 2, 3 더하기](https://www.acmicpc.net/problem/9095) <br/>

주어진 정수 N을 1, 2, 3의 합으로 나타낼 수 있는 방법의 수 구하기. (N < 11, N = 양수)


#### 규칙
1. 1, 2, 3을 더해서 주어진 정수가 되게 만들면 된다. <br/>
다음은 해당 문제에 대한 예시. <br/>
![9095번 예시](/assets/images/post_image/2017/09/BOJ_problem_9095_example.png){:height="40%" width="60%" align="left"}


#### 해결
1. N > 3인 경우는 항상 1, 2, 3으로 한 자리를 고정할 수 있다.
2. 주어진 정수가 N이고, 마지막 자리에 올 수 있는 숫자를 1, 2, 3으로 가정하면 1은 N-1, 2는 N-2, 3은 N-3이 된다. <br/>
마지막 자리 수를 뺀 나머지를 만들 수 있는 경우의 수를 다 더하면 N의 총 경우의 수가 된다. (아래 이미지 참고)

![9095번 힌트1](/assets/images/post_image/2017/09/BOJ_problem_9095_solve1.png){:height="40%" width="60%" align="left"}



### [풀이한 코드](https://github.com/Sihong12/Algorithms/blob/master/src/BOJ/DP/Problem_9095.java)

<br/>
올 수 있는 숫자가 제한 되어 있기 때문에 금방 풀 수 있었다.

------------------------
