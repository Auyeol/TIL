금융감독원 사이트 내 API인 **금융상품통합비교공시 API**의 정기예금 API를 활용하여 데이터를 가공, 딕셔너리를 생성하였다.

```python
def get_deposit_baseLists():
    # 본인의 API KEY 로 수정합니다.
    api_key = "..."
    url = 'http://finlife.fss.or.kr/finlifeapi/depositProductsSearch.json'
    params = {
     'auth': api_key,
     'topFinGrpNo': '020000',
     'pageNo': 1
  }
    # 요구사항에 맞도록 이곳의 코드를 수정합니다.
    response = requests.get(url, params=params).json()
    baseList = response['result']['baseList']   
    optionList = response['result']['optionList'] 
    result = []        

    for li in range(len(baseList)):
        new_dict = {}
        new_dict['금융상품명'] = baseList[li]['fin_prdt_nm']
        new_dict['금융회사명'] = baseList[li]['kor_co_nm']
        make_list = []

        for lis in range(len(optionList)):
            in_new_dict = {}
            if baseList[li]['fin_prdt_cd'] == optionList[lis]['fin_prdt_cd']:
                in_new_dict['저축 금리'] = optionList[lis]['intr_rate']
                in_new_dict['저축 기간'] = optionList[lis]['save_trm']
                in_new_dict['저축금리유형'] = optionList[lis]['intr_rate_type']
                in_new_dict['저축금리유형명'] = optionList[lis]['intr_rate_type_nm']
                in_new_dict['최고 우대금리'] = optionList[lis]['intr_rate2']
                make_list.append(in_new_dict)
        new_dict['금리정보'] = make_list

        result.append(new_dict)
        
    return result

# 금리 정보 안에 저축 금리, 저축 기간, 저축금리유형, 저축금리유형명, 최고 우대금리를 딕셔너리로 넣기 
# 그 밖에 금융상품명, 금융회사명 딕셔너리 

if __name__ == '__main__':
    # json 형태의 데이터 반환
    result = get_deposit_baseLists()
    # prrint.prrint(): json 을 보기 좋은 형식으로 출력
    pprint.pprint(result)

```




--------
# 그 외 정보

---------
## 서버

- 부탁을 받으면 처리해주거나, 사용자의 요구대로 원하는 값을 돌려주는 역할

## 클라이언트

- 정보를 서버에 요청

ex) 네이버 주소 입력 → 

   네이버 메인화면 요청(클라이언트) ←→ 요청받은 네이버 메인 화면 제공(서버)

<aside>
❓ 클라이언트는 어떻게 요청을 서버에 보낼 수 있을까?

</aside>

1. 웹 브라우저(크롬)을 켜서 주소창에 주소(URL)을 입력 
2. 서버에 정보를 요청하는 파이썬 코드를 작성한다. 
    
    ```python
    import requests
    
    url = 'https://fakestoreapi.com/carts' # 요청을 보낼 서버 url
    data = request.get(url).json() 
    # 해당 서버(url)에 데이터를 달라고 요청을 보내는 함수
    # .json() -> 내부의 데이터를 JSON 형태로 변환해주는 함수 
    print(data)
    ```
    
    - 키 값이 ‘weather’인 데이터를 위에서 선언한 `data` 를 통해 가져온다고 하였을 때
        - `data['weather']`과 `data.get('weather')`의 차이점
        - 두 방법은 모두 딕셔너리에서 특정 키의 값을 가져오는 방법이다.
        - `data['weather']` 을 사용시, 딕셔너리 내에 해당 키가 없을 경우 KeyError가 발생하기 때문에 해당 키가 반드시 존재하거나, 키가 존재하지 않는 경우에 대한 예외 처리가 필요하다.
            
            ```json
            data = {'temperature': 25, 'condition': 'Sunny'}
            
            try:
                weather = data['weather']  # KeyError 발생
            except KeyError:
                weather = 'Unknown'
            ```
            
        - `get()` 메서드는 딕셔너리 내에 해당 키가 없으면 None을 반환하며, KeyError가 발생하지 않기에 예외 처리를 할 필요가 없으며, 기본값을 설정할 수 있다. 따라서, `get()` 메서드를 사용하게 되면 코드가 좀 더 간결해지고 안정성이 높아지게 된다.
            
            ```json
            data = {'temperature': 25, 'condition': 'Sunny'}
            
            weather = data.get('weather')  # None 반환
            
            default_weather = data.get('weather', 'Unknown')  # 기본값 설정 가능
            ```
            

[이벤트 종류]

connection - 클라이언트가 접속하여 연결이 만들어질 때 발생하는 이벤트

request - 클라이언트가 요청할 때 발생하는 이벤트

close - 서버를 종료할 때 발생하는 이벤트 

### “클라이언트 → 서버”로 데이터가 전송되는 4가지 주 예시

## [나중에 HTML,CSS 배울 때 자세히 보기]

- 정적 데이터 조회(이미지, 정적 텍스트 문서)
- 동적 데이터 조회(주로 검색, 게시판 목록에서 필터(검색어)를 이용한 정렬
- HTML Form을 통한 데이터 전송(회원 가입, 상품 주문, 데이터 변경)
- HTTP  API를 통한 데이터 전송(회원 가입, 상품 주문, 데이터 변경)
    
    서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax) 
    

<aside>
❓ 요청을 보내는 클라이언트는 매우 다양하다.  (크롬방식, 파이썬 방식, ….) 
이 클라이언트들은 각자 다른 방식으로 요청을 보낼 것이다. 그렇다면 서버는 이 다른 방식들을 어떻게 모두 해석할 수 있는걸까?

</aside>

**API**가 서버와 클라이언트 사이에서 중간다리 역할로 해석할 수 있게 도와줌(?)

**클라이언트로부터 요청이 왔을 때 그에 해당하는 API를 통해 서버로 전송** 

### API

- 클라이언트가 원하는 기능을 수행하기 위해서 서버 측에 만들어 놓은 프로그램
- JSON 데이터 형식을 사용
    - `{}`로 둘러싸인 키-값 쌍의 집합으로 표현됨
    - 키 = 문자열 / 값 = 다양한 데이터 유형 가능
    - 값은 쉼표(,)로 구분된다
        
        ```json
        {
        	"name" : "김싸피",
        	"age" : 27,
        	"city" : "서울",
        	"hobbies" : [    // 여러 값을 넣을 시, 대괄호 사용(?)
        			"공부하기",
        			"복습하기"
        	],
        	"isStudent":true
        }
        // 파싱(Parsing): 데이터를 의미 있는 구조로 분석하고 해석하는 과정 
        ```
        
- 서버 측에 특정 주소로 요청이 오면 정해진 기능을 수행하는 API를 미리 만들어 둔다.
    - 클라이언트는 서버가 미리 만들어 놓은 주소로 요청을 보낸다.
    
![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/010c7dfc-87a8-4e3d-bf7a-792a497c16a0/37269107-f12d-458e-afbf-efd609d34383/Untitled.png)

### OpenAPI

- 외부에서 사용할 수 있돌고 무료로 개방(OPEN)된 API
- 악성 사용자가 고의로 많은 요청을 보냈을 때 서버의 과부화를 막기 위해 `API KEY`를 활용
- 서버에 요청할 때 마다 발급된 클라이언트는 발급된 `API KEY`를 함께 보내 정상적인 사용자 인 것을 확인 받는다.
- 일부 오픈 API는 사용량이 제한되어 있다. 따라서, 공식 문서의 사용량 제한을 확인하여야 한다.