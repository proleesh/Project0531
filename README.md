# 한국인의 삶을 분석하고 파악하라
## 프로젝트 팀원: 송우석, 이성혁
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
![income](https://github.com/proleesh/proleesh/assets/57159010/dada0a2b-58fe-485f-b8bf-27e22b909334)
![sex_income](https://github.com/proleesh/proleesh/assets/57159010/06c83ac7-78c5-49b6-82ea-f348204ef98b)
> 결론: 관계가 있다.