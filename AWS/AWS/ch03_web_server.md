##  3. 웹 서버 구축
>###    Web의 구조와 HTTP 통신의 기본
        3.1.1 웹 애플리케이션
            * Web Application : 인터넷과 인트라넷 등의 네트워크를 통해서 Web브라우저를 사용하고 조작하는 애플리케이션
                - ex) 기업, 개인 홈페이지, 블로그, 뉴스/동영상 전송, SNS교류, 온라인 쇼핑 ..
            1. 웹 어플리케이션 : 브라우저에서 애플리케이션에 접근시, 네트워크 상의 웹 서버에서 프로세싱하고 프로세싱 결과를 브라우저에 표시
            2. 네이티브 어플리케이션 : 설치한 단말기 상에서 프로세싱 실행됨. 동일 기능 앱이라도 플랫폼마다 개발해야 함.
            - 웹 애플리케이션은 web서버의 구축 및 운용이 필수. 네이티브는 네트워크가 없어도 이용 가능함.
        3.1.2 웹서버에 대한 Request(요청)과 Response(응답)
            >> 웹 애플리케이션에서는 Request, Response가 반복하여 애플리케이션을 호출해, 이용자에게 필요한 정보를 주고받음.
            * Request : Web애플리케이션에게 어떠한 프로세싱을 의뢰하는 것.
            * Response : Web서버에서 브라우저로 프로세싱 결과를 송신하는 것.
        3.1.3 Web 서버에 접속과 URL 서식
            * URL : Uniform Resource Locator -- 리소스의 위치를 알려줌, 네트워크상에 존재하는 정보 자원의 장소를 기술하기 위한 데이터 형식. 웹 애플리케이션의 주소
            * URI : Uniform Resource Indicator
            * 서식
                - scheme://<user>:<password>@<host>:<port>/<url-path>?<s earchpart>
                1. 스키마
                    - 어떤 프로토콜을 사용해 통신 하는지 지정
                        >> ftp : 파일전송
                        >> http : web서버 접속
                        >> malito : 전자 메일의 행선지
                        >> telnet : 서버 원격 접속(보안성이 없음)
                            -- 따라서 SSH. 모든 통신 내용을 암호화 시킴. tunnel을 통해 주고 받음
                        >> file : 파일에 대한 접근
                2. 사용자이름(<user>)
                    - 인증이 필요한 서버에 접속시 사용자 이름 지정
                    - 생략가능
                3. 비밀번호(<password>)
                    - 인증 필요 서버 접속시 지정
                    - 생략가능
                4. 호스트 주소(<host>)
                    - 접속 대상 서버의 주소 지정
                    - IP 주소 형식 또는 서버를 나타내는 도메인 이름 지정
                5. 포트번호(<port>)
                    - 접속 대상 호스트의 포트 번호 지정
                    - 생략 가능. 기본적으로 http 지정시 80번 포트 사용되므로 접속시 생략가능
                6. 문서 path(<url-path>)
                    - 접속 대상 서버에 저장되어 있는 프로그램이나 파일의 경로 지정
                7. search(<s earchpart>)
                    - 서버에 문의시 파라미터 지정
                    - Query String(쿼리 문자열)이라고 하며, 파라미터를 "key=value"형식으로 기술
                    - 복수 개의 파라미터 전달 시, "&"사용
        3.1.4 IP 주소와 도메인 이름
            * 인터넷은 TCP/IP 프로토콜을 사용해 통신. 이는 네트워크에 연결되는 기기에 IP주소를 할당하고 통신 제어
            * 현재 널리 사용되는 IPv4에서는 32비트 길이의 주소가 사용됨. 8개씩 나누어 10진수 표현
                >> 이러나 기억하기 어려움. 이를 위해 이해하기 쉬운 이름 부여. >> 도메인 이름
            * 도메인 이름
                - ICANN조직에서 중복되어 사용하지 않도록 관리됨
                - 단 하나밖에 없는 값만 배정됨
                - 취득을 위해 레지스트라라는 조직에 신청해야 함.
                - .(Dot)으로 구분됨. 마지막 블록이 톱 레벨 도메인. 그 앞이 제 2레벨, 그 앞이 제 3레벨
            * 톱 레벨 도메인
                - .com
                - .net
                - .org
                - .kr
            * FQDN : Fully Qualified Domain Name
                - 도메인 이름 앞에 호스트 이름을 붙인 것
                - IP주소로 변환된 후 통신이 이뤄짐(이 변환을 DNS서버가 담당)
                - ex) www.abc.co.kr
                - 회사 자체로는 www1.xxx, www2.xxx로 만들 수 있지만 외부 접근은 www로 접근. (내부적으로 분산. 로드 밸런싱)
                - 만약 ftp.xxx, http.xxx로 여럿 만드는 경우에도 각각을 레지스트라에 등록해야 함.
            * DNS 서버
                - IP주소 변환 담당
                - 인터넷의 최상위 도메인인 루트 도메인을 관리하는 DNS서버를 루트서버라고 부름
                - 네임서버라고도 불림
                - 자신이 관리하는 도메인이나 호스트 이름에 해당하는 IP 주소를 중복되지 않도록 관리하는 파일을 보유함.
                - 호스트에 대한 정보뿐만 아니라 메일 서버나 도메인의 별명 등을 설정 가능
            * 루트 서버
                - 세계에 수십 여개 지역에 배치됨
                - 루트 서버는 .kr이나 .com 등 톱 레벨 도메인을 관리하는 서버 정보를 가짐.
                - 각각의 톱 레벨 도메인 DNS서버는 제2레벨 도메인에 대한 DNS서버 정보를 가짐.
                - 이에 따라 최상위부터 차례로 각 계층의 DNS서버에게 문의해 도메인 이름의 주소 정보를 얻는 구조가 됨.
            * 룩업
                - 정방향 DNS 룩업 : DNS에서 호스트 이름으로부터 IP 주소를 물어보는 것
                - 역방향 DNS 룩업 : IP주소로부터 호스트 이름을 문의
                - 이처럼 IP주소와 호스트 이름 대응시키는 것이 도메인 이름 분석
            * UDP프로토콜 : 데이터가 잘 도착했는지 확인하지 않음.
        3.1.5 HTTP 통신의 구조
            - Web 어플리케이션에서 클라이언트의 브라우저에서 Web 서버의 콘텐츠나 애플리케이션에 접근하는 것으로 프로세싱이 시작됨
            - 이 때, 클라이언트와 Web 서버간의 HTTP 라는 통신 프로토콜 사용됨.
            * HTTP
                - 브라우저와 web서버 사이에 HTML 등의 콘텐츠 송수신에 이용되는 통신 프로토콜
                - 프로세싱 의뢰시 8개 메소드 이용 가능. 특히  GET, POST자주 사용
                - HTTP에서는 클라이언트의 리퀘스트가 제대로 처리될 수도 있고 에러가 나기도 함. 정상/비정상 여부를 나타내기 위해 상태코드가 규정됨
                - 연결 유지하지 않은 Stateless 통신 실시 : 1회 명령 보내면 1회 결과가 오고 그걸로 끝나는 통신
                - 데이터가 암호화되지 않아 통신 경로 어딘가에서 내용이 유출될 가능성 있음. 그래서 보안을 원하면 HTTPS프로토콜을 사용
            * HTTP 상태코드
                - 100번대 : 정보
                - 200번대 : 성공
                - 300번대 : 리다이렉트(301은 다른 URL로 이동)
                - 400번대 : 클라이언트 오류
                - 500번대 : 서버 에러
>###    3.2 S3를 사용한 Web사이트 구축
        * Amazon S3 : Amazon Simple Storage Service - 손쉽게 Web사이트 구축 가능
        3.2.1 Amazon Simple Storage Service
            * Amazon S3
                - 클라우드 상에 스토리지를 제공하는 서비스, 기업 내 시스템에서 자주 이용되는 파일 서버 같은 것.
                - S3는 클라우드상에서 파일을 공유할 수 있는 서비스이므로 인터넷에 접속할 수 있는 환경이면 어디에서든지 접속 가능
                - 파일의 보존만이 아닌 사이트의 콘텐츠 전송이나 백업과 아카이브, 재해 대책, 빅 데이터 분석 등 다양한 용도로 사용 가능
                - 정밀하게 접속 제한 설정 가능
        3.2.2 기본용어
            * Bucket(버킷) : 데이터의 용기
                - 이용시 우선 그릇인 버킷을 작성
                - 버킷 속에 임의의 데이터 저장 가능
                - 접속시에는 S3 버킷을 지정하므로 버킷 이름은 모든 리전에서 유일해야 함.
                - 만들고 데이터를 저장했으면 버킷에 접근 권한을 설정하고 데이터를 공개함.
            * Object(오브젝트) : 저장하는 파일의 호
>###    3.3 EC2를 사용한 Web 서버 구축
        - S3를 사용시 Web 사이트를 쉽게 구축하나, 상세한 설정 불가
        - 실습은 prac.md 참조
        3.3.1 Amazon EC2란?
            * Amazon EC2
                - Amazon Elastic Compute Cloud : 가상 서버 기능을 제공하는 클라우드 서비스
                - EC2는 온프레미스 형태의 기업 시스템에서 Windows 서버나 UNIX 서버에 해당
                - EC2는 AWS의 데이터 센터 내에 설치된 물리 서버를 가상화 기술을 사용해 서비스 이용자들이 공동으로 사용토록 한 서비스
                - 온프레미스 환경과는 달리 EC2는 물리 서버 도입에 드는 초기 비용이 불필요
                - 사용한 만큼 과금하며, 오토 스케일 기능을 이용해 요건 변화에 맞춰 인스턴스의 처리 능력을 자유롭게 확장 또는 축소 가능
        3.3.2 EC2의 기본 용어
            * 인스턴스 : 1대의 서버
            * EBS : Amazon Elastic Block Store >> 서버의 하드 디스크에 해당하는 가상 디스크
            * AMI : Amazon Machine Image >> 서버에 설치하는 OS와 각종 미들웨어 및 애플리케이션의 이미지
        3.3.3 서버 구축
            * 실습 : prac.md참조
            * 인스턴스 상태 : 인스턴스 우클릭하여 조정가능
                - 시작
                - 리부팅
                - 삭제
                - 정지
            * putty접속
                - 인스턴스의 설명에서 ip주소를 클립보드에 복사해 putty에 save
                - 보안접속을 하지 않으면 접속할 수 없다.
                - SSH를 통해 나의 private key로 서버에 접속 전 인증을 받아야 함. 서버의 공개키로 풀 수 있음.
                    >> putty에서 aws주소가 저장된 것을 load
                    >> 좌측에서 SSH - Auth에서 browse
                    >> 해당 인증은 pem을 바로 쓸 수 없다.(putty는 안됨, TeraTerm은 됨.)
                    >> 따라서 putty-64bit-0.70-installer.msi를 다운 받고 시작에서 puttygen을 실행함
                    >> 거기서 load로 pem파일을 열고 ppk파일로 save해주면 된다.
                    >> 다시 putty로 ppk를 통해 인증을 받고 접속한다.
            * 탄력적 IP : 좌측 탭에서 탄력적 IP를 통해 고정 IP를 할당할 수 있다.
>###    3.4 ELB를 사용한 부하 분산
            * 부하
                - 기업의 시스템은 24시간 365일 가동되어야 하므로 피크 타임의 급격한 부하에서 Web서버가 정지되어서는 안된다.
            * 가용성
                - 시스템이 계속 가동될 수 있는 능력
            * 이중화 : 예비 장치를 준비해 장애가 발생해도 시스템 전체가 정지되지 않도록 하는 기술 요소]
            * 커스텀AMI
                - 동일한 구성의 Web서버를 병렬로 여러 대 가동시켜 부하를 분산시키는 방법이 스케일 아웃
                - AWS에서는 동일한 구성의 EC2 인스턴스를 여러 개 생성 가능 > Custom AMI
                - 기존 인스턴스의 우클릭 후 이미지 생성하여 이미지를 만든다.(134p참조)
                - 해당 AMI를 기반으로 인스턴스를 생성함.
            * 로드 밸런스
                - http-node1과 http-node2를 모두 만들었고 가동한다.
                - 로드 밸러너 생성으로 생성
            * 실습 : prac.md참조
>###    3.5 Elastic IP를 사용한 독자 도메인으로 사이트 운용
            * 고정 IP 할당
                - EC2가 사전에 정의된 인스턴스를 기동시, 인터넷으로부터 접속할 때 접속 지점이 되는 public IP주소와 public Host이름이 할당됨.
                - 그러나 인스턴스 중단하고 재기동 시 인스턴스 주소가 변경됨
                - Elastic IP를 할당함으로써 항상 고정 IP주소를 이용할 수 있음. 이는 계정당 5개까지로 제한됨.
        3.5.2 Route 53에 의한 DNS 서버 설정
            * 도메인 등록 : Route 53은 도메인 이름을 구입해 관리할 수 있다. 레지스트라의 역할
            * DNS 서비스 : 도메인 이름을 IP 주소로 변환
            * 헬스 체크 : 장애 발생한 서버가 있으면 다른 정상 서버로 페일 오버 시킬 수 있다.