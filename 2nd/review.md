## 2주차 과제 회고
### 1. NumPy 배열 생산 및 연산
- NumPy를 사용해 3x3 정수배열(1부터 9까지)과 2x5 실수 배열(0~1 사이 난수)을 생성하세요. 생성한 배열을 계산하고 두 배열을 곱한 결과를 출력하세요.
- 배열의 형태를 유지하지 않아도 괜찮습니다.
---
해당 1번 문제는 2 가지가 있다.</br>
1. 곱에 대한 명시가 정확하게 나와있지 않다.</br>
행렬 곱 내적인지, 요소별 곱인지
2. 일반적으로 3x3 행렬과 2x5 행렬은 내적이 불가능하며 요소별 곱 또한 요소가 9개, 10개로 요소별 곱 또한 불가능하다.</br>
</br>

따라서 2가지 Solution을 생각했다.
1. 행렬 내적
   - 행렬 내적을 위해 arr2을 3x4 혹은 4x3으로 만들어 준 다음 내적을 하는 방법을 택한다.
2. 요소별 곱
   - 요소별 곱을 하기 위해 arr1, arr2를 flatten한 다음 arr1에 요소를 하나 추가해 곱한다.
---
1. 행렬 내적
   ```python
   import numpy as np
   import pandas as pd

   arr1 = np.arange(1, 10).reshape(3,3)
   arr2 = np.random.rand(2, 5)

   arr2_c = arr2.copy()
   arr2_c.resize(3, 4)

   print(np.dot(arr1, arr2_c))
   ```
   출력
   ```
   [[ 2.54284851  3.79999731  2.59000382  2.63108628]
    [ 7.13284237  9.38367984  7.43294209  7.52625754]
    [11.72283624 14.96736237 12.27588036 12.42142879]]
   ```
---
2. 요소별 곱
   ```python
   import numpy as np
   import pandas as pd

   arr1 = np.arange(1, 10).reshape(3,3)
   arr2 = np.random.rand(2, 5)

   arr1_c = arr1.flatten().copy()
   arr1_c = np.append(arr1_c, 10)

   print(np.multiply(arr1_c, arr2.flatten()))
   ```
   출력
   ```
   [0.53275297 0.92158079 1.91586507 2.52944489 4.90819703 5.17262655
    6.82983746 7.99490023 0.14045017 5.3833269 ]
   ```
---
- 해당 문제를 풀이할 때, `arr_c`형태를 다시 만들었다. 즉, 기존의 `ndarray`형식을 쓰는게 아니라 `copy()`통해 복사해왔는데, 이는 `resize()`, `flatten()`의 경우 `view`타입이기 때문에 return이 `None`이기 때문이다. 따라서 새로운 `ndarray`를 만들어 계산해야 한다.

---
### 2. Pandas를 활용한 데이터처리
- 아래 데이터를 DataFrame으로 생성하세요.
  ```
  Name: [Alice, Bob, None, Charlie]
  Age: [25, None, 29, 35]
  City: [New york, None, Chicago, None]
  ```
- 누락된 이름은 "Unknown"으로 나이는 평균 나이로 채우고 수정된 DataFrame을 출력하세요.
---
1. 문제 풀이
   ```python
   import pandas as pd

   df = pd.DataFrame({
       'Name': ['Alice', 'Bob', None, 'Charlie'],
       'Age': [25, None, 29, 35],
       'City': ['New york', None, 'Chicago', None]
   })
   df['Name'] = df['Name'].fillna('Unknown')
   df['Age'] = df['Age'].fillna(df['Age'].mean())
   df
   ```
   출력
   ```
   	Name	Age	City
   0	Alice	25.000000	New york
   1	Bob	29.666667	None
   2	Unknown	29.000000	Chicago
   3	Charlie	35.000000	None
   ```
---
- 해당 문제는 `DataFrame`에 `NaN` 혹은 `None`과 같은 결측치를 채우는 것에 대한 문제였다. `fillna()`를 통해 해결이 가능하다.

---
### 3. HTTP 통신을 활용한 데이터 읽기 및 저장
- [해당 링크](https://jsonplaceholder.typicode.com/todos)에서 JSON 데이터를 가져오세요.
- 데이터를 파일로 저장한 뒤, "title"키의 값을 출력하세요.
---
1. 문제 풀이
   ```python
   import pandas as pd

   df = pd.read_json('https://jsonplaceholder.typicode.com/todos')

   df.to_json('assign.json')
   df['title']
   ```
   출력
   ```
   	title
   0	delectus aut autem
   1	quis ut nam facilis et officia qui
   2	fugiat veniam minus
   3	et porro tempora
   4	laboriosam mollitia et enim quasi adipisci qui...
   ...	...
   195	consequuntur aut ut fugit similique
   196	dignissimos quo nobis earum saepe
   197	quis eius est sint explicabo
   198	numquam repellendus a magnam
   199	ipsam aperiam voluptates qui

   200 rows × 1 columns
   ```
---
- 해당 문제는 HTTP통신을 활용한 데이터 읽기 및 저장이다. Pandas는 다양한 형태의 자료를 읽고 쓰는 기능을 지원한다. `.read_*()` 메서드를 이용해 file뿐만 아니라 http주소 자체를 불러 올 수도 있고 `.to_*` 메서드를 이용해 저장도 가능하다.

