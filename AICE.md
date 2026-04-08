\[AICE]



http://aice.study/main



**1. 예측 문제 종류**

1\) 분류 : 범주형 답 (순서 X)

2\) 회귀 : 연속형 답 (e.g. 주가)



**2. 데이터 구조**

1\) 표 데이터 : 행(row)과 열(column)으로 이루어진 정형 데이터

a) columm : 변수(variable) 혹은 특징

\- 범주형

\- 연속형

→ 상관관계를 파악했을 때, 1에 가까울수록 관계가 깊다. (선형관계 X)



**3. 분류**

1\) 종속변수 (y) : 맞추려는 값 (목표 변수)

2\) 독립변수 (x) : 주어진 정보 (설명 변수)

→ x를 알면 y를 맞출 수 있다.

→ y가 범주형이면 분류

&#x09; 연속형이면 회귀



**4. 데이터 분할 (train\_test\_split)**

1\) 이유 : 실제 데이터의 성능 평가

2\) 종류

a) 학습용 데이터

b) 평가용 데이터



**5. 모델링 종류**

1\) 통계적 방법 : 수학적 계산을 통해 모델 도출 (e.g. 선형 회귀)

2\) 머신러닝 : 데이터로부터 스스로 규칙을 찾는 방법 (e.g. DecisionTree) → 기준이 정해져 있다.

3\) 딥러닝 : 스스로 규칙을 학습하는 인공신경망 (활성화 함수의 일종) → 기준 자체를 학습한다.



**6. 모델링 의미**

1\) 모델 = 함수

2\) 오류 최소화

3\) 손실 함수 최소화

4) 모델 학습 = 함수 성능 향상



**7. 손실함수 종류**

1\) 회귀 문제 : MAE · MSE

a) M(ean) A(bsolute) E(rror) : 오차값에 절대값을 붙이는 손실 함수

b) M(ean) S(quared) E(rror) : 예측값과 실제값의 차이를 제곱해 평균한 손실 함수

2\) 분류 문제 : Cross Entropy Loss → 모델이 예측한 확률 분포와 실제 정답 분포의 차이를 측정하는 손실 함수



**8. 경사하강법** : 주어진 지점에서의 순간 기울기를 토대로 현재보다 낮은 곳으로 이동



**9. 회귀 문제 과정**

1\) 목표변수(y), 독립변수(x) 지정 : 컬럼명으로 목표변수, 예측에 사용할 독립변수 지정 (열 분할)

e.g. x = df\[\['성별', '나이', '키', '직종\_금융', '직종\_서비스']]

&#x09; y = df\['연 수입']

\* 'df'를 'data'로 지정하라고 제시되어 있다면 따라야 한다.

2\) 범주형 변수 인코딩 : 글자를 숫자로 변환

a) 레이블 인코딩

e.g. from sklearn.preprocessing import  LabelEncoder



&#x09; le = LabelEncoder()

&#x09; x\['성별'] = le.fit\_transform(x\['성별])

→ 남자 : 0

&#x20;      여자 : 0

b)원 - 핫 인코딩 : 범주어 개수가 증가할 경우

e.g. x = pd.get\_dummies(data = x, columns = \['직종'], drop\_first = True)

3\) 데이터 분할 : train set, test set으로 무작위 분할 (행 분할)

e.g. from sklearn.model\_selection import train\_test\_split



&#x09; x\_train, x\_test, y\_train, y\_test = train\_test\_split(

&#x09; x,

&#x09; y,

&#x20;	 test\_size = o.2,

&#x09; random\_state = 0

&#x09; )



&#x09; x\_train.shape, x\_test.shape, y\_train.shape, y\_test.shape

4\) 모델 불러온 후 적합(학습) : DecisionTreeeRegressor (회귀)

e.g. from sklearn.tree import DecisionRegressor



&#x09; dtr = DecisionTreeRegressor()

&#x09; dtr.fit(x\_train, y\_train)

a) RandomForestRegressor

e.g. from sklearn.ensemble import RandomForestRegressor



&#x20;    rfr = RandomForestRegressor()

&#x09;rfr.fit(x\_train, y\_train)



&#x09;y\_pred = rfr.predict(x\_test)

&#x09;y\_pred

b) 딥러닝 : DNN

e.g. import tensorflow as tf

&#x09; from tensorflow.keras.models import Sequential

&#x20;        from tensorflow.keras.layers import Dense

&#x09; from tensorflow.keras.callbacks import EarlyStopping, ModelCheckpoint

&#x09; 

&#x20;        # DNN 최종 출력층에 'activation function' 사용 X 

&#x09; model = Sequential()

&#x09; # x의 칼럼(feature) 수 : 5개

&#x20;    model.add(Dense(4, activation = 'relu', input\_shape = (5, )))

&#x20;        model.add(Dense(4, activation = 'relu'))

&#x20;        model.add(Dense(1, activation = None)) # 회귀



&#x20;        model.summary() 



&#x09; # 손실함수는 MSE, optimizer는 Adam

&#x09; model.compile(loss = 'mse', optimizer = 'adam', metrics = \['mae'])

&#x09;

&#x09; model.fit(x\_train, y\_train, epochs = 10, batch\_size = 2,

&#x09;		      validation\_data = (x\_test, y\_test))



&#x09; y\_pred = model.predict(x\_test)

&#x09; print(y\_pred)

5\) test set에 대하여 모델 평가 : DecisionTreeRegressor (회귀)

e.g. from sklearn.metrics import mean\_squared\_error, mean\_absolute\_error



&#x09; mse = mean\_squared\_error(y\_test, y\_pred)

&#x09; mae = mean\_absolute\_error(y\_test, y\_pred)



**10. 분류 문제 과정**

1\) 목표변수(y), 독립변수(x) 지정 : 회귀 문제와 동일

2\) 범주형 변수 인코딩 : 회귀 문제와 동일

3\) 데이터 분할 : train set 무작위 분할 (층화 분할)

a) 층화 분할 : 'stratify = y' 옵션으로 'train\_set'과 'test\_set' 안의 범주 분포 설정

→ 실제 데이터의 분포를 학습 데이터가 잘 반영하고 있다고 가정하여, 소수 클래스에 대해 과소적합 되는 것을 방지

e.g. from sklearn.model\_selection import train\_test\_split



&#x20;     x\_train, x\_test, y\_train, y\_test = train\_test\_split(

&#x20;     x,

&#x20;     y,

&#x20;     stratity = y,

&#x20;     test\_size = o.4,

&#x20;     random\_state = 0

&#x20;     )



&#x20;     x\_train.shape, x\_test.shape, y\_train.shape, y\_test.shape

4\) 모델 불러온 후 적합(학습) : DecisionTreeeClassifier (분류)

e.g. from sklearn.tree import DecisionClassifier



&#x20;     dtc = DecisionTreeClassifier()

&#x20;     dtc.fit(x\_train, y\_train)

a) RandomForestClassifier

e.g. from sklearn.ensemble import RandomForestClassifier



&#x20;     rfc = RandomForestClassifier()

&#x20;     rfc.fit(x\_train, y\_train)

&#x20;    

&#x20;     y\_pred = rfc.predict(x\_test)

&#x20;     y\_pred

b) 딥러닝 : DNN

e.g. import tensorflow as tf

&#x20;     from tensorflow.keras.models import Sequential

&#x20;     from tensorflow.keras.layers import Dense

&#x20;     from tensorflow.keras.callbacks import EarlyStopping, ModelCheckpoint

&#x09;

&#x20;     # DNN 최종 출력층에 'activation function softmax' 사용

&#x20;     model = Sequential()

&#x20;     # x의 칼럼(feature) 수 : 4개

&#x20;     model.add(Dense(3, activation = 'relu', input\_shape = (4, )))

&#x20;     model.add(Dense(3, activation = 'relu'))

&#x20;     model.add(Dense(2, activation = 'softmax')) # 2 - class 분류



&#x20;     model.summary()

&#x20;     

&#x20;     # 손실함수를 'sparse\_categorical\_crossentropy'로 설정

&#x20;     model.compile(loss = 'sparse\_categorical\_crossentropy',

&#x20;                                            optimizer = 'adam',

&#x09;				     metrics = \['accuracy'])

&#x09;

&#x20;     model.fit(x\_train, y\_train, epochs = 10, batch\_size = 2,

&#x09;	   validation\_data = (x\_test, y\_test))



&#x20;     y\_pred = model.predict(x\_test)

&#x20;     print(y\_pred)

5\) test set에 대하여 모델 평가 : DecisionTreeClassifier (분류)

e.g. from sklearn.metrics import accuracy\_score, f1\_score



&#x20;         acc = accuracy\_socre(y\_test, y\_pred)

&#x20;     f1 = f1\_socre(y\_test, y\_pred)



&#x20;     acc, f1



**11. 시각화**

1\) Scatter Plot (산점도) : 두 변수의 분포를 통해 관계 파악 (x, y 지정)

e.g. sns.scatterplot(data = tips, x = "total\_bill", y = "tip")

2\) Lm Plot (선형 모델 그래프) : 산점도에 선형 회귀 모델의 추세선과 신뢰 구간 표시

e.g. sns.lmplot(data = tips, x = 'total\_bill', y = 'tip', hue = 'time')

3\) Count Plot : 범주별 값의 개수 표시 (x만 지정)

e.g. sns.countplot(data = titanic, x = 'class'

4\) Bar Plot (막대 그래프) : 범주별 값 분포를 통해 변수 간 관계 파악

e.g. sns.barplot(data = titanic, x = 'sex', y = 'survived')

5\) Box Plot (상자 그림) : 연속형 변수의 값 분포 파악

e.g. sns.boxplot(data = penguins, x = 'body\_mass\_g')

6\) Violin Plot : 'Box Plot'보다 더 상세한 분포 파악

e.g. sns.violinplot(data = penguins, x = 'body\_mass\_g', y = 'species')

7\) Join Plot : 히스토그램과 산점도를 한 번에 표현 (두 변수 간의 상관관계 파악)

e.g. sns.joinplot(data = penguins, x = "bill\_length\_mm", y = "body\_mass\_g")

8\) Heat Map : 데이터 크기를 색상으로 표현 (두 변수 간의 상관관계 파악)

e.g. sns.heatmap(penguins.corr(), annot = True)



**12. 전처리**

1\) 변수 타입 확인 : 정보 출력을 통해 변수의 성질과 맞지 않는 타입이 있는지 확인

e.g. df.info()

2\) 변수 타입 변경 : 타입 변경이 필요한 경우 'astype' 사용

e.g. df\['고객 번호'] = df\['고객 번호'].astype('object')

e.g. df\['나이'] = df\['나이'].astype('float')

3\) 결측치 확인

a) 원본 정보 : 정보 출력을 통해 'null' 개수를 간접 파악

e.g. df.info()

b) 결측치 : 칼럼별로 결측치가 몇 개 있는지 직접 출력

e.g. df.isnull().sum()

4\) 결측치 처리

a) 칼럼 제거

e.g. df = df.drop('sex', axis = 1)

b) 행 제거

e.g. df = df.dropna()

&#x09; df = df.dropna(subset = \['bill\_depth\_mm'])

c) 평균값 대치 : 결측치를 해당 칼럼의 평균값으로 대치 (수치형 변수)

e.g. df\['bill\_length\_mm'] = df\['bill\_length\_mm'].fillna(df\['bill\_length\_mm'].mean())

d) 최빈값 대치 : 결측치를 해당 칼럼의 최빈값으로 대치 (범주형 변수)
e.g. df\['species'] = df\['species'].fillna(df\['species'].mode()\[0])

5\) 변수 형태별로 분리 : 범주형 변수, 혹은 수치형(연속형, 정수형) 변수만 수집

e.g. cat\_cols = df.select\_dtypes('object').columns.values

&#x09; num\_cols = df.select\_dtypes('number').columns.values

6\)  이상치 확인 (수치형)

a) 분포 확인 : 수치형 변수의 분포 확인

e.g. df.select\_dtypes('number').describe()

&#x09; sns.boxplot(data = df, y = 'bill\_length\_mm')

b) 이상치 파악

e.g. Q3 = df\['bill\_length\_mm'].quantile(0.75)

&#x09; Q1 = df\['bill\_length\_mm'].quantile(0.25)

&#x09; IQR = Q3 - Q1



&#x20; 	 upper = Q3 + 1.5 \* IQR

&#x20;        lower = Q1 - 1.5 \* IQR



&#x20;        df\['bill\_length\_mm'] > upper

&#x09; df\['bill\_length\_mm'] < lower

7\) 이상치 처리 : 너무 큰 이상치는 'upper\_fence',  작은 이상치는 'lower\_fence'로 대치

e.g. df.loc\[df\['bill\_length\_mm'] > upper, 'bill\_length\_mm'] = upper

&#x20;        df.loc\[df\['bill\_length\_mm'] < lower, 'bill\_length\_mm'] = lower 

8\) 범주 분포 확인 : 범주형 변수의 분포 확인

e.g. df.select\_dtypes('object').describe()

9\) 범주형 변수 처리 : 범주 개수(unique)가 지나치게 많은 열 제거

e.g. df = df.drop('고객 번호', axis = 1)

10\) 데이터 불균형 확인

e.g. df\['species'].value\_counts()

&#x09; df\['species'].value\_counts().plot(kind = 'bar')

