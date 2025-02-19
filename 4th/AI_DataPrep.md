## 데이터 전처리 Mini-Quest
[Colab link]()</br>
1. [Data Preprocessing(데이터 전처리)](#1-data-preprocessing데이터-전처리)
2. [Data Augmentation(데이터 증강)](#2-data-augmentation데이터-증강)
3. [Dataset Split(데이터셋 분할)](#3-dataset-split데이터셋-분할)

### 1. Data Preprocessing(데이터 전처리)
1. 간단한 데이터셋을 사용하여 데이터 전처리 과정을 단계별로 진행해보세요. 각 단계에서 필요한 코드를 작성하고 그 결과를 확인합니다.</br>

    - 가상의 학생 성적 데이터
        ```python
        data = {
        '학생': ['A', 'B', 'C', 'D', 'E'],
        '수학': [90, np.nan, 85, 88, np.nan],
        '영어': [80, 78, np.nan, 90, 85],
        '과학': [np.nan, 89, 85, 92, 80]
        }
        ```
    - 문제설명</br>
        1\. 데이터 수집 : 가상의 학생 성적 데이터를 생성합니다.</br>
        2\. 결측값 처리 : 데이터 셋에 누락된 값이 있을 때, 이를 평균값으로 대체 합니다.</br>
        3\. 이상치 제거 : 데이터셋에 이상치가 없는지 확인하고 필요하면 제거합니다.</br>
        4\. 데이터 정규화 : 수학, 영어, 과학 점수를 0과 1 사이의 값으로 스케일링 합니다.</br>
        5\. 데이터 분할 : 데이터셋을 학습용과 검증용으로 나눕니다.</br>

    - 풀이
    ```python
    # 1. 데이터 수집
    df = pd.DataFrame(data)

    # 2. 결측값 처리
    df.fillna({'수학': df['수학'].mean()}, inplace=True) # pandas 3.0 식
    df.fillna({'영어': df['영어'].mean()}, inplace=True)
    df.fillna({'과학': df['과학'].mean()}, inplace=True)

    # 3. 이상치 제거 (IQR 이용)
    for col in ['수학', '영어', '과학']:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    outlier = df[(df[col] < lower_bound) | (df[col] > upper_bound)]
    if outlier.empty:
        print(f"'{col}'에 이상치가 없습니다.")
    else:
        print("이상치 : ")
        print(outlier[[col]])
        df = df[(df[col] >= lower_bound) & (df[col] <= upper_bound)]

    # 4. 데이터 정규화
    scaler = MinMaxScaler()
    df[['수학', '영어', '과학']] = scaler.fit_transform(df[['수학', '영어', '과학']])

    # 5. 데이터 분할
    train, test = train_test_split(df, test_size=0.2, random_state=42)
    print("학습용 데이터셋 : ")
    print(train)
    print("검증용 데이터셋 : ")
    print(test)
    ```
    - 출력
    ```
    이상치 : 
        수학
    0  90.0
    2  85.0
    '영어'에 이상치가 없습니다.
    '과학'에 이상치가 없습니다.
    학습용 데이터셋 : 
    학생   수학        영어   과학
    3  D  1.0  1.000000  1.0
    4  E  0.0  0.583333  0.0
    검증용 데이터셋 : 
    학생   수학   영어    과학
    1  B  0.0  0.0  0.75
    ```

2. 가상의 데이터셋을 사용하여 전처리 과정을 직접 구현해보세요. 각 단계에 맞춰 코드를 작성하고 최종적으로 학습용과 검증용 데이터로 나누는 작업을 합니다.</br>

    - 가상의 제품 판매 데이터
        ```python
        data = {
        '제품': ['A', 'B', 'C', 'D', 'E'],
        '가격': [100, 150, 200, 0, 250],
        '판매량': [30, 45, np.nan, 55, 60]
        }
        ```
    - 문제</br>
        1\. 결측값을 중앙값으로 대체합니다.</br>
        2\. 이상치를 제거합니다. (ex. 가격이 0 이하인 경우)</br>
        3\. 데이터를 표준화합니다. (평균 0, 표준편차 1)</br>
        4\. 데이터를 학습용과 검증용으로 나눕니다. (학습용 70%, 검증용 30%)</br>

    - 문제 설명</br>
        1\. 데이터 수집 : 가상의 제품 판매 데이터를 생성합니다.</br>
        2\. 결측값 처리 : 데이터셋에 누락된 값이 있을 때, 이를 중앙값으로 대체합니다.</br>
        3\. 이상치 제거 : 가격이 0 이하인 데이터를 제거합니다.</br>
        4\. 데이터 표준화 : 가격과 판매량 데이터를 평균 0, 표준편차 1로 변환합니다.</br>
        5\. 데이터 분할 : 데이터셋을 학습용과 검증용으로 나눕니다.</br>

    - 풀이
    ```python
    # 1. 데이터 수집
    df = pd.DataFrame(data)

    # 2. 결측값 처리
    df.fillna({'판매량': df['판매량'].median()}, inplace=True)

    # 3. 이상치 제거
    df = df[df['가격'] > 0]

    # 4. 데이터 표준화
    scaler = StandardScaler()
    df[['가격', '판매량']] = scaler.fit_transform(df[['가격', '판매량']])

    # 5. 데이터 분할
    train_df, valid_df = train_test_split(df, test_size=0.3, random_state=42)

    print("학습용 데이터셋:")
    print(train_df)
    print("\n검증용 데이터셋:")
    print(valid_df)
    ```
    - 출력
    ```
    학습용 데이터셋:
    제품        가격       판매량
    0  A -1.341641 -1.501111
    2  C  0.447214  0.346410

    검증용 데이터셋:
    제품        가격       판매량
    1  B -0.447214 -0.115470
    4  E  1.341641  1.270171
    ```
### 2. Data Augmentation(데이터 증강)
1. Python과 Pillow를 사용하여 이미지 회전, 뒤집기, 이동, 확대/축소, 색상 변형, 노이즈 추가를 해보세요.</br>
    - 이미지 예시
        ![alt text](/4th/image/DataAug_1.png)
    
    - 문제 설명
        1\. 필요한 라이브러리 설치 및 불러오기</br>
        2\. 이미지 불러오기</br>
        3\. 이미지 증강 기법 적용</br>

2. Python과 TensorFlow를 사용해 데이터셋을 증강하여 더 많은 학습 데이터를 생성하고 이를 활용해 모델을 학습시켜 보세요.</br>
    - 문제 설명
        1\. 필요한 라이브러리 설치 및 불러오기
        2\. 데이터셋 불러오기 및 증강
        3\. 모델 생성 및 학습
        4\. 모델 평가

    - 데이터셋 및 모델 관련 코드
        ```python
        # 1. 필요한 라이브러리 설치 및 불러오기
        import tensorflow as tf
        from tensorflow.keras.preprocessing.image import ImageDataGenerator
        from tensorflow.keras import layers, models

        # 2. 데이터셋 불러오기 및 증강
        (train_images, train_labels), (test_images, test_labels) = tf.keras.datasets.cifar10.load_data()

        # 데이터 증강을 위한 ImageDataGenerator 설정
        ...

        # 데이터셋 준비
        train_images = train_images.astype('float32') / 255.0
        test_images = test_images.astype('float32') / 255.0

        # 3. 모델 생성 및 학습
        model = models.Sequential([
            layers.Input(shape=(32, 32, 3)),
            layers.Conv2D(32, (3, 3), activation='relu'),
            layers.MaxPooling2D((2, 2)),
            layers.Conv2D(64, (3, 3), activation='relu'),
            layers.MaxPooling2D((2, 2)),
            layers.Conv2D(64, (3, 3), activation='relu'),
            layers.Flatten(),
            layers.Dense(64, activation='relu'),
            layers.Dense(10, activation='softmax')
        ])

        model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])

        # 증강된 데이터를 사용하여 모델 학습
        train_generator = datagen.flow(train_images, train_labels, batch_size=64)
        model.fit(train_generator, epochs=10, validation_data=(test_images, test_labels))

        # 4. 모델 평가
        test_loss, test_acc = model.evaluate(test_images, test_labels)
        print(f'Test accuracy: {test_acc}')
        ```
### 3. Dataset Split(데이터셋 분할)
1. 데이터셋을 랜덤으로 분할하고 각 데이터셋의 크기를 출력하여 분할이 제대로 이루어졌는지 확인해보세요.</br>
    - 문제 설명</br>
        1\. 데이터 로드
        ```python
        data = {
            'feature1': range(1, 101),
            'feature2': range(101, 201),
            'label': [1 if x % 2 == 0 else 0 for x in range(1, 101)]
        }
        df = pd.DataFrame(data)
        ```
        2\. 데이터셋 분할 : 데이터를 학습, 검증, 테스트 데이터셋으로 나눕니다.
        3\. 결과 확인 : 각 데이터셋의 크기를 출력하여 분할 결과를 확인합니다.

2. 간단한 모델을 학습하고 검증 및 테스트 데이터셋을 사용하여 모델의 성능을 편가해보세요.</br>
    - 문제 설명
        1\. 데이터 로드 및 분할 : 데이터를 학습, 검증, 테스트 데이터셋으로 분할합니다.</br>
        ```python
        data = {
            'feature1': range(1, 101),
            'feature2': range(101, 201),
            'label': [1 if x % 2 == 0 else 0 for x in range(1, 101)]
        }
        df = pd.DataFrame(data)
        ```
        2\. 모델 학습 : 학습 데이터셋을 사용하여 간단한 모델을 학습합니다.
        3\. 모델 평가 : 검증 데이터셋을 사용하여 모델의 성능을 평가합니다.
        4\. 최종 평가 : 테스트 데이터셋을 사용하여 모델의 최종 성능을 평가합니다.

    - 모델 학습, 평가, 최종 평가 코드
        ```python
        # 필요한 라이브러리 불러오기
        import pandas as pd
        from sklearn.model_selection import train_test_split
        from sklearn.linear_model import LogisticRegression
        from sklearn.metrics import accuracy_score

        # 샘플 데이터 생성
        data = {
            'feature1': range(1, 101),
            'feature2': range(101, 201),
            'label': [1 if x % 2 == 0 else 0 for x in range(1, 101)]
        }
        df = pd.DataFrame(data)

        # 데이터셋을 학습, 검증, 테스트 세트로 분할 (60:20:20 비율)
        ...

        # 학습 데이터와 레이블 분리
        X_train = train_data[['feature1', 'feature2']]
        y_train = train_data['label']
        X_validation = validation_data[['feature1', 'feature2']]
        y_validation = validation_data['label']
        X_test = test_data[['feature1', 'feature2']]
        y_test = test_data['label']

        # 모델 학습
        model = LogisticRegression()
        model.fit(X_train, y_train)

        # 검증 데이터로 성능 평가
        y_val_pred = model.predict(X_validation)
        val_accuracy = accuracy_score(y_validation, y_val_pred)
        print("검증 데이터 정확도:", val_accuracy)

        # 테스트 데이터로 최종 성능 평가
        y_test_pred = model.predict(X_test)
        test_accuracy = accuracy_score(y_test, y_test_pred)
        print("테스트 데이터 정확도:", test_accuracy)
        ```
