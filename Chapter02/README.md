# Chapter 02. 문장을 작은 단위로 쪼개기

### 2-1 토큰화란?

- 토큰화(tokenization): 문장을 토큰 시퀀스로 나누는 과정.

    - 수행 대상에 따라 문자, 단어, 서브워드 등 세 가지 방법이 있음.

    - 토큰화를 수행하는 프로그램을 토크나이저(tokenizer)라고 한다.

    - 토크나이저는 토큰화 뿐만 아니라 품사 부착(Part-of-Speech tagging)까지 수행하므로 토큰화 개념을 넓은 의미로 해석할 때는  
    토큰 나누기에 품사 부착까지 일컫는 경우도 종종 있다.


- 단어 단위 토큰화
    - 단어(어절) 단위로 토큰화를 수행할 수 있다.
        ```
        예제
        입력 : 어제 카페 갔었어

        토큰화 결과 : 어제, 카페, 갔었어
        ```
    - 공백으로 분리하면 별도로 토크나이저를 쓰지 않아도 된다는 장점이 있지만, <span style="font-weight:bold; color:tomato">어휘 집합(vocabulary)</span>의 크기가 매우 커질 수 있다.
    
    - 보통 언어 하나로 모델을 구축할 때 어휘 집합 크기는 10만 개를 훌쩍 넘는 경우가 다반사.
    - 어휘 집합 크기가 커지면 그만큼 모델 학습이 어려워질 수 있다.

- 문자 단위 토큰화
    - 토큰화를 단어 대신 문자 단위로 고려해 볼 수 있다.
    - 문제점 : 문자 토큰은 의미있는 단위가 되기 어렵다.  
        ex) 어제의 어와 어미 어의 구분이 사라질 수 있다.  
        언어 모델에 입력할 토큰 시퀀스가 길면 모델이 해당 문장을 학습하기가 아려워지고 결과적으로 성능이 떨어지게 된다.

- 서브워드 단위 토큰화
    - 서브워드(subword)단위 토큰화는 단어와 문자 단위 토큰화의 중간에 있는 형태
        - 어휘 집합 크기가 지나치게 커지지 않으면서도 미등록 토큰 문제를 피하고, 분석된 토큰 시퀀스가 너무 길어지지 않게 한다.

---
#### 2-2 바이트 페어 인코딩
- Byte Pair Encoding(바이트 페어 인코딩) : 원래 정보를 압축하는 알고리즘으로 제안되었지만, 최근에는 자연어 처리 모델에 널리 쓰이는 토큰화 기법

- GPT 모델은 BPE 기법으로 토큰화를 수행하며, BERT 모델은 BPE와 유사한 워드피스(wordpiece)를 토크나이저로 사용

- BPE란
    - 데이터에서 가장 많이 등장한 문자열을 병합해서 데이터를 압축하는 기법
        ```
        aaabdaaabac
        ```
    - 데이터에 등장한 글자(a, b, c, d)를 초기 사전으로 구성, 연속된 두 글자를 한글자로 병합.
    - aa가 많이 등장하니 이를 Z로 병합하면 문자열은
        ````
        ZabdZabac
        ````
    - 그 다음 ab가 많이 등장하니 이를 Y로 병합하면
        ```
        ZYdZYac
        ```
    - ZY도 많이 등장하니 이를 X로 병합하면
        ```
        XdXac
        ```
    - BPE 수행 이전에는 사전 크기가 4개(a, b, c, d)였지만 수행 이후 크기가 7개(a, b, c, d, Z, Y, X)로 늘었다.  
    반면 데이터 길이는 11에서 5로 줄었다.

    - BPE는 사전의 크기를 지나치게 늘리지 않으면서도 데이터길이를 효율적으로 압축 할 수 있다.

    - BPE를 활용한 토큰화 절차
        - 어휘 집합 구축 : 자주 등장하는 문자열을 병합하고 이를 어휘 집합에 추가.  
        이를 원하는 어휘 집합 크기가 될 때까지 반복

        - 토큰화 : 토큰화 대상 문장의 각 어절에서 어휘 집합에 있는 서브워드가 포함되었을 때 해당 서브워드를 어절에서 분리.

- BPE 어휘 집합 구축
    - 어휘 집합을 만들기 위해 먼저 말뭉치가 필요.
    - 말뭉치의 모든 문장을 공백으로 나눠야 함.  
    -> 이를 프리토크나이즈(pre-tokenize)라고 함.
    - BPE 어휘 집합은 고빈도 바이그램 쌍을 병합하는 방식으로 구축.

- 워드피스
    - wordpiece는 말뭉치에서 자주 등장한 문자열을 토큰으로 인식한다는 점에서 BPE와 유사함.  
     하지만, 어휘 집합을 구축할 때 문자열을 병합하는 기준이 다름.
    
    - 병합했을 때 말뭉치의 우도(likelihood)를 가장 높이는 쌍을 병합.

---
#### 2-3 어휘 집합 구축
- 코드 참조

---

#### 2-4 토큰화 하기