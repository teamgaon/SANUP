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
### [hanspell](https://github.com/ssut/py-hanspell)
> - 불용어 처리 후, 네이버 맞춤법 검사기를 통해 오탈자, 띄어쓰기 전처리
>
> ![image](https://github.com/teamgaon/SANUP/blob/main/pic/%EB%B0%94%EA%BE%BC%20%EA%B2%83.png)
>
> - hanspell이 잘못 처리한 띄어쓰기 수정  
>   - 도 매 -> 도매
 
### 토크나이저[(KoELECTRA)](https://github.com/monologg/KoELECTRA)
> - 모델 학습에 필요한 문장 최대 길이 계산  
>   - max_length=64
> 
> ![image](https://github.com/teamgaon/SANUP/blob/main/pic/KakaoTalk_20220413_134348181.png)

## 모델링
### KoELECTRA
BERT의 학습 방식을 바꾸어서, 좀 더 가벼우면서도 성능을 개선한 모델 중 하나이다.
***
- KoBERT
  - 사전(Vocabulary)
    - 크기 : 8,002
    - 한글 위키 기반으로 학습한 토크나이저(SentencePiece)
    - Less number of parameters(92M < 110M )
- KoELECTRA  

|     | Vocab Len | do_lower_case |
| --- | --------: | ------------: |
| v1  |     32200 |         False |
| v2  |     32200 |         False |
| v3  |     35000 |         False |

  - v1, v2의 경우 약 14G Corpus (2.6B tokens)를 사용 (뉴스, 위키, 나무위키)
  - v3의 경우 약 20G의 모두의 말뭉치를 추가적으로 사용 (신문, 문어, 구어, 메신저, 웹)
***
KoELECTRA의 vocab size가 KoBERT의 약 4배이므로 정확도가 향상될 것으로 판단

#### StratifiedKFold
- Kfold ensemble
  - fold = 5
#### Optimizer
- AdamW
#### Learning rate scheduler
- Cosine annealing with warmup

1. 첫 번째 모델
> - digit_1, digit_2, digit_3를 병합하여 target으로 예측
> ![image](https://github.com/teamgaon/SANUP/blob/main/pic/7.png)
>   - train acc 0.93
>   - test acc 0.92

2. 두 번째 모델
> - digit_1, digit_2, digit_3를 각각 target으로 예측
>   - digit_1
>     - train acc 0.99
>     - test acc 0.98
>   - digit_2
>     - train acc 0.97
>     - test acc 0.96
>   - digit_3
>     - train acc 0.93
>     - test acc 0.92
>     
