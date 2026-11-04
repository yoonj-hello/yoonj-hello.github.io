# [파이썬 기초 7] 함수와 모듈 — 코드를 재사용하는 방법

> 파이썬 입문 가이드 시리즈 7/10
> 함수 정의, 매개변수, 반환값, 람다, `map` / `filter`, 재귀 함수, 그리고 `random` 모듈까지 정리합니다.

---

## 1. 함수란?

**자주 쓰는 코드를 이름 붙여 묶어둔 것**입니다. 한 번 만들어두면 필요할 때마다 호출만 하면 됩니다.

```python
def hap(x, y):          # def 함수이름(매개변수):
    result = x + y
    return result       # 결과를 돌려줌

a = hap(3, 5)           # 호출
print(a)                # 8
```

- **매개변수(parameter)**: 함수가 받는 값 (`x`, `y`)
- **인자(argument)**: 호출할 때 넘겨주는 실제 값 (`3`, `5`)
- **반환값(return value)**: 함수가 돌려주는 결과. `return`이 없으면 `None`

## 2. 매개변수 다루기

### 기본값 매개변수

```python
def func(a, b=10):
    return a + b

print(func(5))       # 15  (b는 기본값 10)
print(func(5, 20))   # 25
```

> 기본값이 있는 매개변수는 **항상 뒤쪽**에 놓아야 합니다.

### 키워드 인자

```python
def intro(name, age):
    print(f"{name}, {age}세")

intro(age=20, name="홍길동")   # 순서 상관없이 이름으로 전달
```

### 여러 값 반환

```python
def min_max(nums):
    return min(nums), max(nums)

lo, hi = min_max([3, 1, 4])
```

## 3. 변수의 범위 (Scope)

```python
x = 10             # 전역 변수

def f():
    y = 5          # 지역 변수 (함수 안에서만 존재)
    print(x, y)    # 전역 변수는 읽을 수 있음

f()
# print(y)         # NameError
```

함수 안에서 **전역 변수의 값을 바꾸려면** `global` 선언이 필요합니다. (8편 tkinter에서 자주 등장)

```python
money = 0
def add():
    global money
    money += 1000
```

## 4. 람다 함수 (lambda)

이름 없는 **한 줄짜리 함수**입니다. `lambda 매개변수: 반환할 식`

```python
f = lambda x: x + 1
print(f(5))        # 6

add = lambda a, b: a + b
print(add(3, 4))   # 7
```

아래 두 함수는 완전히 같습니다.

```python
def hap(x, y):
    return x + y

hap2 = lambda x, y: x + y
```

## 5. `map()` 과 `filter()`

### `map(함수, 반복가능객체)` — 모든 요소에 같은 작업 적용

```python
nums = [1, 2, 3, 4]
print(list(map(lambda x: x ** 2, nums)))     # [1, 4, 9, 16]

data = ["1", "2", "3"]
print(list(map(int, data)))                  # [1, 2, 3]  문자열 → 정수 일괄 변환

def add10(x):
    return x + 10
print(list(map(add10, [1, 2, 3])))           # [11, 12, 13]

# 두 리스트를 함께
a = [1, 2, 3]
b = [4, 5, 6]
print(list(map(lambda x, y: x + y, a, b)))   # [5, 7, 9]

# 문자열 메서드도 가능
print(list(map(str.upper, ["hanbit", "plus"])))  # ['HANBIT', 'PLUS']
```

> `map`의 결과는 바로 출력되지 않으므로 **`list()`로 감싸서** 확인합니다.

### `filter(함수, 반복가능객체)` — 조건이 참인 요소만 남김

```python
num = [1, 2, 3, 4, 5]
print(list(filter(lambda x: x % 2 == 0, num)))   # [2, 4]
```

## 6. 재귀 함수

**함수가 자기 자신을 다시 호출**하는 방식입니다. 반드시 **종료 조건(기저 조건)** 이 있어야 합니다.

### 종료 조건이 없다면?

```python
def selfcall():
    print('하', end="")
    selfcall()       # 끝없이 호출 → RecursionError (최대 재귀 깊이 초과)
```

### 카운트다운

```python
def count(num):
    if num >= 1:
        print(num, end=" ")
        count(num - 1)

count(10)    # 10 9 8 7 6 5 4 3 2 1
```

### 1부터 n까지의 합

```python
def recursive_sum(n):
    if n == 1:            # 종료 조건
        return 1
    return n + recursive_sum(n - 1)

print(recursive_sum(100))   # 5050
```

실행 흐름: `recursive_sum(3)` → `3 + recursive_sum(2)` → `3 + (2 + recursive_sum(1))` → `3 + (2 + 1)` = 6

### 각 자리 숫자의 합

```python
def digit_sum(num):
    if num == 0:
        return 0
    return (num % 10) + digit_sum(num // 10)

# digit_sum(1234) = 4 + digit_sum(123)
#                 = 4 + 3 + digit_sum(12)
#                 = 4 + 3 + 2 + digit_sum(1)
#                 = 4 + 3 + 2 + 1 + digit_sum(0)  → 10
print(digit_sum(1234))   # 10
```

### 팩토리얼

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial(5))   # 120
```

## 7. 함수로 계산기 만들기

```python
def calc(x, y, op):
    if op == "+":
        return x + y
    elif op == "-":
        return x - y
    elif op == "*":
        return x * y
    elif op == "/":
        return x / y
    else:
        return None          # 잘못된 기호 처리

op = input("계산입력(+, -, *, /):")
x = int(input("첫 번째 숫자 입력:"))
y = int(input("두 번째 숫자 입력:"))

res = calc(x, y, op)
if res is None:
    print("지원하지 않는 연산입니다.")
else:
    print(f"계산기:{x}{op}{y}={res}")
```

> 💡 `if`만 연속으로 쓰고 `else` 처리가 없으면, 잘못된 기호를 입력했을 때 `result`가 만들어지지 않아 오류가 납니다. 마지막에 `else`를 꼭 챙겨주세요.

---

## 8. 모듈 (Module)

**미리 만들어진 함수/클래스를 모아둔 파일**입니다. `import`로 가져다 씁니다.

```python
import random                     # 모듈 전체
from random import randint        # 특정 함수만
import random as rd               # 별칭
```

### `random` 모듈

```python
import random

print(random.random())            # 0 이상 1 미만 실수
print(random.randint(1, 100))     # 1~100 정수 (100 포함)
print(random.randrange(0, 10, 2)) # 0, 2, 4, 6, 8 중 하나 (10 미포함)

a = ["사과", "딸기", "포도", "오렌지"]
print(random.choice(a))           # 1개 무작위 선택
print(random.sample(a, 2))        # 중복 없이 2개 선택
random.shuffle(a)                 # 리스트를 섞음 (원본 변경)
print(a)
```

### 로또 번호 추첨 (1~45 중 6개)

```python
import random

lotto = []
for i in range(1, 46):
    lotto.append(i)
print(random.sample(lotto, 6))

# 더 간단하게
print(sorted(random.sample(range(1, 46), 6)))
```

### 자주 쓰는 표준 모듈

| 모듈 | 용도 |
|---|---|
| `random` | 난수, 무작위 선택 |
| `math` | 수학 함수 (`sqrt`, `pi`, `ceil` …) |
| `datetime` | 날짜/시간 |
| `os`, `shutil` | 파일/폴더 처리 (9편) |
| `tkinter` | GUI 프로그램 (8편) |

### 나만의 모듈 만들기

`mymath.py` 파일에 함수를 저장하면 다른 파일에서 `import mymath`로 불러올 수 있습니다.

```python
# mymath.py
def hap(x, y):
    return x + y

# main.py (같은 폴더)
import mymath
print(mymath.hap(3, 5))
```

---

## ✅ 핵심 정리

- 함수는 `def 이름(매개변수):` + `return`으로 정의합니다.
- 기본값 매개변수는 뒤쪽에, 전역 변수 수정은 `global`로.
- `lambda`는 한 줄 함수, `map`은 일괄 변환, `filter`는 조건 선별.
- 재귀 함수에는 **반드시 종료 조건**이 필요합니다.
- `import`로 모듈을 가져다 쓰며, `random`은 무작위 처리에 유용합니다.

## 📝 연습문제

1. 정수를 입력받아 소수(prime)인지 판별하는 함수 `is_prime(n)`을 만들어 보세요.
2. `filter`와 `lambda`로 리스트에서 3의 배수만 추출해 보세요.
3. 재귀 함수로 피보나치 수열의 n번째 값을 구해 보세요.

---

**시리즈 목차**: 1.변수와 데이터 · 2.연산자 · 3.조건문 · 4.반복문 · 5.리스트 · 6.튜플/셋/딕셔너리 · **7.함수와 모듈** · 8.윈도우 프로그래밍 · 9.파일 입출력 · 10.객체지향
