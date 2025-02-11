## 3주차 메인 과제
### 1. 데이터 리모델링과 처리
- 주어진 데이터를 `Pandas` `DataFrame`으로 만들고 `groupby`기능을 이용해 Year별 총 Sales를 구하세요.
- 구한 결과를 바탕으로 Year별 총 매출을 `Total_Sales`라는 새로운 컬럼으로 추가한 `DataFrame`을 출력하세요.

    |Year|Quater|Sales|
    |--|--|--|
    |2023|Q1|200|
    |2023|Q2|300|
    |2023|Q3|250|

### 2. 정형 데이터와 비정형 데이터 처리
#### 정형 데이터 처리
- 주어진 데이터를 `DataFrame`으로 만들고 Age가 30세 이상(`>= 30`), Salary가 5만 이상(`>= 50000`)인 직원만 필터링한 `DataFrame`을 만드세요.
- 필터링 된 결과에서 직원의 Name, Age, Department 컬럼만 출력하세요. (또는 필요한 컬럼만)
    ```python
    data = {
        "ID" : [1, 2, 3, 4, 5],
        "Name" : ["Alice", "Bob", "Charlie", "David", "Eve"],
        "Age" : [25, 32, 45, 29, 40],
        "Department" : ["HR", "Finance", "IT", "Marketing", "IT"],
        "Salary" : [48000, 52000, 60000, 45000, 70000]
    }
    ```
#### 비정형 데이터 처리
- API에서 JSON 데이터를 가저와 `DataFrame`으로 변환 후 아래 필드를 추출해 새로운 `DataFrame`을 만드세요.

    |필드|변환 내용|
    |--|--|
    |`id`|ID|
    |`name`|Name|
    |`username`|Username|
    |`email`|Email|
    |`adress.city`|City|
    |`company.name`|Company|

- `City`가 `"Lebsackbury"` 또는 `"Roscoeview"`에 해당하는 사용자만 필터링하세요.
- 필터링 된 `DataFrame`을 CSV파일로 저장하세요.
- [API](https://jsonplaceholder.typicode.com/users)

### 3. 시각화 및 시계열 데이터 활용
- 아래 데이터를 `Pandas`와 `Matplotlib`를 사용해 시계열 그래프로 시각화하세요.
- X축은 날짜, Y축은 가격으로 설정하고 가격의 추세를 선 그래프로 나타내세요.

    |Data|Price|
    |--|--|
    |2023-01-01|100|
    |2023-02-01|120|
    |2023-03-01|130|
    |2023-04-01|125|
    |2023-05-01|140|