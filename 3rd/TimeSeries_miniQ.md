## 시계열 데이터 Mini-Quest
[Colab link](https://colab.research.google.com/drive/1ZoRvh5Inc4FEDxGQzSEhhcL_N_gLD4vb?usp=sharing)</br>
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
    풀이
    ```python
    plt.figure(figsize=(9, 6))
    plt.plot(date_range, values)
    plt.xlabel("Date")
    plt.ylabel("Value")
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/time1-1.png)
2. 1번에서 생성한 데이터를 기반으로 7일 이동 평균을 계산하고 원본 데이터와 함께 그래프로 비교하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    date_range = pd.date_range(start="2023-01-01", periods=100, freq="D")
    values = np.cumsum(np.random.randn(100))
    ```
    풀이
    ```python
    plt.figure(figsize=(9, 6))

    values_series = pd.Series(values, index=date_range)
    rolling = values_series.rolling(window=7).mean()
    plt.plot(date_range, values, label="Original Data")
    plt.plot(date_range, rolling, label="Moving Data")

    plt.xlabel("Date")
    plt.ylabel("Value")
    plt.legend()
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/time1-2.png)
3. 1번에서 생성한 시계열 데이터에서 이상치를 탐지하고 이상치만 강조하여 그래프에 표시하는 코드를 작성하세요. 이상치는 사분위수 범위(IQR)를 이용해 판단합니다.</br>
    샘플 데이터
    ```python
    np.random.seed(42)
    date_range = pd.date_range(start="2023-01-01", periods=100, freq="D")
    values = np.cumsum(np.random.randn(100))
    ```
    풀이
    ```python
    plt.figure(figsize=(9, 6))
    plt.plot(date_range, values)

    Q1 = np.percentile(values, 25)
    Q3 = np.percentile(values, 75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    outliers = values[(values < lower_bound) | (values > upper_bound)]
    outliers_dates = date_range[(values < lower_bound) | (values > upper_bound)]

    plt.scatter(outliers_dates, outliers, s = 100, color="red", marker = 'o', label="Outliers")
    plt.xlabel("Date")
    plt.ylabel("Value")
    plt.legend()
    plt.show()
    ```
    출력</br>
    ![alt text](/3rd/image/time1-3.png)</br>
    **해당 seed에서 Outlier 확인되지 않음**
### 2. 리샘플링(Resampling)
1. `Pandas`를 사용하여 3시간 간격의 시계열 데이터를 생성한 후 하루 단위(`D`)로 평균을 구하는 다운 샘플링을 수행하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-05", freq="3h")
    ```
     풀이
    ```python
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randn(len(date_rng))
    })

    df = df.set_index("datetime")
    df_d = df.resample("D").mean()
    df_d
    ```
    출력</br>
    |datetime|value|
    |---|---|
    |2024-01-01 00:00:00|-0\.08468487392165791|
    |2024-01-02 00:00:00|0\.10880976052339267|
    |2024-01-03 00:00:00|0\.07174422253364321|
    |2024-01-04 00:00:00|-0\.08318076005148432|
    |2024-01-05 00:00:00|-1\.0623037137261049|
2. 3시간 간격으로 생성된 시계열 데이터에서 1시간 단위로 업 샘플링한 후 선형보간(`linear`)을 적용하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-03", freq="3h")
    ```
    풀이
    ```python
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randn(len(date_rng))
    })

    df = df.set_index("datetime")
    df_h = df.resample("H").interpolate(method="linear")
    df_h.head()
    ```
    출력
    |datetime|value|
    |---|---|
    |2024-01-01 00:00:00|0\.25049285034587654|
    |2024-01-01 01:00:00|0\.2824779700629096|
    |2024-01-01 02:00:00|0\.31446308977994264|
    |2024-01-01 03:00:00|0\.3464482094969757|
    |2024-01-01 04:00:00|0\.004290565805153523|
3. 3시간 간격으로 생성된 시계열 데이터에서 하루 단위(`D`)로 다운 샘플링을 수행한 후 각 날짜에 해당하는 최소(`min`)값과 최대(`max`)값을 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-07", freq="3h")
    ```
    풀이
    ```python
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randn(len(date_rng))
    })

    df = df.set_index("datetime")
    df_d = df.resample("D").agg(["min", "max"])
    df_d
    ```
    출력

    |datetime|value min|value max|
    |---|---|---|
    |2024-01-01 00:00:00|-0\.8895144296255233|1\.8967929826539474|
    |2024-01-02 00:00:00|-1\.0708924980611123|2\.720169166589619|
    |2024-01-03 00:00:00|-1\.5148472246858646|0\.714000494092092|
    |2024-01-04 00:00:00|-1\.245738778711988|0\.8563987943234723|
    |2024-01-05 00:00:00|-1\.377669367957091|1\.083051243175277|
    |2024-01-06 00:00:00|-0\.3152692446403456|3\.852731490654721|
    |2024-01-07 00:00:00|0\.7589692204932674|0\.7589692204932674|
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
    풀이
    ```python
    df = df.set_index("datetime")
    df['SMA'] = df['value'].rolling(window=7).mean()
    df
    ```
    출력
    |datetime|value|SMA|
    |---|---|---|
    |2024-01-01 00:00:00|64|NaN|
    |2024-01-02 00:00:00|103|NaN|
    |2024-01-03 00:00:00|109|NaN|
    |2024-01-04 00:00:00|146|NaN|
    |2024-01-05 00:00:00|57|NaN|
    |2024-01-06 00:00:00|102|NaN|
    |2024-01-07 00:00:00|109|98\.57142857142857|
    |2024-01-08 00:00:00|54|97\.14285714285714|
    |2024-01-09 00:00:00|117|99\.14285714285714|
    |2024-01-10 00:00:00|55|91\.42857142857143|
    |2024-01-11 00:00:00|145|91\.28571428571429|
    |2024-01-12 00:00:00|143|103\.57142857142857|
    |2024-01-13 00:00:00|96|102\.71428571428571|
    |2024-01-14 00:00:00|148|108\.28571428571429|
    |2024-01-15 00:00:00|104|115\.42857142857143|
    |2024-01-16 00:00:00|89|111\.42857142857143|
    |2024-01-17 00:00:00|101|118\.0|
    |2024-01-18 00:00:00|65|106\.57142857142857|
    |2024-01-19 00:00:00|62|95\.0|
    |2024-01-20 00:00:00|79|92\.57142857142857|
2. 시계열 데이터에서 7일 지수 이동평균(EMA)을 계산하고 기존 데이터와 비교하여 출력하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-20", freq="D")
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randint(50, 150, size=len(date_rng))
    })
    ```
    풀이
    ```python
    df = df.set_index("datetime")
    df['EMA'] = df['value'].ewm(span=7).mean()
    df
    ```
    출력
    |datetime|value|EMA|
    |---|---|---|
    |2024-01-01 00:00:00|68|68\.0|
    |2024-01-02 00:00:00|66|66\.85714285714286|
    |2024-01-03 00:00:00|112|86\.37837837837837|
    |2024-01-04 00:00:00|68|79\.65714285714286|
    |2024-01-05 00:00:00|141|99\.76440460947504|
    |2024-01-06 00:00:00|107|101\.96495396495396|
    |2024-01-07 00:00:00|104|102\.55208846939495|
    |2024-01-08 00:00:00|139|112\.67777871979652|
    |2024-01-09 00:00:00|139|119\.79254395552275|
    |2024-01-10 00:00:00|111|117\.46323647560905|
    |2024-01-11 00:00:00|72|105\.59622240305768|
    |2024-01-12 00:00:00|58|93\.30791815102695|
    |2024-01-13 00:00:00|61|85\.03438203131118|
    |2024-01-14 00:00:00|50|76\.11689520683338|
    |2024-01-15 00:00:00|107|83\.94224516417076|
    |2024-01-16 00:00:00|50|75\.37077549761145|
    |2024-01-17 00:00:00|83|77\.2925273295269|
    |2024-01-18 00:00:00|145|94\.31536532306455|
    |2024-01-19 00:00:00|97|94\.98937389098847|
    |2024-01-20 00:00:00|138|105\.77623785001187|
3. 주어진 시계열 데이터에서 이동평균을 활용하여 변동성이 큰 날을 탐색하는 코드를 작성하세요. 7일 단순 이동평균(SMA)과 비교하여 특정 일자의 값이 이동평균보다 $\pm$ 20% 이상 차이가 나는 경우만 출력하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start="2024-01-01", end="2024-01-20", freq="D")
    df = pd.DataFrame({
        "datetime": date_rng,
        "value": np.random.randint(50, 150, size=len(date_rng))
    })
    ```
    풀이
    ```python
    df = df.set_index("datetime")
    df['SMA'] = df['value'].rolling(window=7).mean()

    result = df[(df['value'] - df['SMA']).abs() > 0.2 * df['SMA']]
    result
    ```
    출력
    |datetime|value|SMA|
    |---|---|---|
    |2024-01-07 00:00:00|71|91\.28571428571429|
    |2024-01-08 00:00:00|142|104\.42857142857143|
    |2024-01-11 00:00:00|75|108\.42857142857143|
    |2024-01-12 00:00:00|65|101\.71428571428571|
    |2024-01-14 00:00:00|135|108\.28571428571429|
    |2024-01-16 00:00:00|78|97\.71428571428571|
    |2024-01-17 00:00:00|127|98\.0|
    |2024-01-18 00:00:00|141|107\.42857142857143|
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
    풀이
    ```python
    df = pd.DataFrame(data)
    df.info()
    ```
    출력
    ```
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10 entries, 0 to 9
    Data columns (total 6 columns):
    #   Column  Non-Null Count  Dtype         
    ---  ------  --------------  -----         
    0   Date    10 non-null     datetime64[ns]
    1   Open    10 non-null     int64         
    2   High    10 non-null     int64         
    3   Low     10 non-null     int64         
    4   Close   10 non-null     int64         
    5   Volume  10 non-null     int64         
    dtypes: datetime64[ns](1), int64(5)
    memory usage: 612.0 bytes
    ```
2. 주어진 `df` 데이터프레임에서 5일 이동평균(SMA)과 5일 지수 이동평균(EMA)을 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    data = {
        'Date': pd.date_range(start='2024-01-01', periods=10, freq='D'),
        'Close': [101, 104, 106, 105, 109, 108, 111, 113, 116, 119]
    }
    ```
    풀이
    ```python
    df = pd.DataFrame(data)
    df['SMA'] = df['Close'].rolling(window=5).mean()
    df['EMA'] = df['Close'].ewm(span=5).mean()
    df
    ```
    출력
    |index|Date|Close|SMA|EMA|
    |---|---|---|---|---|
    |0|2024-01-01 00:00:00|101|NaN|101\.0|
    |1|2024-01-02 00:00:00|104|NaN|102\.8|
    |2|2024-01-03 00:00:00|106|NaN|104\.31578947368419|
    |3|2024-01-04 00:00:00|105|NaN|104\.6|
    |4|2024-01-05 00:00:00|109|105\.0|106\.28909952606634|
    |5|2024-01-06 00:00:00|108|106\.4|106\.91428571428573|
    |6|2024-01-07 00:00:00|111|107\.8|108\.3608547838757|
    |7|2024-01-08 00:00:00|113|109\.2|109\.97002379064236|
    |8|2024-01-09 00:00:00|116|111\.4|112\.0336967294351|
    |9|2024-01-10 00:00:00|119|113\.4|114\.39677725118484|
3. `df` 데이터프레임에서 주간(7일) 단위로 종가(Close) 평균을 리샘플링 한 후 이를 바탕으로 주간 변동성(표준편차)을 계산하는 코드를 작성하세요.</br>
    샘플 데이터
    ```python
    date_rng = pd.date_range(start='2024-01-01', periods=30, freq='D')
    close_prices = np.random.uniform(100, 200, size=len(date_rng))
    ```
    풀이
    ```python
    df = pd.DataFrame({
        'Date': date_rng,
        'Close': close_prices
    })

    df = df.set_index('Date')

    weekly_df = df.resample('W').agg(Weekly_std=('Close', 'std'))

    weekly_df
    ```
    출력
    |Date|Weekly\_std|
    |---|---|
    |2024-01-07 00:00:00|29\.527024629630652|
    |2024-01-14 00:00:00|21\.552952272742488|
    |2024-01-21 00:00:00|28\.47130585565111|
    |2024-01-28 00:00:00|27\.804351762199474|
    |2024-02-04 00:00:00|27\.99068188798478|