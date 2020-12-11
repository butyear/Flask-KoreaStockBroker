# **Flask-KoreaStockBroker**

증권사 API를 Flask 기반으로 제작하여

범용 환경에서 트레이딩 할 수 있도록 한다.

(현재는 대신증권만 지원합니다.)


* * *

## **사용방법**

1. 32비트 가상환경을 생성합니다.

2. 의존성을 설치합니다.
    > pip install -r requirements.txt

3. CreonPlus를 실행합니다.

4. config.cfg 파일에 정보를 입력합니다.
 
        [BROKER]
        
        BROKER_NAME = DAISHIN
        
        BROKER_ID = (대신증권 ID)
        
        BROKER_PW = (대신증권 비밀번호)
            
        CERT_PW = (공인인증 비밀번호)

6. main.py 파일을 실행합니다.
    > python main.py


* * *


## **API 명세**

1. ### 로그인 조회

    - endpoint : /connection

    - method : GET

    - argument
        - None.

    - result (Example)
        > http://127.0.0.1:5000/connection

            "broker": "DAISHIN",
            
            "res": "연결되었습니다.",
            
            "status": 200

2. ### 계좌 조회

    - endpoint : /accountinfo

    - method : GET

    - argument
        - None.

    - result (Example)
        > http://127.0.0.1:5000/accountinfo

            "name": (계좌 사용자 이름)

            "profit_amount": 0,

            "tot_amount": 0,

            "twoday_amount": (D+2 예수금)       

3. ### 차트 데이터 조회

    - endpoint : /chart

    - method : GET
    
    - argument
        - code : 종목코드(Axxxxxx, Qxxxxxx)
        - n : 조회갯수
        - date_from : 조회시작날짜
        - date_to : 조회종료날짜(default : 당일)

    - result (Example)
        > http://127.0.0.1:5000/chart?code=A233740&n=20

        > http://127.0.0.1:5000/chart?code=A233740&date_from=20200501

        > http://127.0.0.1:5000/chart?code=A233740&date_from=20200501&date_to=20200701

            "index": 0,
            "date": 20201127,
            "open": 12900,
            "high": 13490,
            "low": 12900,
            "close": 13395,
            "volume": 32898683,
            "vol_amount": 437723000000

4. ### 종목정보 조회

    - endpoint : /stockfeatures

    - method : GET
    
    - argument
        - code : 종목코드(Axxxxxx, Qxxxxxx)

    - result (Example)
        > http://127.0.0.1:5000/stockfeatures?code=A207940

            "이름": "삼성바이오로직스",
            "증거금률(%)": 40,
            "시장구분코드 ": 1,
            "부구분코드": 1,
            "감리": 0,
            "관리": 0,
            "현재상태": 0,
            "결산기": 12,
            "K200여부": 10,
            "업종코드": 1,
            "상장일": 20161110,
            "신용가능여부": 1,
            "PER": 148.76,
            "EPS": 5371,
            "자본금(백만)": 165412,
            "액면가": 2500,
            "배당률": 0.0,
            "배당수익률": 0.0,
            "부채비율": 35.76,
            "유보율": 2532.48,
            "자기자본이익률(ROE)": 0.0,
            "매출액증가율": 30.94,
            "경상이익증가율": -48.7,
            "순이익증가율": -9.46,
            "투자심리": 0.0,
            "매출액": 701591,
            "경상이익": 155435000000,
            "당기순이익": 202904000000,
            "BPS": 67994,
            "영업이익증가율": 64.77,
            "영업이익": 91742000000,
            "매출액영업이익률": 13.08,
            "매출액경상이익률": 22.15,
            "이자보상비율": 3.58,
            "분기BPS": 67994,
            "분기매출액증가율": 103.33,
            "분기영업이익증가율": 0.0,
            "분기경상이익증가율": 0.0,
            "분기순이익증가율": 0.0,
            "분기매출액": 789470,
            "분기영업이익": 200210000000,
            "분기경상이익": 189265000000,
            "분기당기순이익": 144753000000,
            "분개매출액영업이익률": 25.36,
            "분기매출액경상이익률": 23.97,
            "분기ROE": 4.36,
            "분기이자보상비율": 16.07,
            "분기유보율": 2619.77,
            "분기부채비율": 36.42,
            "최근분기년월": 202009

5. ### 공매도정보 조회
    
     - endpoint : /short

     - method : GET
     
     - argument
       - code : 종목코드(Axxxxxx, Qxxxxxx)

     - result (Example)
        > http://127.0.0.1:5000/short?code=A233740
        
            "index": 0,
            "거래일": 20201127,
            "종가": 799000,
            "전일대비": 0,
            "거래량": 88639,
            "공매도량": 25,
            "공매도비중": 0.0282,
            "공매도거래대금": 2007

6. ### 다수종목 조회 (CYBOS MarketEye)
      - endpoint : /marketeye
  
      - method : GET
  
      - argument
        - code : 종목코드  
        - &code=(종목코드)&code=(종목코드) 형식으로 입력
  
      - result(Example)
         > http://127.0.0.1:5000/marketeye?code=A233740&code=A251340
            
            [
                "종목코드": "A233740",
                "종목명": "KODEX 코스닥150",
                "시간": 1559,
                "현재가": 13315,
                "시가": 13510,
                "고가": 13695,
                "저가": 13315,
                "매도호가": 13315,
                "매수호가": 13310,
                "거래량": 31637099,
                "거래대금": 427861610000,
                "총매도호가잔량": 90912,
                "총매수호가잔량": 180863,
                "최우선매도호가잔량": 8689,
                "최우선매수호가잔량": 15692 
            ],
            [
                "종목코드": "A251340",
                "종목명": "KODEX 코스닥150",
                "시간": 1559,
                "현재가": 5025,
                "시가": 4995,
                "고가": 5025,
                "저가": 4960,
                "매도호가": 5030,
                "매수호가": 5025,
                "거래량": 58000829,
                "거래대금": 289614330000,
                "총매도호가잔량": 891209,
                "총매수호가잔량": 612776,
                "최우선매도호가잔량": 138613,
                "최우선매수호가잔량": 129132
            ]

7. ### 매수
    
     - endpoint : /buy

     - method : GET
     
     - argument
       - acc : 계좌번호
       - front : 종목 앞 구분코드(A, Q)
       - code : 종목코드 숫자
       - amount : 수량
       - price : 주문가격

     - result (Example, 모의투자로 테스트)
        > http://127.0.0.1:5000/buy?acc=(계좌번호)&front=A&code=233740&amount=1&price=13900
        
            {
                "Server_Status": 200, 
                "cybos_status": 0, 
                "message": "0040 모의투자 매수 주문이 완료되었습니다.(ordss.cststkord)"
            }


8. ### 매도
    
     - endpoint : /sell

     - method : GET
     
     - argument
       - acc : 계좌번호
       - front : 종목 앞 구분코드(A, Q)
       - code : 종목코드 숫자
       - amount : 수량
       - price : 주문가격

     - result (Example, 모의투자로 테스트)
        > http://127.0.0.1:5000/sell?acc=(계좌번호)&front=A&code=233740&amount=1&price=13900
        
            {
                "Server_Status": 200, 
                "cybos_status": 0, 
                "message": "0039 모의투자 매도 주문이 완료되었습니다.(ordss.cststkord)"
            }
    

9. ### 매매입체분석 (대신증권 매매입체분석)
     - [대신증권 투자주체별 현황(7254) 참고링크](https://m.blog.naver.com/songtong/220941944898)
     - endpoint : /tradematrix

     - method : GET
     
     - argument
       - code : 종목코드 

     - result (Example, 모의투자로 테스트)
        > http://127.0.0.1:5000/tradematrix?code=A005930
            
            {
                "날짜": 20200615,
                "종가": 49900,
                "개인": 5371193,
                "외국인": -1557410,
                "기관계": -3866544,
                "금융투자": -2058778,
                "보험": -100283,
                "투신": -298496,
                "은행": -8769,
                "기타금융": 44549,
                "연기금등": -1101531,
                "기타법인": 35781,
                "기타외인": 16980,
                "사모펀드": -343236,
                "국가지자체": 0
            },
            {
                "날짜": 20200616,
                "종가": 52100,
                "개인": 4326244,
                "외국인": -3684032,
                "기관계": -756223,
                "금융투자": 459079,
                "보험": -157813,
                "투신": -298776,
                "은행": -10871,
                "기타금융": 64111,
                "연기금등": -407092,
                "기타법인": 101648,
                "기타외인": 12363,
                "사모펀드": -404861,
                "국가지자체": 0
            }, 

            (후략, 6개월치 데이터가 불러와집니다.) 

10. ### 호가잔량 호출
     - endpoint : /hogainfo

     - method : GET
     
     - argument
       - code : 종목코드 
       - k : 호가 단수 

     - result (Example) 
        > http://127.0.0.1:5000/hogainfo?code=A233740&k=5
        
        > KODEX 코스닥150 레버리지의 5단 매수, 매도호가 및 잔량현황 호출 

            [
                {
                    "매수매도여부": "매수",
                    "호가": 15555,
                    "잔량": 2660
                },
                {
                    "매수매도여부": "매수",
                    "호가": 15550,
                    "잔량": 1205
                },
                {
                    "매수매도여부": "매수",
                    "호가": 15545,
                    "잔량": 589
                },
                {
                    "매수매도여부": "매수",
                    "호가": 15540,
                    "잔량": 1057
                },
                {
                    "매수매도여부": "매수",
                    "호가": 15535,
                    "잔량": 1956
                },
                {
                    "매수매도여부": "매도",
                    "호가": 15560,
                    "잔량": 72926
                },
                {
                    "매수매도여부": "매도",
                    "호가": 15565,
                    "잔량": 1997
                },
                {
                    "매수매도여부": "매도",
                    "호가": 15570,
                    "잔량": 6828
                },
                {
                    "매수매도여부": "매도",
                    "호가": 15575,
                    "잔량": 3759
                },
                {
                    "매수매도여부": "매도",
                    "호가": 15580,
                    "잔량": 14508
                }
            ]

        
* * *

## **주요 사항**
1. 이 프로그램을 사용하여 주식 종목을 매수, 매도함으로서 발생하는 수익과 손실에 대한 책임은 운용자에게 있습니다.
2. BSD 3-Clause License를 준수합니다.
3. Endpoint /hogainfo?code=(종목코드) 를 이용하여 10단 호가현황을 불러올 수 있으나, 
   
   실매매에서 테스트되지 않아 사용을 추전하지 않습니다.
* * *

## **참고 글**
 [퀀티랩 - 대신증권 크레온(Creon) HTS 브리지 서버 만들기 (Flask 편)](http://blog.quantylab.com/creon_hts_bridge.html)

* * *
