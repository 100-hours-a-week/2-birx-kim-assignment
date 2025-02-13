## 3주차 과제 회고
[colab link](https://colab.research.google.com/drive/1Tn7HA2nQNAdTwG4lZwQsQQw9of96e3rE?usp=sharing)
### 1. 데이터 리모델링과 처리
- 주어진 데이터를 `Pandas` `DataFrame`으로 만들고 `groupby`기능을 이용해 Year별 총 Sales를 구하세요.
- 구한 결과를 바탕으로 Year별 총 매출을 `Total_Sales`라는 새로운 컬럼으로 추가한 `DataFrame`을 출력하세요.

    |Year|Quater|Sales|
    |--|--|--|
    |2023|Q1|200|
    |2023|Q2|300|
    |2023|Q3|250|
- 풀이
    ```python
    df = pd.DataFrame({
        "Year" : [2023, 2023, 2023],
        "Quater" : ["Q1", "Q2", "Q3"],
        "Sales" : [200, 300, 250]
    })

    Year_sum = df.groupby("Year").sum()
    df["Total_Sales"] = Year_sum.loc[2023, "Sales"]
    df
    ```
- 출력
    |index|Year|Quater|Sales|Total\_Sales|
    |---|---|---|---|---|
    |0|2023|Q1|200|750|
    |1|2023|Q2|300|750|
    |2|2023|Q3|250|750|
- 회고</br>
    해당 과제는 `groupby`을 이용한 과제였다. 사실 해당 예시 데이터는 2023년 뿐이어서 의미가 있는지 확실한 체감이 불가능하지만 예를 들어 2021 ~ 2024년까지의 자료가 다 있다면 `groupby`의 기능을 확실히 알 수 있을 것이다.
---
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
- 풀이
    ```python
    df = pd.DataFrame(data)
    r_df = df[(df["Age"] >= 30) & (df["Salary"] >= 50000)]
    r_df[["Name", "Age", "Department"]]
    ```
- 출력
    |index|Name|Age|Department|
    |---|---|---|---|
    |1|Bob|32|Finance|
    |2|Charlie|45|IT|
    |4|Eve|40|IT|
- 회고</br>
    해당 내용은 `Pandas`의 `DataFrame`의 필터링 기능을 이용해 정형 데이터를 처리하는 것을 실습할 수 있는 문제였다. 또한 필터링 된 `DataFrame` 중 원하는 Column을 보기 위해서는 Slicing 하듯 입력하면 필요한 Column만 출력할 수 있다.
#### 비정형 데이터 처리
- [API](https://jsonplaceholder.typicode.com/users)에서 JSON 데이터를 가저와 `DataFrame`으로 변환 후 아래 필드를 추출해 새로운 `DataFrame`을 만드세요.

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
- 풀이
    ```python
    df = pd.read_json("https://jsonplaceholder.typicode.com/users")

    new_df = pd.DataFrame({
        "ID" : df["id"],
        "Name" : df["name"],
        "Username" : df["username"],
        "Email" : df["email"],
        "City" : df["address"].apply(lambda x : x["city"]),
        "Company" : df["company"].apply(lambda x : x["name"])
    })

    new_df = new_df[(new_df["City"] == "Lebsackbury") | (new_df["City"] == "Roscoeview")]
    new_df.to_csv("new_df.csv", index=False)
    new_df
    ```
- 출력
    |index|ID|Name|Username|Email|City|Company|
    |---|---|---|---|---|---|---|
    |4|5|Chelsey Dietrich|Kamren|Lucio\_Hettinger@annie\.ca|Roscoeview|Keebler LLC|
    |9|10|Clementina DuBuque|Moriah\.Stanton|Rey\.Padberg@karina\.biz|Lebsackbury|Hoeger LLC|
- 회고</br>
    해당 내용은 비정형 데이터인 `JSON`파일을 이용해 데이터를 필터링하고 Column이름을 바꾸는 내용의 문제였다. 처음에는 데이터 이름을 변환하기 위해서 `df.rename`메서드를 사용하였는데, `City`와 `Company`에서 문제가 생겼다.</br>
    `DataFrame`은 `JSON`의 구조와 다르기 때문에 하위 항목을 `DataFrame`으로 보기 위해서는 여러 방법을 사용해야한다. 본인의 경우 `pd.read_json()`메서드를 이용해 `JSON`파일을 `url`로 바로 불러왔으며 `adress`의 항목과 `company`의 항목에서 다른 데이터는 필요가 없으므로 `lambda`함수를 사용해서 `adress.city`와 `company.name`만 추출해서 새로운 `DataFrame`을 만들었다.</br>
    하지만 공부해 본 결과, 나머지 내용을 살리기 위해서는 중첩된 키를 평탄화하는 방법이 있다. `pd.json_normalize()`메서드를 사용하는 방법이다. `pd.json_normalize()`을 이용해 `adress`를 평탄화 한 결과는 아래와 같다.
    ```python
    df = pd.read_json("https://jsonplaceholder.typicode.com/users")

    df = pd.json_normalize(df["address"])
    df
    ```
    출력
    |index|street|suite|city|zipcode|geo\.lat|geo\.lng|
    |---|---|---|---|---|---|---|
    |0|Kulas Light|Apt\. 556|Gwenborough|92998-3874|-37\.3159|81\.1496|
    |1|Victor Plains|Suite 879|Wisokyburgh|90566-7771|-43\.9509|-34\.4618|
    |2|Douglas Extension|Suite 847|McKenziehaven|59590-4157|-68\.6102|-47\.0653|
    |3|Hoeger Mall|Apt\. 692|South Elvis|53919-4257|29\.4572|-164\.2990|
    |4|Skiles Walks|Suite 351|Roscoeview|33263|-31\.8129|62\.5342|
    |5|Norberto Crossing|Apt\. 950|South Christy|23505-1337|-71\.4197|71\.7478|
    |6|Rex Trail|Suite 280|Howemouth|58804-1099|24\.8918|21\.8984|
    |7|Ellsworth Summit|Suite 729|Aliyaview|45169|-14\.3990|-120\.7677|
    |8|Dayna Park|Suite 449|Bartholomebury|76495-3109|24\.6463|-168\.8889|
    |9|Kattie Turnpike|Suite 198|Lebsackbury|31428-2261|-38\.2386|57\.2232|
---
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
- 풀이
    ```python
    df = pd.DataFrame({
        "Data" : ["2023-01-01", "2023-02-01", "2023-03-01", "2023-04-01", "2023-05-01"],
        "Price" : [100, 120, 130, 125, 140]
    })

    plt.plot(df["Data"], df["Price"])
    plt.xlabel("Data")
    plt.ylabel("Price")
    plt.show()
    ```
- 출력</br>
    ![alt text](/3rd/image/assign3.png)
- 회고
    `Matplotlib`을 사용해 `DataFrame`을 선 그래프로 시각화 하는 것을 실습하였다. 시계열로 된 데이터는 추세가 생기므로 `DataFrame`형식보다는 확실히 그래프로 그리는 것이 이해하는데 더욱 도음이 된다.
---