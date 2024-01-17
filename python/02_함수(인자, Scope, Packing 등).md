### 오늘 TIL 요약 
-------
`함수` `인자` `LEGB` `Packing` `Unpacking` `sorted()` `.sort()`
-------
**1. 함수를 '정의'할 때 함수가 받을 값 -> 매개변수, 기본 인자 값  
    함수를 '호출'할 때, 실제로 전달되는 값 -> 인자, 키워드 인자**

**2. 위치 인자는 키워드 인자 이후에 등장할 수 없다**
    
    ```python
    def greet(name, age)
        return
    greet(age=35, 'Dave') # positional argument follows keyword argument 
    ``````

**3.파이썬의 이름 검색 규칙은 Local -> Enclosed -> Global -> Built-in 순서대로 이름을 찾아 나간다. 함수 내에서는 바깥 Scope의 변수에 접근 가능하나, 수정은 할 수 없다.** 

**4. 패킹 `*` 여러 개의 인자를 하나의 '튜플'로 묶는 역할**  
    &nbsp;&nbsp;&nbsp;&nbsp;**언패킹 `*` 시퀀스나 반복 가능한 객체를 각각의 요소로 언패킹**  
    &nbsp;&nbsp;&nbsp;&nbsp;**`**`는 '딕셔너리의 키-값 쌍'으로 패킹, 언패킹**

**5. .sort()는 정렬한 값을 반환하는 것이 아니라 단순히 정렬, 정렬 후 정렬된 상태로 계속 유지됨 / sorted() 정렬된 결과를 반환하나, print()까지는 되지 않는다. 실행한 뒤, 정렬된 상태로 유지되지 않음**

-------
### TIL(Today I learned) 날짜
2024.01.17
-------
### 세부내용


## 함수(Functions)
- 특정 작업을 수행하기 위한 재사용 가능한 코드 묶음
- 코드의 중복을 방지할 수 있으며, 재사용성이 높아짐
    
    + 가독성과 유지보수성 향상
```python
# 두 수의 합을 구하는 코드
num1 = 5
num2 = 3

sum_result = num1 + num2
print(sum_result)
#----------------------------------
# 두 수의 합을 구하는 함수
def get_sum(num1, num2):
    return num1 + num2

# 함수 사용하여 결과 출력
num1 = 5
num2 = 3
sum_result = get_sum(num1, num2) # get_sum을 한번만 사용하는 것이 아닌, 여러번 사용가능
print(sum_result)
```
-------
### 인자(argument)

- 함수를 호출할 때, 실제로 전달되는 값
    
    

```python
# 매개변수와 인자 예시

def add_numbers(x, y): # x와 y는 매개변수(parameter)
    result = x + y
    return result

a = 2
b = 3
sum_result = add_numbers(a, b) # a와 b는 인자(argument)
print(sum_result)
```

### Positional Arguments(위치 인자)

- 함수 호출 시 인자의 위치에 따라 전달되는 인자
- 위치인자는 함수 호출 시 반드시 값을 전달해야 한다.

```python
def greet(name, age):
    print(f'안녕하세요, {name}님! {age}살이시군요.')
		# return은 필수가 아님 

# 값이 하나라도 빠지면 실행되지 않는다.
greet('Alice', 25) # 안녕하세요, Alice님! 25살이시군요
```

```python
def greet(name,age):
    print(f'안녕하세요, {name}님! {age}살이시군요.')

greet('Alice') # greet() missing 1 required positinal argument: 'age'
```
- 필수인자 `age` 가 누락되어 있다고 Error발생

`greet(30)` 을 입력하더라도 동일하게 `age`가 누락되었다고 나옴

위치인자이기 때문에 첫번째에 정수를 넣더라도 `name`으로 인식함 

- `greet(30, ‘Bella’)`  입력 → 안녕하세요, 30님! Bella살이시군요 (정상출력)

### **Default Argument Values (기본 인자 값)**

- 함수 정의에서 매개변수에 기본 값을 할당하는 것
- 함수 호출 시 인자를 전달하지 않으면, 기본값이 매개변수에 할당됨

```python
def greet(name, age=30):
    print(f'안녕하세요, {name}님! {age}살이시군요.')

# age에 매개변수를 넣지 않아도 기본 인자 값이 존재하기 때문에 오류 발생 X 		
greet('Bob') # 안녕하세요, Bob님! 30살이시군요.
greet('Charlie', 40) # 안녕하세요, Charlie님! 40살이시군요.
```

<aside>
❗ **인자 구분** **(기본 인자값은 함수 정의할 때 / 키워드 인자는 함수를 호출할 때)**

</aside>

### **Keyword Arguments (키워드 인자)**

- 함수 호출 시 인자의 이름과 함께 값을 전달하는 인자
- 매개변수와 인자를 일치시키지 않고, 특정 매개변수에 값을 할당할 수 있다
- 인자의 순서는 상관없으며, 인자의 이름을 명시하여 전달한다.

<aside>
❗ **위치인자는 키워드 인자 이후에 등장할 수 없다. `greet(age=35, 'Dave')`**

</aside>

```python
def greet(name, age):
	  print(f'안녕하세요, {name}님! {age}살이시군요.')

greet(name='Dave', age=35)  # 안녕하세요, Dave님! 35살이시군요.
greet(age=35, 'Dave')  #  positional argument follows keyword argument 
# 파이썬의 입장에서는 'Dave'의 위치가 어디인지 명확하게 결정할 수 없음
# 구조상 불가능 
```

### **Arbitrary Argument Lists (임의의 인자 목록)**

- 정해지지 않은 개수의 인자를 처리하는 인자 → print()함수도 동일한 개념
- 함수 정의 시 매개변수 앞에 `*` 를 붙여 사용하며, 여러 개의 인자를 tuple로 처리
- tuple → 변경이 불가능한 특성을 가지고 있는 Data types

```python
def calculate_sum(*args):
    print(args)
    total = sum(args)
    print(f'합계: {total}')

	"""
	(1, 2, 3) # 여러 개의 튜플이 아닌, 한 개의 튜플로 나옴 
	합계: 6
	"""
calculate_sum(1, 2, 3)
```

### **Arbitrary Keyword Argument Lists (임의의 키워드 인자 목록)**

- 정해지지 않은 개수의 **키워드** 인자를 처리하는 인자
- 0개 이상의 인자를 받기에 인자가 없는 경우 → `{}` 출력
- 함수 정의 시 매개변수 앞에 `**`를 붙여 사용, 여러 개의 인자를 dictionary로 묶어 처리
- tuple → key-vaule 쌍으로 묶어 저장하는 Data types

```python
def print_info(**kwargs):
    print(kwargs)

# **를 붙여서 딕셔너리로 묶어서 처리 가능함 / *는 튜플 
print_info(name='Eve', age=30) # {'name': 'Eve', 'age': 30}
```

### 함수 인자 권장 작성순서

- 호출 시 인자를 전달하는 과정에서 혼란을 줄일 수 있도록 하기 위함
- 위치 → 기본 → 가변 → 가변 키워드

```python
def func(pos1, pos2, default_arg='default', *args, **kwargs):
# ...

def func1(pos1, pos2, age=30, *args, **kwargs):
    print(pos1, pos2, age, args, kwargs)

func1(1,2,3,4,5) # -> 1 2 3 (4, 5) {} 출력 # 키워드를 입력하지 않았으므로 튜플(args)
func1(1,2,3,a=100, b=200) # -> 1 2 3 () {'a':100, 'b':200}
	# 키워드를 이용해서 입력했으므로 딕셔너리(kwargs)로 저장
```
--------

### LEGB Rule 예시 1

- sum이라는 이름을 global scope에서 사용하게 되면서 기존에 built-in scope에 있던 내장함수 sum을 사용하지 못하게 됨
- sum을 참조 시 LEGB Rule에 따라 global에서 먼저 찾기 때문
```python
print(sum) # <built-in function sum>
print(sum(range(3))) # 3

sum = 5 # 이미 global에 sum이 존재하기 때문에 아래의 sum(range(3))은 built로 올라가지 않는다. 

print(sum) # 5
print(sum(range(3))) # TypeError: 'int' object is not callable
```
```python
#### LEGB Rule 예시 2

a = 1 # global
b = 2 # global 

# 함수 정의 위에 함수 호출(enclosed())을 하면 사용 불가능 
def enclosed():
    a = 10 # local 
    c = 3 # local

    def local(c): # 함수 정의 / 함수는 호출할 때 실행된다. 따라서 c 값은 중요 X
        print(a, b, c) # def local 기준 enclosed 10 / global 2 / local 500 출력

    local(500) # 함수 내에서 위의 local호출, 매개변수가 c이므로 c의 값은 500이 됨 
    print(a, b, c) # def endclosed() 기준 local 10 / global 2 / local 3 출력

enclosed() # 함수 종료되었으므로 a,b,c는 없음 / global로 선언한 a,b 들어감
print(a, b) # 1 2 출력
```
----------
## Packing & Unpacking

### Packing

- 여러 개의 값을 하나의 변수에 묶어서 담는 것
- *args를 통해 한 번 경험해보았음
    
    
    ### 패킹 예시
    
    - 변수에 담긴 값들은 튜플(tuple) 형태로 묶임
        
        ```python
        packed_values = 1, 2, 3, 4, 5
        print(packed_values)  # (1, 2, 3, 4, 5)
        ```
        
    
    ### ‘*’을 활용한 패킹
    
    - `b`는 남은 요소들을 리스트로 패킹하여 할당
        
        ```python
        numbers = [1, 2, 3, 4, 5]
        a, *b, c = numbers
        
        print(a) # 1
        print(b) # [2, 3, 4]
        print(c) # 5
        
        ```
        
    - print 함수에 임의의 가변 인자를 작성할 수 있었던 이유
        
        ```python
        print('hello') # hello
        
        print('you', 'need', 'python') # you need python
        
        ```
        
        https://github.com/ragu6963/TIL/assets/32388270/05db04bd-d01c-4f4c-a193-854e59385d67
        
        print() 함수의 기본 인자 중, `end='\n'` 을 보고 자동으로 Enter해주는 걸 확인 가능
        
        이를 보고 `end=' '` 로 수정하는 것도 가능  
        

### UnPacking

- 패킹된 변수의 값을 개별적인 변수로 분리하여 할당하는 것
    
    ### 언패킹 예시
    
    - 튜플이나 리스트 등의 객체의 요소들을 개별 변수에 할당
        
        ```python
        packed_values = 1, 2, 3, 4, 5
        a, b, c, d, e = packed_values 
        print(a, b, c, d, e)  # 1 2 3 4 5
        
        ```
        
    
    ### ‘*’을 활용한 언패킹
    
    - `*` 는 리스트의 요소를 언패킹
        
        ```python
        names = ['alice', 'jane', 'peter']
        print(*names)  # alice jane peter
        
        ```
        
    
    ### ‘**’을 활용한 언패킹
    
    - `**` 는 딕셔너리의 키-값 쌍을 함수의 키워드 인자로 언패킹
        
        ```python
        def my_function(x, y, z):
            print(x, y, z)
        
        my_dict = {'x': 1, 'y': 2, 'z': 3} # key에 해당하는 'x','y','z'를 없앨 수 있음, 대신 key의 값이 x,y,z와 동일하지 않다면 불가능 (?)
        my_function(**my_dict)  # 1 2 3
        
        ```
        
    - ‘*’
        - 패킹 연산자로 사용될 때, 여러 개의 인자를 하나의 튜플로 묶는 역할
        - 언패킹 연산자로 사용될 때, 시퀀스나 반복 가능한 객체를 각각의 요소로 <br>언패킹하여 함수의 인자로 전달
    - ‘**’
        - 언패킹 연산자로 사용될 때, 딕셔너리의 키-값 쌍을 키워드 인자로 <br>언패킹하여 함수의 인자로 전달하는 역할

### `*`, `**` 패킹 / 언패킹 연산자 정리

- `*`
    - 패킹 연산자로 사용될 때, 여러 개의 인자를 하나의 튜플로 묶는 역할
    - 언패킹 연산자로 사용될 때, 시퀀스나 반복 가능한 객체를 각각의 요소로 언패킹하여 함수의 인자로 전달
- `**`
    - 언패킹 연산자로 사용될 때, 딕셔너리의 키-값 쌍을 키워드 인자로 언패킹하여 함수의 인자로 전달하는 역할

--------
### 정렬 `.sort()`, `sorted()`

- **.sort()**
    - 리스트를 정렬, 기본은 오름차순, `.sort(reverse = True)` 내림차순
    - 정렬한 값을 반환하는 것이 아닌, 단순히 정렬만 하기에 `print(aList.sort())`시 `None` 출력
    - 한번 실행하고 나면 리스트는 **정렬된 상태로 계속 유지됨**

```python
a = [1, 10, 5, 7, 6]
a.sort() # 단순히 정렬만 하는 기능, 값을 반환하지 않으므로 
					# print(a.sort())를 하면 None이 나온다.
print(a) # [1, 5, 6, 7, 10]
```

- **sorted()**
    - 리스트를 정렬, 정렬된 결과를 반환, 기본은 오름차순, `sorted(reverse=True)` 내림차순
    - 정렬된 결과를 반환하긴 함, `print()` 까지는 되지 않는다.  `print(sorted())` 를 해야 출력됨
    - 한번 실행한 뒤, **정렬된 상태로 유지되는 것이 아님**
    
    위의 `.sort()` `sorted()` 괄호 안에 `reverse = True` 를 통해 내림차순으로 설정하는 것을 보아 
    
    해당 함수들은 Default 값으로 `reverse = Fasle` 임을 알 수 있다. 
    
    ```python
    a = [1, 10, 5, 7, 6]
    sorted(a) # 출력결과 없음 / 정렬된 결과를 반환하나, print()까지는 되지 않음
    print(a) # [1, 10, 5, 7, 6]
    print(sorted(a)) # [1, 5, 6, 7, 10]
    ```


