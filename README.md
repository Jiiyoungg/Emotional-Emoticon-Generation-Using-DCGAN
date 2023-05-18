# Emotional-Emoticon-Generation-Using-DCGAN
DCGAN을 사용하여 텍스트의 감정을 반영하는 이모티콘 생성

프로젝트 의의
============
> 특정 포털사이트의 뉴스 서비스에서 감정 이모티콘을 모두 없애고 추천 스티커로 대체하는 것을 보고 소통 창구를 빼앗긴 느낌이 든다는 의견이 있었고, 그렇다면 악플을 방지하는 본래 취지를 살리면서 동시에 유저들의 의견을 반영할 수 있는 방안으로 [댓글의 감정을 반영하는 이모티콘 생성] 제시.



프로젝트 AtoZ 과정
------------
 * 포털사이트 댓글 감정분석


   1. 포털사이트의 댓글 크롤링


   2. 뉴스의 카테고리(정치, 경제, 사회, 생활/문화, IT/과학, 세계)별 댓글 감정분석
     * KoBERT를 사용하여 7가지의 감정(기쁨, 슬픔, 놀람, 분노, 공포, 혐오, 중립)으로 분류
     * 카테고리별 감정 분포 확인


   3. 비속어가 포함된 댓글 분류
     * 오분류된 댓글을 확인해본 결과, 비속어가 포함된 댓글이 대다수
       -> 추가적으로 이를 분류하기 위해 비속어 탐지 진행
     * FastText + Bidirectional GRU 사용


   4. LDA를 통한 감정별 논쟁 분석
     * 감정별로 주로 어떤 주제로 작성되었는지 분석
     * LDAvis 라이브러리 사용

 * DTW 클러스터링을 이용한 시간흐름에 따른 댓글 의견개진 분석


    1. Elbow method, Silhouette mothod, DB index를 이용하여 군집의 수 결정


    2. DTW 클러스터링 진행
      -> 그 결과, 다수의 의견은 계속해서 증가하지만, 
        소수의 의견은 침묵하는 "침묵의 나선이론" 현상 발견
        
        
  * **감정을 나타내는 이모티콘 생성**


     **1. 댓글 라벨링**
   
   
     **2. KoBERT를 사용하여 얻은 임베딩 벡터를 DCGAN에 노이즈 대신 입력**
   
   
       **-> '감정' 이라는 잠재요소 파악 기대**
      
    
진행중 발생한 문제점과 해결방안    
---------
*데이터 수가 충분하지 않음에 따라 판별자의 과적합 발생

* 해결방안
  1. Loss 개선
    * WGAN, WGAN-GP, LSGAN 등 개선된 모델 시도
  2. 기본 모델 조정
    * 버트의 CLS 토큰에 가우시안 노이즈 추가
    * 드롭아웃 비율 조정
    * BGenerator freezing 레이어 선택(+컴퓨팅 시간 줄이기)
    * 판별자에 넣기 전에 가우시안 노이즈 추가
    * 판별자의 중간 레이어에 가우시안 레이어 추가


이모티콘 예시   
---------
