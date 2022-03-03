# :pushpin: Impact museum
- 사회적 가치를 추구하는 소셜벤처 이커머스 플랫폼
- url...

<br>

## 1. 제작 기간 & 참여 인원
- 2021년 1월 14일 ~
- 개인 프로젝트
<br>

## 2. 사용 기술
**Back-end**       => 해당 기술을 왜 사용했는지 이유 & 그리고 그 기술 버전을 왜 사용했는지 이유 필요!
- Python 3.9.1 
- Django 3.2.9
- DRF(Django Rest Framework) 3.13.1 (이 버전 맞는지 다시 확인 필요)
<br>

## 3. ERD 설계
<img src="https://user-images.githubusercontent.com/95380638/150262590-29403524-27cf-4329-8733-5a75ca70a8f8.png">

- Customer 모델은 User 모델과 1:1관계로 설정
- Order 모델은 Customer 모델과 1:N관계로 설정 - 1명의 사용자가 쇼핑몰에서 여러 번 주문할 수 있음
- Shipping Address 모델은 Customer 모델 & Order 모델과 1:N관계로 설정 - 1명의 사용자가 다양한 배송 주소지를 생성할 수 있고, 1개의 주문 건이 배송 취소 및 실패 등으로 다양한 배송 주소지를 가질 수 있음
- Order Item 모델(장바구니 기능을 생각하기)은 Product 모델 & Order 모델과 1:N관계로 설정 - 1개의 상품이 여러 번 장바구니에 포함될 수 있고, 1개의 주문 건에 많은 상품들이 장바구니에 추가될 수 있음
<br>

## 4. 핵심 기능     
- 이 서비스의 핵심 기능은 ...입니다.     => 해당 기능을 왜 구현하려고 했는지 이유 필요!
<details>
  <summary>핵심 기능 설명 펼치기</summary>
  
  4.1 상품 정보에 관련된 기본적인 CRUD 구축
  
  4.2 DRF(Django Rest Framework)를 기반으로 CRUD API 서버 구축
  
  4.3 URL Configuration으로 app별 URL 관리
  
  4.4 Template & Static Inheritance 설정 완료 및 app별 namespacing 설정 완료
  
  4.5 DB 구성 및 ERD 설계 완료(이미지 저장 및 랜더링 가능)
  
  4.6 상품 1개 조회 페이지에서 장바구니 버튼 클릭 시, 장바구니 페이지에 추가 및 DB 저장 
  
  4.7 모델 내 필드 추가 및 속성 변경      
      - Customer 모델 : user 필드 null, blank = False로 변경 / name 필드 null = False로 변경 (user와 name필드가 비어 있으면 안되기 때문)       
      - Post(Product) 모델 : price 필드를 floatField에서 PositiveIntegerField로 변경 (가격이니까 실수보다는 양의 정수로 수정하는 게 맞다고 판단) / created_at 필드는 DateTimeField에 auto_now_add=True 옵션을 추가. (상품 데이터가 언제 생성되었는지 필요하기 때문.) / updated_at 필드를 새로 생성하고 DateTimeField의 auto_now=True 옵션을 추가. (상품 데이터를 수정했을 때 기록을 남기기 위해서 날짜가 갱신되어야 한다.)       
      - OrderItem 모델 : quantity 필드가 원래 null=True, blank=True 였으나 null=False, blank=False로 수정. (수량은 비어있으면 안되고 최소 1개로 설정되어야 하기 때문)
  
  2.1 IPython 8.0.1 설치
      - 설치한 이유는, 문법에 따라서 색상으로 강조를 해줘서 기존의 장고 연동 shell보다 작성하기 편함. 그리고 여러 줄에 걸쳐서 코드 입력 후, 위쪽 화살표로 전체 코드를 다시 불러오기가 편리함.
  
  2.1 Admin 페이지 개선
      - admin.py 에서 admin모듈의 ModelAdmin 클래스를 상속받아서 모델 클래스를 정의하고, register 장식자를 이용해 admin에 등록
      - 그리고 나서 클래스의 list_display 속성을 추가하여 -> admin페이지에 모델 별 필드를 표시해서 모델 데이터를 한눈에 파악할 수 있도록 개선
  
  2.2 (1) Admin 페이지 개선
          - admin.py에서 mark_safe 함수를 사용해서 Post 모델 객체의 이미지 url를 admin 페이지에서 볼 수 있도록 이미지으로 표시     
          - settings.py에서 MEDIA_URL과 MEDIA_ROOT를 수정하고 models.py에서 Post 모델 ImageField에 upload_to 속성을 설정 / posts앱의 post모델 디렉터리 밑에 년/월/일 디렉터리로 구분하여 더 깔끔하게 관리할 수 있도록 설정.
  
  2.2 (2) 메인 화면에서 검색 기능 구현
          - index.html에서 form element를 사용 / action attribute를 비워두어서 현재 form이 있는 같은 주소로 URL를 request하게 되고 Input element에 name를 query로 설정해서 값을 전송하고 / value도 query로 설정해서 검색한 이후에도 값이 남아있게끔 설정       
          - View index 함수에서 query라는 변수를 지정하고 GET 방식으로 들어온 query라는 이름으로 담긴 값을 담아준다. 그리고 query가 있을 때(검색했을 때) Post 모델 전체를 조회한 posts 변수를 다시 정의해서, filter를 통해 검색한 값이 포함되는 데이터로 설정. Q 함수를 import 해서 제품명 또는 브랜드명을 검색할 수 있도록 설정.
  
  2.4 상품 목록을 보여주는 메인 페이지 상품 개수 수정
      - index View 함수에서 posts = Post.objects.all().order_by('-id')[:8] -> 이렇게 전체 Post 데이터에서 id필드를 기준으로 역순처리하고, 8개의 데이터만 가져오기.
      - 그래서 index 메인 페이지에는 DB에 그 이상의 데이터가 있을지라도, 8개의 상품 목록만 항상 보여줄 수 있도록 설정 완료.
  
  2.41 bootstrap의 navbar를 이용해서 반응형 웹사이트가 될 수 있게 설정
       - 아직 메인 상품 목록은 되지 않음 / navbar만 반응
       - 상품 상세 페이지도 아직 미반영
  
  2.42 상품 1개 조회 시, DoesNotExist 오류가 발생했을 때는 Http404, 즉 Page not found 오류를 띄울 수 있게 설정
       - 그래서 posts 앱의 views.py에 상품 1개 조회 시, 예외처리를 위해 Http404 import 진행
       - 404에러는 서버에서 요청한 리소스를 찾을 수 없는 경우를 의미
  
  2.5 로그인/로그아웃 및 회원가입 기능 구현
      - Accounts App을 생성하고, 앱 내부 urls.py에서는 회원가입 / 로그인 / 로그아웃 URL 설정
      - base.html에서 로그인 및 회원가입 버튼을 누르면 -> GET방식으로 HTTP Request 진행 + {% if user.is_authenticated %} -> user라는 변수를 바로 사용할 수 있게 되므로, is_authenticated 함수를 사용 / 해당 함수는 로그인 되어있으면 True를 아니면 False를 반환하는 Boolean 함수이다. -> 따라서 로그인 되어있을 때, 로그아웃 버튼과 마이페이지 버튼을 생성하게 설정 -> 로그아웃 역시 정보 노출이 되지 않게 POST방식으로 진행
      - sign_up.html에서는 form element로 POST방식으로 HTTP Request진행 -> 아이디 / 비밀번호 / 비밀번호 확인 / 닉네임 / 이메일을 입력받고, 관심있는 사회문제를 선택할 수 있게끔 설정
      - login.html에서는 로그인 실패 시, 에러 메시지를 띄우고 POST방식으로 HTTP Request 진행 -> 아이디와 비밀번호만 받게끔 설정
      - 앱 내부 views.py의 sign_up View에서는, GET방식일 경우 회원가입 페이지를 랜더링하고 POST방식일 경우 회원가입 절차 진행 => POST방식으로 전달한 username / password 데이터가 있는지 확인하고, password와 password_check데이터가 같은지 확인 -> 만약 같다면 User모델의 create_user 함수를 사용해서 username / password데이터로 User 모델에 추가(회원가입) -> 그리고 회원가입 시 입력한 nickname / email 데이터를 변수에 저장하고 User 모델과 OnetoOne관계인 Customer 모델에 해당 유저 및 데이터 추가 -> 위 과정을 다 진행한 다음, 자동으로 auth모듈의 login함수를 통해 로그인을 시키고 메인 페이지로 redirect해주기 -> 만약, 회원가입 정보를 다 받지 못한다면 context 딕셔너리에 error라는 key를 저장해서 에러 메세지 띄우기
      - 앱 내부 views.py의 login View에서는, GET방식일 경우 로그인 페이지를 랜더링하고 POST방식일 경우 로그인 절차 진행 => POST방식으로 전달한 username / password 데이터가 있다면, auth 모듈의 authenticate 함수를 사용해서 username / password 데이터를 가진 유저가 있는지 확인 진행 -> 유저가 있다면 user 인스턴스를 return하므로, 그렇다면 auth 모듈의 login함수를 사용해서 로그인 시켜주고 바로 메인페이지로 redirect -> 만약, 그런 유저가 없다면 context에 error key 저장해서 에러 메세지 띄우기 -> 로그인 정보를 다 받지 못한다면 모두 입력하라는 에러 메세지 띄우기
      - 앱 내부 views.py의 logout View는 POST방식일 경우 auth모듈의 logout함수를 사용해서 로그아웃 진행 -> 서버 내 쿠키와 세션 정보를 초기화 
      - +++ 추가해야 할 사항 : Customer 모델의 필드가 현재 user / name / email 이렇게 되어있는데, 여기에 사회문제와 관련된 필드를 추가해서 -> 회원가입 시 받은 정보를 DB에 저장하게끔 하기


</details>
<br>

## 5. 핵심 트러블 슈팅
### 5.1 ...

<br>

## 6. 그 외 트러블 슈팅    ==> 이러한 오류가 왜 발생했는지 & 그리고 어떤 문제를 해결해서 & 결국 내가 얻어낸 성과가 뭔지 
- models.py 설정 시 FloatField 및 IntegerField의 경우 default=0 처럼 default 값 설정 필요

- DB에서 ImageField 설정 시 이미지를 업로드 하지 않으면 html template에서 ValueError 발생 -> models.py에서 관련 모델에 @property를 설정해서 오류 방지

- Javascript 파일 - 새로 생성한 함수에서 url를 정의할 때, 상품 1개 조회 화면에서 장바구니 버튼을 클릭하기 때문에 url에 post_id(product_id) 값이 들어가야 했는데 그러지 않아 **uncaught (in promise) syntaxerror: unexpected token < in json at position 0** 와 같은 오류 발생 -> url 정의할 때 미리 정의한 post_id 변수를 +연산자로 url에 포함시키서 오류 해결   

- 상품 1개 조회 후 결제버튼 클릭해서 상품 1개만 결제 페이지에 보여주는 기능 / 장바구니 페이지에서 결제버튼 클릭해서 장바구니 상품들 결제 페이지에 보여주는 기능 -> 만약 상품 1개 조회 페이지에서 결제 버튼을 눌렀다면 Order 모델에 기존 주문과 다른 새로운 데이터가 추가되어야 하지 않을까 생각한다. 아직 하지 못했다. 또한, Order 모델의 date_ordered 필드가 어드민 페이지에서는 보이지가 않는다.. 상품 1개에 대한 이미지, 상품명, 가격은 DB에서 가져왔으나 해당 상품과 관련된 orderitem 모델에 있는 quantity를 연동해서 보여주지 못했고, 그래서 수량 * 가격 = 총액도 아직 구현하지 못했다...

- 상품 1개 조회 후 결제버튼 클릭해서 상품 1개만 결제 페이지에 보여주는 기능 / 장바구니 페이지에서 결제버튼 클릭해서 장바구니 상품들 결제 페이지에 보여주는 기능 관련 추가 오류 
  - 장바구니 페이지에서 결제 버튼 눌렀을 때, **TypeError: checkout() missing 1 required positional argument: 'post_id'** 다음과 같은 에러가 있었다. views.py에서 설정한 함수의 필수 파라미터를 request, user_id로 설정했더니 장바구니 페이지에서 버튼을 눌렀을 때는 user_id가 없어서 생긴 오류였다. 이 문제를 해결하기 위해 장바구니 페이지에서 결제 버튼을 눌렀을 때 연결되는 views 함수(request만 파라미터 설정)를 따로 만들고 / 상품 1개 조회 페이지에서 결제 버튼을 눌렀을 때 연결되는 views 함수(request와 user_id를 파라미터로 설정)도 따로 만들어줘서 오류를 해결할 수 있었다.

- Customer 모델의 name과 user 필드의 null, blank를 False로 변경했으나 -> migrations에서 문제 발생. 필드를 non-nullable로 바꾸는데 default를 주지 않았던 게 문제였음. 그래서 name과 user 필드에 default='미지정' 이라고 수정한 다음 migrations를 진행하고 migrate를 했으나 ValueError: Field 'id' expected a number but got '미지정' 라는 오류 발생. 해당 오류를 보고 default=0으로 수정하고 다시 migration/migrate 진행했으나 똑같은 오류발생.
  - Customer 모델의 user필드는 User모델과의 OneToOneField로 설정되어 있기 때문에, default값이 문자나 0이 아닌 1이상의 숫자로 설정해야 User모델의 pk와 충돌하지 않게 된다. pk는 자동적으로 1부터 증가하기 때문이다. 그래서 Customer 모델의 user와 name 필드 모두 default=1로 수정하고 / python manage.py showmigrations 명령어를 통해 아직 적용되지 않은 2개의 migration 파일을 삭제한 다음, 다시 migration / migrate 진행하여 오류 해결.
<br>

## 7. 회고 / 느낀점
- ㅇㅇㅇ

