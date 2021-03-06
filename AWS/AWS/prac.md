##  실습
>###    1. S3를 이용해 웹 사이트 만들기
        0) aws.amazon.com에 접속
        1) 콘솔 로그인
        2) 상단 바 서비스탭에서 S3로 이동
        3) 버킷 만들기 버튼으로 버킷 생성. 버킷 이름 및 리전 설정.
            - 중복이름 작성 시 오류 발생
        4) 권한 등 설정
        5) 만들고 접속. 업로드. --> 다운받은 S3 sample drag and drop
        6) html 파일을 클릭하고 링크 누르면 접속 가능. denied된 경우 public 권한이 설정되어 있지 않아서 그러함
            - public 권한 : html 파일 클릭하고 권한을 클릭. 거기서 주거나, 개요에서 퍼블릭으로 설정 클릭
        7) 이미지가 포함되어 있으면 이 또한 public 으로 권한을 주어야만 볼 수 있음. 설정할 것.
        8) 파일이 공개되었다면 다음은 공개하는 폴더에 도메인 이름 만으로 접근할 수 있도록 Web 서버 기능 설정
            >> 버킷의 속성탭으로 이동
            >> 정적 웹 사이트 호스팅 클릭
            >> 이 버킷을 사용하여 웹 사이트 호스팅 을 선택하고 다음 지정
                - 엔드포인트 : 서버의 끝에 클라이언트를 마주할 수 있는 종단점
                    >> 웹 브라우저에서 엔드 포인터에 접근하면 도메인 이름만으로도 접근 가능
            >> 만약 존재하지 않는 경로로 들어가려 하면 에러 파일로 이동
        9) 위 방법과 같이 HTML 파일만 잘 만들어 놓는다면 간단하게 웹 사이트를 구축할 수 있다.
>###    2. EC2를 이용해 웹 사이트 구축
        0) aws.amazon.com에 접속
        1) 콘솔 로그인
        2) 상단 바 서비스탭에서 EC2로 이동
        3) 좌측 바에서 인스턴스 클릭
        4) 인스턴스 생성 - AMI 결정. 맨 위 AMI선택 후 종류는 프리티어가 가능한 것 1개 선택
        5) 태그는 별칭 같은 것. 추가
            >> 서버 이름을 붙여 관리하기 쉽도록 하는 것.
        6) 보안 그룹 설정
            >> 보안 그룹은 EC2 인스턴스에 접속할 수 있는 포트 번호와 주소를 설정할 수 있는 방화벽 기능임.
            >> 어디서 접근하든지 SSH로 접근시 위치 무관으로 하면 다 접속함.
            >> 추가해서 HTTP를 해주면 Well-known port도 설정 가능.
            >> 이 경우, 보안 그룹이 전 세계에 열려 있다고 경고가 나오는데 그냥 넘어간다.
        7) 동작 시키면 키 페이를 생성토록 함. : 키 페어는 AWS에 저장하는 퍼블릭 키와 사용자가 저장하는 프라이빗 키 파일로 구성됨.
            >> 이를 사용해 SSH를 통해 인스턴스에 안전히 접속 가능
            >> 기존에 사용한 것이 있으면 그것을 써도 됨. 없으면 새로 생성
            >> 키 페어 다운로드시 AWSkeypair라는 이름의 키 페어 생성됨.
            >> 해당 키는 EC2서버에 접속시 필요. 철저히 관리해야 함.
        8) 다운로드 후 인스턴스 시작
>###    3. 커스텀AMI
        0) aws.amazon.com에 접속
        1) 콘솔 로그인
        2) 상단 바 서비스탭에서 EC2로 이동
        3) 좌측 바에서 AMI 클릭
        4) 이미지 만들고 기존 그룹으로 만듦
>###    4. 로드밸런서 작성
        생략
        0) 좌측 탭에서 로드 밸런서 생성 클릭
        1) 클래식 로드 밸런서 생성
        2) 이름 : http-ELB, 내 기본 VPC
        3) 보안 그룹 할당 : 이름 정하고 넘김
        4) 헬스 체크 설정
        5) EC2 인스턴스 추가 : 부하 분산을 원하는 http-node1, http-node2를 선택, 교차 영역 로드 밸런스 활성화 클릭 시 인스턴스 처리를 분산할 수 있다.
        6) 태그 추가 : Name, WebSample-ELB
        7) 동작확인 : 로드밸런서 탭에서 아래 부분을 늘이고 인스턴스 탭 확인. InService상태이면 ELB가 헬스 체크 하고, EC2 인스턴스가 정상적으로 응답하는 것.
        8) 만약 OutofService이면 정상적인 응답이 아님.
        9) 제일 좌측 탭에서 DNS이름 확인. 해당 URL을 접속시도 해볼 수 있다. 접속시마다 리퀘스트 편성됨.