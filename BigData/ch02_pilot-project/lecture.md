##  2. 빅데이터 파일럿 프로젝트
###    소개
>###    1. 파일럿 프로젝트 도메인의 이해
        * 도메인에 대한 이해
            - 파일럿 프로젝트에서 다루고자 하는 빅데이터 도메인 : 자동차의 최첨단 전자창지와 무선통신을 결합한 스마트카 서비스
        * 스마트카
            - 수백 개의 IoT 센서 장착
            - 장동차의 상태를 모니터링하며 수많은 차량 상태 정보를 실시간으로 만듦
            - 해당 데이터는 수집 - 적재 - 처리 및 탐색 - 분석 및 응용 단계로 운전자에게 편의성과 안정성 제공
        1) 요구사항 파악    
            - 차량의 다양한 장치로부터 발생하는 로그 파일을 수집해 기능별 상태 점검(p.33)
                >> 배터리 상태, 브레이크 상태, 타이어 상태, 라이트 상태 등의 스키마를 가짐
                >> 100대의 시범 운행 차량. 3초 간격 데이터 발생
            - 운전자의 운행 정보가 담긴 로그를 실시간으로 수집해 주행 패턴 분석(p.34)
                >> 100대의 시범 운행 차량, 1초 간격 데이터 발생
                >> 가속 페달, 브레이크 페발, 운전대 회전각, 방향지시등, 주행 속도, 주행 지역등 스키마를 가짐
        2) 데이터셋
            - 스마트카 상태 정보
            - 스마트카 운전자 운행
            - 스마트카 마스터
            - 스마트카 물품구매 이력
>###    2. 빅데이터 파일럿 아키텍쳐 이해
        * 환경
            - 개인용 PC 1대에 가상 머신 3대를 만들고 그 위에 필요한 빅데이터 컴포넌트 추가 및 확장
        * 아키텍처
            - 주키퍼
            1) 수집
                - 플럼(Flume), 카프카(Kafka), 스톰(Strom), 에스퍼(Esper)
            - 하둡
            2) 적재
                - HBase, 레디스(Redis)
            3) 처리 / 탐색
                - 휴(Hue)
                    - 하이브(Hive), 스파크(Spark), 우지(Oozie), 스쿱(Sqoop)
            4) 분석 / 응용
                - 임팔라(Impala), 제플린(Zeppelin), 머하웃(Mahout)
        * 레이어
            - 전처리(수집/적재) - 하둡 - 후처리(탐색/분석)
            1) 수집 레이어
                - 요구사항으로부터 차량의 로그 수집 : 플럼
                - 실시간 로그 이벤트 처리 : 스톰
                - 데이터의 안정적 수집을 위한 버퍼링 및 트랜잭션 처리 : 카프카
            2) 적재 레이어
                - 요구사항1의 대용량 로그 파일은 수집과 동시에 플럼 - 하둡으로 적재
                - 요구사항 2의 실시간 데이터는 플럼 - 카프카 - 스톰 - HBase/레디스로 적재
                - 실시간 이벤트 분석 : 스톰(분석결과에 따라 HBase, 레디스로 적재)
            3) 처리/탐색 레이어
                - 정제/변형/통합/분리/탐색, 정형화된 구조로 정규화 : 하이브
                - 가공/분석된 데이터 외부 제공 : 스쿱
                - 프로세스 구성 및 복잡도 낮추고 자동화 : 우지
            4) 분석/응용 레이어
                - 스마트카 상태 점검, 운전자 운행 패턴 분석 : 임팔라, 제플린
                - 군집 분석, 분류/예측 분석 : 머하웃
        * 구축 환경 : p.42~p.43참조
>###    3. 빅데이터 파일럿 프로젝트용 PC 환경 구성
        * 설치
            - 교재 참고
>###    4. 빅데이터 파일럿 프로젝트용 PC 서버 구성
        * 교재 참고
>###    5. CM(Cloudera Manager) 설치
        * 교재 참고
>###    6. 스마트카 로그 시뮬레이터 설치
>###    7. 파일럿 환경 관리