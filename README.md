# 한국인의 삶을 분석하고 파악하라
## 프로젝트 팀원: 송우석, 이성혁(5번완성)
## 분석하고자 하는 문제 정의 (가설)
> 1. 성별에 따라 월급이 다를까?
> 2. 몇 살 때 월급을 가장 많이 받을까?
> 3. 어떤 연령대의 월급이 가장 많을까?
> 4. 성별 월급 차이는 연령대별로 다를까?
> 5. 어떤 직업이 월급을 가장 많이 받을까?
> 6. 성별로 어떤 직업이 가장 많을까?
> 7. 종교가 있으면 이혼을 덜 할까?
> 8. 어느 지역에 노년층이 많을까?

## 1. 성별에 따라 월급이 다를까?
#### 실현 코드:
```
# 성별 변수 검토 및 전처리하기
# 1. 변수 검토하기
welfare['sex'].dtypes # 변수 타입 출력

# 2. 전처리하기
# 이상치 확인
welfare['sex'].value_counts() # 빈도 구하기

# 이상치 결측 처리
welfare['sex'] = np.where(welfare['sex'] == 9, np.nan, welfare['sex'])

# 결측치 확인
welfare['sex'].isna().sum()

# 성별 항목 이름 부여
welfare['sex'] = np.where(welfare['sex'] == 1, 'male', 'female')

# 월급 변수 검토 및 전처리하기
# 1. 변수 검토하기
welfare['income'].dtypes # 변수 타입 출력

welfare['income'].describe() # 요약 통계량 구하기

sns.histplot(data = welfare, x = 'income')

# 2. 전처리하기
welfare['income'].describe() # 이상치 확인

welfare['income'].isna().sum() # 결측치 확인

## 성별 월급 평균표 만들기
sex_income = welfare.dropna(subset = ['income']).groupby('sex', as_index = False).agg(mean_income = ('income', 'mean')) # income 평균 구하기

sex_income

# 빈도 구하기
welfare['sex'].value_counts()
## 성별 월급 평균표 만들기
sex_income = welfare.dropna(subset = ['income']).groupby('sex', as_index = False).agg(mean_income = ('income', 'mean')) # income 평균 구하기

sns.barplot(data = sex_income, x = 'sex', y = 'mean_income')
```
#### 히스토그램:
![income](https://github.com/proleesh/proleesh/assets/57159010/dada0a2b-58fe-485f-b8bf-27e22b909334)
#### 막대그래프:
![sex_income](https://github.com/proleesh/proleesh/assets/57159010/06c83ac7-78c5-49b6-82ea-f348204ef98b)
> 결론: 관계가 있다.

## 5. 어떤 직업이 월급을 가장 많이 받을까?
#### 실현 코드:
```
# 작업 변수 검토 및 전처리하기
# 1. 직업 변수 검토업 및 전처리하기
welfare['code_job'].dtypes # 변수 타입 출력

# 2. 전처리하기
# 이상치 확인
welfare['code_job'].value_counts() # 빈도수 확인

# 직업별 월급 평균 계산하기 (groupby함수 이용)
job_income_avg = welfare.groupby('code_job')['income'].mean()
# 작업별 월급 평균표 만들기
job_income = welfare.dropna(subset = ['income']).groupby('code_job', as_index = False).agg(mean_income = ('income', 'mean'))
print(job_income_avg)
# 월급이 가장 높은 직업 찾기
highest_income_job = job_income_avg.idxmax()

# 결과 출력
print("결론: 가장 많은 월급을 받는 직업코드는 :", highest_income_job)
# 히스토그램
sns.histplot(data = welfare, x = 'income')
# 막대그래프
sns.barplot(data = job_income, x = 'code_job', y = 'mean_income')
```
#### 히스토그램
![code_job](https://github.com/proleesh/Project0531/assets/57159010/ce88b3c4-cb1d-4aea-bf15-53d7b0f3b82d)
#### 막대그래프
![highest_job_code](https://github.com/proleesh/Project0531/assets/57159010/2bb89ea9-734a-4481-998e-f71a70d230f0)
> 결론: 직업코드: 241(의료 진료 전문가)이 가장 많이 받는다.
