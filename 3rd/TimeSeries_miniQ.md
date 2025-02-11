## 시계열 데이터 Mini-Quest
[Colab link]()</br>
1. [시계열 데이터(Time Series Data)](#1-시계열-데이터time-series-data)
2. [리샘플링(Resampling)](#2-리샘플링resampling)
3. [이동평균(Moving Average)](#3-이동평균moving-average)
4. [금융 데이터(Financial Data)](#4-금융-데이터financial-data)
### 1. 시계열 데이터(Time Series Data)
1. 100일간의 시계열 데이터를 생성하고 이를 선 그래프로 시각화하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    date_range = pd.date_range(start="2023-01-01", periods=100, freq="D")
    values = np.cumsum(np.random.randn(100))
    ```
2. 1번에서 생성한 데이터를 기반으로 7일 이동 평균을 계산하고 원본 데이터와 함께 그래프로 비교하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    date_range = pd.date_range(start="2023-01-01", periods=100, freq="D")
    values = np.cumsum(np.random.randn(100))
    ```
3. 1번에서 생성한 시계열 데이터에서 이상치를 탐지하고 이상치만 강조하여 그래프에 표시하는 코드를 작성하세요. 이상치는 사분위수 범위(IQR)를 이용해 판단합니다.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    date_range = pd.date_range(start="2023-01-01", periods=100, freq="D")
    values = np.cumsum(np.random.randn(100))
    ```
### 2. 리샘플링(Resampling)
1. `Pandas`를 사용하여 3시간 간격의 시계열 데이터를 생성한 후 하루 단위(`D`)로 평균을 구하는 다운 샘플링을 수행하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-05", freq="3h")
    ```
2. 3시간 간격으로 생성된 시계열 데이터에서 1시간 단위로 업 샘플링한 후 선형보간(`linear`)을 적용하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-03", freq="3h")
    ```
3. 3시간 간격으로 생성된 시계열 데이터에서 하루 단위(`D`)로 다운 샘플링을 수행한 후 각 날짜에 해당하는 최소(`min`)값과 최대(`max`)값을 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-07", freq="3h")
    ```
### 3. 이동평균(Moving Average)
1. 주어진 시계열 데이터에서 7일 단순 이동평균(SMA)을 계산하여 새로운 컬럼을 추가하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-20", freq="D")
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randint(50, 150, size=len(date_rng))
    })
    ```
2. 시계열 데이터에서 7일 지수 이동평균(EMA)을 계산하고 기존 데이터와 비교하여 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-20", freq="D")
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randint(50, 150, size=len(date_rng))
    })
    ```
3. 주어진 시계열 데이터에서 이동평균을 활용하여 변동성이 큰 날을 탐색하는 코드를 작성하세요. 7일 단순 이동평균(SMA)과 비교하여 특정 일자의 값이 이동평균보다 $\pm$ 20% 이상 차이가 나는 경우만 출력하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-20", freq="D")
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randint(50, 150, size=len(date_rng))
    })
    ```
### 4. 금융 데이터(Financial Data)
1. 샘플 금융 데이터프레임을 직접 생성한 후 데이터의 기분 정보(행 개수, 열 개수, 데이터 타입 등)를 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = {
        'Date': pd.date_range(start='2024-01-01', periods=10, freq='D'),
        'Open': [100, 102, 105, 103, 108, 107, 110, 112, 115, 118],
        'High': [102, 106, 108, 107, 110, 109, 112, 115, 117, 120],
        'Low': [98, 100, 103, 101, 106, 105, 108, 110, 113, 116],
        'Close': [101, 104, 106, 105, 109, 108, 111, 113, 116, 119],
        'Volume': [1000, 1200, 1500, 1300, 1600, 1400, 1700, 1800, 1900, 2000]
    }
    ```
2. 주어진 `df` 데이터프레임에서 5일 이동평균(SMA)과 5일 지수 이동평균(EMA)을 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = {
        'Date': pd.date_range(start='2024-01-01', periods=10, freq='D'),
        'Close': [101, 104, 106, 105, 109, 108, 111, 113, 116, 119]
    }
    ```
3. `df` 데이터프레임에서 주간(7일) 단위로 종가(Close) 평균을 리샘플링 한 후 이를 바탕으로 주간 변동성(표준편차)을 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start='2024-01-01', periods=30, freq='D')
    close_prices = np.random.uniform(100, 200, size=len(date_rng))
    ```