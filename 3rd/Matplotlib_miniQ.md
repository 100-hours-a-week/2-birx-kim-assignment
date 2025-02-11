## Matplotlib Mini-Quest
[Colab link]()</br>
1. [막대 그래프(Bar Chart)](#1-막대-그래프bar-chart)
2. [히스토그램(Histogram)](#2-히스토그램histogram)
3. [산점도(Scatter Plot)](#3-산점도scatter-plot)
4. [박스플롯(Box Plot)](#4-박스플롯box-plot)
5. [고급 다중 그래프(Advanced Multiple Graphs)](#5-고급-다중-그래프advanced-multiple-graphs)
6. [벤 다이어그램(Venn diagram)](#6-벤-다이어그램venn-diagram)
### 1. 막대 그래프(Bar Chart)
1. `Matplotlib`을 활용하여 5개의 카테고리와 각각의 값이 포함된 기본 세로 막대 그래프를 생성하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    categories = ['A', 'B', 'C', 'D', 'E']
    values = [12, 25, 18, 30, 22]
    ```
2. 누적형 막대 그래프를 생성하여 두 개의 연도별 데이터를 각각 다른 색상으로 누적하여 포현하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    categories = ['A', 'B', 'C', 'D', 'E']
    values_2023 = [10, 15, 20, 25, 30]
    values_2024 = [5, 10, 12, 18, 22]
    ```
3. 한 기업의 부서별 연간 성과(2023년 vs 2024년)를 비교하는 그룹형 막대 그래프를 생성하는 코드를 작성하세요.
    샘플 데이터
    ```python
    departments = ['Sales', 'Marketing', 'IT', 'HR', 'Finance']
    performance_2023 = [80, 70, 90, 60, 75]
    performance_2024 = [85, 75, 95, 65, 80]
    ```
### 2. 히스토그램(Histogram)
1. 정규분포를 따르는 1,000개의 데이터를 생성한 후 구간을 15개로 설정한 히스토그램을 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = np.random.randn(1000)
    ```
2. 두 개의 서로 다른 정규분포를 따르는 데이터셋을 생성한 후 두 히스토그램을 같은 그래프에서 겹쳐서 비교하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data1 = np.random.randn(1000)
    data2 = np.random.randn(1000) + 3
    ```
3. 한 데이터셋의 누적 히스토그램을 그린 후 X축과 Y축의 적절한 레이블을 설정하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = np.random.randn(1000)
    ```
### 3. 산점도(Scatter Plot)
1. 두 개의 리스트를 사용하여 산점도를 그리고 X축과 Y축의 라벨을 추가하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    x = [1, 2, 3, 4, 5]
    y = [3, 1, 4, 5, 2]
    ```
2. `numpy`를 활용하여 난수를 생성한 후 산점도를 그리고 점의 색상과 투명도를 설정하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    x = np.random.rand(50) * 10
    y = np.random.rand(50) * 10
    ```
3. `numpy`를 활용하여 세 개의 그룹('A', 'B', 'C')에 속하는 데이터의 산점도를 서로 다른 색상으로 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(10)
    x = np.random.randn(50) * 2
    y = np.random.randn(50) * 2
    categories = np.random.choice(['A', 'B', 'C'], size=50)
    ```
### 4. 박스플롯(Box Plot)
1. 평균 0, 표준편차 1을 따르는 정규분포 난수 50개를 생성한 후 해당 데이터를 이용해 기본 박스 플롯을 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    data = np.random.randn(50)
    ```
2. 세 개의 그룹(Group A, Group B, Group C)에 대해 각각 다른 평균을 가지는 데이터를 생성하고 이를 이용해 다중 박스 플롯을 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    group_a = np.random.randn(50) * 1.5
    group_b = np.random.randn(50) * 1.5 + 3
    group_c = np.random.randn(50) * 1.5 - 3
    ```
3. 평균이 서로 다른 두 개의 그룹(Group X, Group Y)을 비교하는 박스 플롯을 그리세요. 단 이상값을 강조하고 스타일을 커스터마이징해야 합니다.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    group_x = np.random.randn(50) * 2
    group_y = np.random.randn(50) * 2 + 5
    ```
### 5. 고급 다중 그래프(Advanced Multiple Graphs)
1. `plt.subplots()`를 사용하여 2 x 1 형태의 서브플롯을 만들고 첫 번째 서브플롯에는 $y = x^2$ 두 번째 서브플롯에는 $y = x^3$을 그리는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    x = np.linspace(-5, 5, 100)
    y1 = x ** 2
    y2 = x ** 3
    ```
2. X축을 공유하는 1행 2열 형태의 서브플롯을 생성하고 첫 번째 서브플롯에는 정규분포를 다르는 난수의 히스토그램, 두 번째 서브플롯에는 균등분포를 따르는 난수의 히스토그램을 그리세요.</br>
    샘플 데이터
    ```python
    normal_data = np.random.randn(1000)
    uniform_data = np.random.rand(1000)
    ```
3. `gridspec`을 사용하여 불규칙한 레이아웃의 서브플롯을 생성하고 각각 선 그래프, 산점도, 막대 그래프, 히스토그램을 그리세요.</br>
    샘플 데이터
    ```python
    x = np.linspace(0, 10, 100)
    y1 = np.sin(x)
    y2 = np.random.randn(100)
    categories = ['A', 'B', 'C', 'D', 'E']
    values = [3, 7, 5, 2, 8]
    ```
### 6. 벤 다이어그램(Venn diagram)
1. 두 개의 과일 집합을 정의하고 두 집합의 차집합(한 집합에만 존재하는 요소)을 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    set_A = {"사과", "바나나", "체리", "망고"}
    set_B = {"바나나", "망고", "포도", "수박"}
    ```
2. 벤 다이어그램을 그리지 않고 세 개의 집합을 비교하여 각 집합이 단독으로 가지는 요소 개수와 교집합 개수를 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    set_A = {"사과", "바나나", "체리", "망고"}
    set_B = {"바나나", "망고", "포도", "수박"}
    set_C = {"망고", "수박", "딸기", "오렌지"}
    ```
3. 벤 다이어그램을 그리면서 특정 조건을 만족하는 경우 색상을 다르게 지정하는 코드를 작성하세요.</br>
    조건 : 두 개의 집합을 비교할 때, 교집합이 2개 이상이면 노란색 그렇지 않으면 기본 색상을 사용하세요.</br>
    샘플 데이터
    ```python
    set_A = {"사과", "바나나", "체리", "망고"}
    set_B = {"바나나", "망고", "포도", "수박"}
    ```
