---
layout: post
title:  "[Python3] Input/Output"
date:   2020-06-13 21:36:00 +0900
categories: algorithm
---

## 개요
> Python3의 입출력에 대해 알아보자

---

## Input(입력)

### 1. input() 함수
Python에서는 표준 입력 함수로 input 함수를 지원한다.

<pre><code> input() </code></pre>

이 함수는 표준 입력장치(키보드)로부터 **문자열**을 입력받는다.
따라서 원하는 자료형으로 변환시키기 위해 다양한 함수를 함께 사용할 수 있다.

```python 
eval() # 인수를 유효한 파이썬 표현식으로 리턴
int() # 정수 객체를 리턴
int(,base=10) # base는 진법을 나타내며 10이 기본값
float() # 실수 객체를 리턴
tuple()
```

### 2. sys.stdin.readline()
종종 알고리즘 문제를 input() 함수로 풀 때 시간초과가 발생하는 경우가 있다. 이럴 때 sys.stdin을 사용한다.
이 메소드는 입력한 한 라인을 interable한 컨테이너에 저장한다. 띄어쓰기와 \n까지 포함하므로 split()을 이용하는 것이 좋다. 


```python
import sys
for x in sys.stdin.readline():  
    print(x)

# input : 12 3
# output :  1
#           2
#
#           3
```

### 3. sys.stdin

```python
import sys
for x in sys.stdin():  
    print(x)

# input : 12 3
# output : 12 3
```

### 4. 자주 등장하는 상황
- 입력받은 값 정수, 리스트로 저장

```python
x = list(map(int, input().split()))

# input : 1 2 3
# output : [1, 2, 3]
```

- 연속적으로 주어지는 입력 값 정수, 리스트로 저장

```python
x = list(map(int, input()))

# input : 123
# output : [1, 2, 3]
```

- 공백 단위 분리해서 저장

```python
x, y = input().split()
```

- ',' 단위로 분리해서 저장

```python
x = list(input().split(','))

# input : 1,2,3
# output : ['1', '2', '3']
```

- 입력이 끝날 때까지 입력받기

```python
while True:
    try:
        # code
    except:
        break
```

```python
import sys
for i in sys.stdin:
    # code
```

## Output(출력)
표준 출력 함수로 print 함수를 사용한다.

## 관련 문제

<p> <img src="/assets/img/algorithm/BOJ1.png" > </p>
출처 : [plzrun's algorithm](https://plzrun.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4PS-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
