# 주행 기록계 데이터 분석을 통한 주행 습관 분석 #
<br>

주행 습관 분석은 운전자 및 차량 안전 향상을 위해 필수적인 요소임

본 연구에서는 주행 기록계 데이터를 바탕으로 운전자의 주행 습관을 분석 및 평가하고자 함

이를 수행하기 위해 다음과 같은 문제점을 해결해야 함
- 방대한 주행 기록계 데이터에서 중요 요인(급감속, 급회전 등) 파악하는 것
- 데이터를 분석 및 점수화에 용이한 형태로 변환하는 것
- 변환된 데이터를 분석하여 주행 습관 분류하는 것

본 연구는 'A frame work for evaluating aggresive driving behaviors based on in-vehicle driving records. Jooyoung Lee, Kitae Jang, 2017' 기반으로 실험을 진행함
<br>
<br>

---
<br>

<h2> 1. 데이터 분석 및 전처리 </h2>
경기 71바 1001 차량을 비롯한 15대 차량의 210701 일시 DTG(Digital Tacho Graph) 데이터 활용
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230720810-14f7c655-f532-4780-8e4f-c154c3a5d54f.png">

<br>

주요 시계열 데이터로 속도, 가속도, 요레이트 사용
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230721213-fb012b31-0e79-47e9-8695-602aaf0323a7.png">

<br>
<br>

<h2> 2. 급격한 주행 변화 감지 </h2>
uLSIF : 두개의 동일한 크기의 연속된 시간 데이터에서 두 분포의 밀도 비율 기반 차이를 사용하여 분포의 불일치도(dis-simmilarity) 계산

<br>
<br>

<img width="400" alt="image" src="https://user-images.githubusercontent.com/53384975/230721577-f53238f9-712b-4167-a2e7-2c16ec6fe06d.png">

<br>

앞뒤 15초 간 데이터(속도, 가속도, 요레이트)의 밀도 비 차이를 이용한 불일치도 측정
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230721801-248ad449-ecde-4481-9f92-f103fe73f39e.png">

<br>

불일치도 상위 5% 해당하는 값들을 위험 운전 시발점으로 정의

해당 지점으로부터 15초 간 속도, 가속도, 요레이트 데이터 추출 -> 위험 운전 상황(Driving Event)으로 정의
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230721838-33238981-cf4c-4d89-96f7-e20e398a0320.png">

<br>
<br>

<h2> 3. 데이터 차원축소 </h2>
Sparse Autoencoder : 입력값과 출력값의 차이를 학습해 입력값의 정보를 보존하면서 입력값을 더 낮은 차원으로 변환

  - Driving Event 군집화를 위한 차원축소
 
  - 15x3 입력 차원을 30개의 인코딩 차원으로 축소
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230722286-9aedc859-032d-40ca-8d0f-e8da770fb72b.png">

<br>
<br>

<h2> 4. 군집화 </h2>
K-means Clustering : 데이터를 K개의 군집으로 그룹화하여 비슷한 특성을 가지는 데이터끼리 묶음

  - 정돈되지 않은 위험 운전 데이터를 유형 별로 나누기 위한 군집화

  - K개의 군집은 K개의 서로 다른 위험 운행 유형을 의미
<br>
<br>

<img width="700" alt="image" src="https://user-images.githubusercontent.com/53384975/230723099-f6aac6c8-8c0e-4c2f-b332-1b4f07c5296f.png">

<br>
<br>

<h2> 5. 평가지표 </h2>
평가지표 삽입

  - Velocity Evalution : Low/Mid/High

  - Acceleration Safety : safety/danger

  - Steering Safety : safety/danger
<br>

평가지표 별 할당된 점수 곱하여 군집의 위험도 점수 계산
<br>
<br>

<img width="500" alt="image" src="https://user-images.githubusercontent.com/53384975/230723249-7a199dfb-7ffe-4665-aecc-ad7bcf2343fc.png">

<br>
<br>

<h2> 6. 운전자 평가 </h2>

- 새 운전자의 DTG 데이터 이용한 Driving Event 추출

- 해당 Driving Event가 속해있는 군집들에 대해 위험도 점수 합산

- 총 운행 거리로 나누어 단위 거리 당 위험도를 최종 점수로 반환
<br>
<br>

<img width="500" alt="image" src="https://user-images.githubusercontent.com/53384975/230723464-363d9d72-fdf1-4e68-a843-f75c54dde572.png">

<br>
<br>

<h2> 7. 결과 </h2>
평가지표를 기존 교통통안전공단 운전자 분석 결과와 비교
<br>
<br>

<img width="500" alt="image" src="https://user-images.githubusercontent.com/53384975/230723493-d18cc188-53b2-4451-bc7b-24ecd9d7f043.png">
