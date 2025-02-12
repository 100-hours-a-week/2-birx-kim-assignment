## Seaborn Mini-Quest
[Colab link](https://colab.research.google.com/drive/1M3JL3OloCvK1Lz0oP323eyXiuIpXgag2?usp=sharing)</br>
1. [범주형 데이터(Categorical Data)](#1-범주형-데이터categorical-data)
2. [연속형 데이터(Continuous Data)](#2-연속형-데이터continuous-data)
3. [관계형 데이터(Relational Data)](#3-관계형-데이터relational-data)
### 1. 범주형 데이터(Categorical Data)
1. 샘플 데이터를 직접 생성한 후 `Seaborn`을 활용하여 막대 그래프(Bar Plot)를 생성하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = pd.DataFrame({
        "카테고리": ["X", "X", "Y", "Y", "Z", "Z", "Z", "X", "Y", "Z"],
        "값": [5, 9, 4, 6, 12, 10, 14, 7, 5, 18]
    })
    ```
    풀이
    ```python
    sns.barplot(x="카테고리", y="값", data=data)
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn1-1.png)
2. `Seaborn`의 `sns.boxplot()`을 활용하여 범주형 데이터의 분포를 시각화하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = pd.DataFrame({
        "group": ["A", "A", "B", "B", "C", "C", "C", "A", "B", "C"],
        "score": [65, 70, 55, 60, 90, 85, 95, 72, 58, 88]
    })
    ```
    풀이
    ```python
    sns.boxplot(x="group", y="score", data=data)
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn1-2.png)
3. Seaborn의 `sns.violinplot()`과 `sns.stripplot()`을 함께 사용하여 범주형 데이터의 분포를 더욱 자세히 시각화하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = pd.DataFrame({
        "category": ["A", "A", "B", "B", "C", "C", "C", "A", "B", "C"],
        "score": [80, 85, 70, 75, 95, 90, 100, 82, 72, 98]
    })
    ```
    풀이
    ```python
    sns.violinplot(x="category", y="score", data=data)
    sns.stripplot(x="category", y="score", data=data, jitter=True, color="salmon")
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn1-3.png)
### 2. 연속형 데이터(Continuous Data)
1. 평균 0, 표준편차 1을 따르는 정규분포 데이터를 500개 생성한 후 히스토그램과 KDE를 함께 시각화하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.randn(500)
    ```
    풀이
    ```python
    sns.histplot(data, kde=True)
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn2-1.png)
2. 0 부터 20까지 균등한 간격으로 생성된 데이터를 사용하여 사인 함수의 선 그래프를 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    x = np.linspace(0, 20, 100)
    y = np.sin(x)
    ```
    풀이
    ```python
    sns.lineplot(x=x, y=y)
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn2-2.png)
3. 랜덤한 100개의 연속형 데이터를 생성하여 산점도와 회귀선을 포함한 그래프를 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(0)
    x = np.random.rand(100) * 10
    y = 2 * x + np.random.randn(100)
    ```
    풀이
    ```python
    sns.regplot(x=x, y=y, line_kws = {'color' : 'salmon'})
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn2-3.png)
### 3. 관계형 데이터(Relational Data)
1. `Seaborn`의 `scatterplot`을 활용하여 "총 결제 금액"(`total_bill`)과 "팁"(`tip`)의 관계를 시각화하는 코드를 작성하세요. 단, `scatterplot`의 색상과 스타일을 다르게 설정하여 출력해야 합니다.</br>
    샘플 데이터
    ```python
    tips = sns.load_dataset("tips")
    ```
    풀이
    ```python
    sns.scatterplot(x="total_bill", y="tip", data=tips)
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn3-1.png)
2. `sns.regplot`을 사용하여 "총 결제 금액"(`total_bill`)과 "팁"(`tip`)의 관계를 나타내는 회귀선 그래프를 그리고 산점도의 투명도를 조정하는 코드를 작성하세요. 단, 산점도에서 특정 성별(`sex`)만 필터링하여 표시해야 합니다.</br>
    샘플 데이터
    ```python
    tips = sns.load_dataset("tips")
    ```
    풀이
    ```python
    sns.regplot(x="total_bill", y="tip", data=tips, scatter_kws={"alpha": 0.5}, line_kws={'color' : 'salmon'})
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn3-2.png)
3. `sns.pairplot`을 사용하여 다중 변수(`total_bill`, `tip`, `size`)간의 관계를 성별(`sex`)과 요일(`day`)에 따라 시각화하는 코드를 작성하세요. 단, 요일(`day`)에 따라 다른 색상을 적용해야 합니다.</br>
    샘플 데이터
    ```python
    tips = sns.load_dataset("tips")
    ```
    풀이
    ```python
    sns.pairplot(data=tips, hue="sex", vars=["total_bill", "tip", "size"], palette="coolwarm")
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/seaborn3-3.png)