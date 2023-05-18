## 정규표현식(Regular Expression)

### 정규표현식(Regular Expression)의 정의
![RegExp](/image/RegEx.png)   
정규표현식은 일종의 문자를 표현하는 공식으로, 특정 규칙이 있는 문자열 집합을 추출할 때 자주 사용되는 기법이다.  
닷넷 언어, 자바, 파이썬, POSIX C, C++(C++ 이후)에서 표준라이브러리를 통해 제공한다. 

### 정규표현식의 필요성

- 문자열에서 특정한 조건을 표현할 때 매우 간단하게 표현할 수 있다(높은 가독성).
- 특정 검색패턴에 대한 하나 이상의 일치 항목을 검색하여 텍스트에서 정보를 추출하는데 유용하다.
- 다양한 확인/추출을 위한 함수와 세부 설정을 위한 옵션 존재
  


### 정규표현식 용어
- 외우는 것은 현실적으로 불가능하다 판단하여 정리하고 찾아쓰기  
  
| 표현식 | 의미 |
|:---:|:---:|
|^x|문자열의 시작을 표현하며 x문자로 시작됨을 의미한다.|
|x$|문자열의 종료를 표현하며 x 문자로 종료됨을 의미한다.|
|.x|임의의 한 문자의 자리수를 표현하며 문자열이 x로 끝난다는 것을 의미한다.|
|x+|반복을 표현하며 x 문자가 한번 이상 반복됨을 의미한다.|
|x?|존재여부를 표현하며 x 문자가 존재할 수도, 존재하지 않을 수도 있음을 의미한다.|
|X*|반복여부를 표현하며 x 문자가 0번 또는 그 이상 반복됨을 의미한다.|
|x\|y|or 를 표현하며 x 또는 y 문자가 존재함을 의미한다.|
|(x)|그룹을 표현하며 x 를 그룹으로 처리함을 의미한다.|
|(x)(y)|그룹들의 집합을 표현하며 앞에서 부터 순서대로 번호를 부여하여 관리하고 x, y 는 각 그룹의 데이터로 관리된다.|
|(x)(?:y)|그룹들의 집합에 대한 예외를 표현하며 그룹 집합으로 관리되지 않음을 의미한다. |
|x{n}|반복을 표현하며 x 문자가 n번 반복됨을 의미한다.|
|x{n,}|반복을 표현하며 x 문자가 n번 이상 반복됨을 의미한다.|
|x{n,m}|반복을 표현하며 x 문자가 최소 n번 이상 최대 m 번 이하로 반복됨을 의미한다.|


| 표현식 | 의미 |
|:---:|:---:|
|[xy]|	문자 선택을 표현하며 x 와 y 중에 하나를 의미한다.|
|[^xy]|not 을 표현하며  x 및 y 를 제외한 문자를 의미한다.|
|[x-z]|range를 표현하며 x ~ z 사이의 문자를 의미한다.|
|\\^|escape 를 표현하며 ^ 를 문자로 사용함을 의미한다.|

| Flag | 의미 |
|:---:|:---:|
|g|Global 의 표현하며 대상 문자열내에 모든 패턴들을 검색하는 것을 의미한다.|
|i|	Ignore case 를 표현하며 대상 문자열에 대해서 대/소문자를 식별하지 않는 것을 의미한다.|
|m|Multi line을 표현하며 대상 문자열이 다중 라인의 문자열인 경우에도 검색하는 것을 의미한다.|

### 파이썬에서 정규식  
- 파이썬에선 re 라이브러리 사용
- 패턴문자열(raw string) -> ex) re.compile(r'abc')
- re 모듈 함수
  - match(패턴, 문자열, 플래그) : 문자열의 처음부터 시작해서 작성한 패턴이 일치하는지 확인한다.
  - search(패턴, 문자열, 플래그) : match()와 유사하지만 처음부터 일치하지 않아도 됨
  - findall(패턴, 문자열, 플래그) : 문자열 안에 패턴에 맞는 케이스를 전부 찾아서 리스트로 반환한다.
  - finditer(패턴, 문자열, 플래그) : findall()과 유사하지만 패턴에 맞는 문자열의 리스트가 아닌 iterator 객체 형식으로 반환한다.
  - fullmatch(패턴, 문자열, 플래그) : 문자열에 시작과 끝이 정확하게 패턴과 일치할 때 반환한다.
  - split(패턴, 문자열, 최대 split 수, 플래그) : 문자열에서 패턴이 맞으면 이를 기점으로 리스트로 쪼개는 함수. 3번째 인자를 지정하면 지정한 수 만큼 쪼갠다.
  - sub(패턴, 교체할 문자열, 문자열, 최대 교체 수, 플래그) : 문자열에 맞는 패턴을 2번째 인자(교체할 문자열)로 교체한다.
  - subn(패턴, 교체할 문자열, 문자열, 최대 교체 수, 플래그) : sub()와 동일하지만 반환 결과가 (문자열, 매칭횟수)형태로 반환된다.
  - compile(패턴, 플래그) : 패턴과 플래그가 동일한 정규식을 여러번 사용할 때 사용

### 여러가지 예제 파이썬 코드


* 패스워드 유효성 검사
  - 총 길이 8글자 이상
  - 1개 이상의 숫자, 영문 대문자, 영문 소문자 포함
  - !@#$%^&*()문자 중 1개 이상 포함

- isPassword 함수 정의
```python
import re

# 패스워드 유효성 검사
def isPassword(pw):
  if len(pw) < 8:
    return False
  regs = (r'[0-9]+', r'[a-z]+', r'[A-Z]+', r'[~!@#$%^&*]+')
  for reg in regs:
    if not re.search(reg, pw):
      return False
  return True
```

- 유효성 검사 테스트  
![result](/image/실행결과1.png)  
* 휴대폰번호 유효성 검사
  *  3개의 그룹 '-'로 구분
  *  첫 번째 그룹 010, 011, 012, 016, 017, 018, 019 중 하나
  *  두 번째, 세 번째 그룹 4글자

- isPhoneNumber 함수 정의
```python
import re

# 패스워드 유효성 검사
def isPhoneNumber(num):
  if re.search(r'^01[0-26-9]-[0-9]{4}-[0-9]{4}$', num):
    return True
  return False
  ```

- 유효성 검사 테스트  
![result2](/image/실행결과2.png)  

- 휴대폰번호만 추출하기
  - 앞과 규칙은 동일  

ex) 'phone 010-2384-1241 call'과 같은 문자열에서 휴대폰 번호만 추출

```python
import re

msg = 'phone 010-1234-5678 call 011-1234-1234'

r = re.finditer(r'01[0-26-9]-[0-9]{4}-[0-9]{4}', msg)
for idx, m in enumerate(r, start=1):
  print(f'Phone number{idx}:{m.group()}')
```

- 실행결과  
![result3](/image/실행결과3.png) 

- character
```python
import re
# 첫 번째 글자 숫자, 두 번째 글자 a/b/c 중 한 글자, 세 번째 글자 c~z 사이 글자가 아닌 글자
mc = re.search(r'[0-9][abc][^c-z]', '123abcd')
#  True
```

- 횟수 관련 메타 문자
```python
import re
# 숫자가 0회 이상 사용된 패턴 찾기
mc = re.search(r'[0-9]*','123')

# 알파벳 소문자가 1회 이상 사용된 패턴 찾기
mc = re.search(r'[a-z]+','ab')

# 알파벳 대문자가 없거나 1회 사용된 패턴 찾기
mc = re.search(r'[A-Z]?','')

# 숫자가 3회 연속 사용된 패턴 찾기
mc = re.search(r'[0-9]{3}','abc1234de')

# 숫자가 2~4회 연속 사용된 패턴 찾기
mc = re.search(r'[0-9]{2, 4}','1234')

# 숫자가 3회 이상 연속 사용된 패턴 찾기
mc = re.search(r'[0-9]{3,}','12345')

# 숫자가 3회 이하 연속 사용된 패턴 찾기(0도 포함)
mc = re.search(r'[A-Z]?','12345')
```

- 그 외 메타문자
```python
# a로 시작하고 o로 끝나는데 사이에 2개의 문자가 있는 패턴 검색
mc = re.search(r'a..o','a12o')

# \n이 포함된 패턴을 검색하고 싶을 때
mc = re.search('.abc', '\nabc', re.S)

# 1개 이상의 알파벳 소문자로 시작되는 패턴에서 '알파벳 소문자' 검색
mc = re.search(r'^[a-z]+','a12o')

# 1개 이상의 숫자로 끝나는 패턴에서 '숫자' 검색
mc = re.search(r'[0-9]+$','abc123')

# abc 또는 123 패턴 찾기
mc = re.search(r'abc|123','-1234-')
```

- 그룹 관련 메타 문자

```python
import re
# 1. 숫자 그룹 => 숫자 3글자
# 2. 구분 기호 그룹 => : 또는 - 또는 공백 문자가 있을 수 있으며, 그룹 지정
# 3. 그룹번호 부여하지 않는 숫자 그룹 => 숫자 3글자 또는 4글자 
# 4. 2번에서 사용된 구분 기호 => 첫 번째 구분 기호와 동일한 것을 사용함
# 5. 이름을 pnum으로 갖는 숫자 그룹 => 숫자 4글자
pattern = re.compile(r'([0-9]{3})(:|-| )(?:[0-9]{3,4})\2(?P<pnum>[0-9]{4})')
mc = re.search(pattern, '010-123-4567')
if mc:
    print('{}~{}에 {}존재'.format(mc.start(), mc.end(), mc.group()))
    for x in mc.groups():
        print(x)
    print(mc.group(3), mc.group('pnum'))
else:
    print('문자열에 패턴이 존재하지 않음')

# 0~12에 010-123-4567존재
# 010
# -
# 4567
# 4567 4567
```

- Special Sequences
  - \d : 숫자 한 개 == [0-9]
  - \D : \d가 아닌 문자 한 개 == [^0-9]
  - \w : 숫자 + 문자(알파벳, 한글 등) + _ == [가-힣a-zA-Z0-9_] (이름 작성에 사용 가능한 문자)
  - \W : \w가 아닌 문자 == [^가-힣a-zA-Z0-9_]
  - \s : 화이트스페이스 문자 == [ \t\n\r\f\v] (space 포함)
  - \S : \s가 아닌 문자 == [^ \t\n\r\f\v]
  - \^ : 메타문자 '^'를 일반 문자로 취급
  - \A : 문자열의 시작부터 매치되는 문자열을 찾음
  - \Z : 문자열의 끝부터 매치되는 문자열을 찾음
  - \A, \Z는 여러 줄 문자열도 1개의 문자열로 취급함

  ^ $ : 여러 줄 문자열인 경우 옵션에 따라서 여러 줄을 각 줄로 나누어서 사용, 하나의 문자열로 사용  
  \A, \Z : 여러 줄 문자열인 경우 항상 하나의 문자열로 사용

```python
# 숫자 1개 + 이름 작성에 사용 가능 문자 1개 + 이름 작성에 사용 불가한 문자 1개 +
# 화이트스페이스문자(=공백문자) 1개 + 숫자 아닌 문자 1개
pattern = '\d\w\W\s\D'
```

```python
# 숫자1글자 이상 + (공백문자 1개 있거나 없거나) + ^ + (공백문자 0개 이상) + 숫자 1글자 이상 검색
pattern = '\d+\s?\^\s*\d+'
```

```python
# 숫자1글자 이상 + (공백문자 1개 있거나 없거나) + ^ + (공백문자 0개 이상) + 숫자 1글자 이상 검색
pattern = '\d+\s?\^\s*\d+'
```

```python
# 숫자1글자 이상 + (공백문자 1개 있거나 없거나) + ^ + (공백문자 0개 이상) + 숫자 1글자 이상 검색
pattern = '\d+\s?\^\s*\d+'
```

- \b : boundry - \w와 \W의 사이를 의미함(단어의 경계)
- \B : boundry가 아닌 것
- \b, \B는 패턴의 맨 앞, 뒤에 사용함
- \b, \B는 \w(워드문자)가 아닌 문자를 구분문자로 취급함

```python
# pi가 포함된 모든 문자열 : r'pi'
# 단어의 시작이 pi인 것 : r'\bpi'
```

Ex)  
\bp\w+ed -> 경계 + p + 워드문자 1개 이상 + ed  
\np\W+ed\b -> 경계 + p + 워드문자 1개 이상 + ed + 경계

- 탐색함수
```python
# findall
import re

ret = re.findall(r'wow', 'wowowowow')
print(type(ret))
for t in ret:
    print(f"Matched : {t}")

# <class 'list'>
# Matched : wow
# Matched : wow
```

```python
# finditer
import re

ret = re.finditer(r'wow', 'wowowowow')
print(type(ret))
for t in ret:
    print(f"Matched : {t}")

# <class 'callable_iterator'>
# Matched : <re.Match object; span=(0, 3), match='wow'>
# Matched : <re.Match object; span=(4, 7), match='wow'>
```

```python
# search
import re

pattern = re.compile(r'[a-z]{2}')
ret1 = pattern.search('123abc123')
ret2 = pattern.search('abcXde')
if ret1 : print(f'Matched ret1 : {ret1.group()}')
if ret2 : print(f'Matched ret2 : {ret2.group()}')

# Matched ret2 : ab
# Matched ret2 : ab
```

```python
# match
import re

pattern = re.compile(r'[a-z]{2}')
ret1 = pattern.match('123abc123')
ret2 = pattern.match('abcXde')
if ret1 : print(f'Matched ret1 : {ret1.group()}') #None
if ret2 : print(f'Matched ret2 : {ret2.group()}')

# Matched ret2 : ab
```

```python
# fullmatch
import re

pattern = re.compile(r'[a-z]{2}')
ret1 = pattern.fullmatch('abcd')
ret2 = pattern.fullmatch('ab')
if ret1 : print(f'Matched ret1 : {ret1.group()}')
if ret2 : print(f'Matched ret2 : {ret2.group()}')

# Matched ret2 : ab
```

```python
# 숫자 1개 이상으로 이루어진 문자열 (숫자만으로 이루어진 문자열)
import re

data = ('1', '12', '1234', '12ab34', '12 34')
for x in data :
    r = re.fullmatch(r'\d+', x)
    print(r)

# <re.Match object; span=(0, 1), match='1'>
# <re.Match object; span=(0, 2), match='12'>
# <re.Match object; span=(0, 4), match='1234'>
# None
# None
```

- 치환/분리 함수
```python
# 연속된 알파벳 소문자 1개 이상을 X로 치환
import re

ret = re.sub('[a-z]+', 'X',  'abc1de2fg3hij')
print(ret)

# X1X2X3X
```

```python
# 숫자를 구분자로 하여 문자열 분리
import re

ret = re.split(r'\d', 'abc1de2fg3hij')
print(ret)

# ['abc', 'de', 'fg', 'hij']
```

- 최종 패턴 활용하기
  - 0보다 큰 순수 양수만을 검색하는 패턴  
  -> r'[1-9]\d*$'
  - 년도는 4글자, 월은 01 ~ 12, 일은 01 ~ 31, '-'을 사용하여 구분하는 날짜형식 찾기  
  -> r'(19|20)\d{2}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$'
  - 메일 주소 찾기  
  -> r'\w+[\.\w]*@\w+(\.\w+){1,2}'


예제코드 출처 및 공부 자료 : https://www.youtube.com/watch?v=UD9jqWeaRkg&list=PLnp1rUgG4UVbGVR1QPhR-tGGGne2xWAuB
