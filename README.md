# 통계데이터 인공지능 활용대회
### 자연어 기반 인공지능 산업분류 자동화
자연어 기반의 통계데이터를 인공지능으로 자동 분류하는 기계학습 모델 발굴

## 데이터프레임
전국사업체조사 자료 데이터
- 일련번호(AI_id), 산업대분류(digit_1), 산업중분류(digit_2), 산업소분류(digit_3), 사업 대상(text_obj), 사업 방법(text_mthd), 사업 취급품목(text_deal)

![image](https://github.com/teamgaon/SANUP/blob/main/pic/KakaoTalk_20220413_134348181.png)

- text 컬럼 생성
  - 토크나이징을 위해 사업 대상(text_obj), 사업 방법(text_mthd), 사업 취급품목(text_deal) 병합
  
![image](https://github.com/teamgaon/SANUP/blob/main/pic/6.png)

## 데이터 전처리
> ### [hanspell](https://github.com/ssut/py-hanspell)
> - 불용어 처리 후, 네이버 맞춤법 검사기를 통해 오탈자, 띄어쓰기 전처리
>
> ![image](https://github.com/teamgaon/SANUP/blob/main/pic/%EB%B0%94%EA%BE%BC%20%EA%B2%83.png)
>
> - hanspell이 잘못 처리한 띄어쓰기 수정  
>   - 도 매 -> 도매
 
> ### 토크나이저[(KoELECTRA)](https://github.com/monologg/KoELECTRA)
> - 모델 학습에 필요한 문장 최대 길이 계산  
>   - max_length=64
> 
> ![image](https://github.com/teamgaon/SANUP/blob/main/pic/KakaoTalk_20220413_134348181.png)


>
