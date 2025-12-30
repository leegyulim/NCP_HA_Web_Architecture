# NCP 기반 고가용성(HA) 웹 서비스 아키텍처 구축
프로젝트 목적: 단일 서버 장애 및 데이터 센터 급 장애에도 중단되지 않는 탄력적 인프라 환경 구축

## 1. 아키텍처 다이어그램
여기에 아키텍처 다이어그램(그림)을 꼭 넣으세요. (draw.io나 Lucidchart를 추천합니다.)

## 2. 주요 구성 요소
VPC & Subnet: 보안 강화를 위한 Public/Private Subnet 분리 및 Multi-Zone 배치.
Load Balancer: ALB(Application Load Balancer)를 통한 부하 분산 및 헬스 체크 기반 장애 감지.
Auto Scaling: 트래픽 가변성에 따른 자동 서버 확장 및 장애 서버 자동 교체(Self-healing).
Cloud DB for MySQL: 고가용성(HA) 옵션을 적용하여 Master-Standby Failover 환경 구축.

## 3. 핵심 트러블슈팅
Issue: Ubuntu 24.04 Nginx 설치 시 IPv6 소켓 에러로 인한 서비스 기동 실패./n
Solution: /etc/nginx/sites-available/default 설정 수정을 통해 IPv4 환경에 최적화하여 해결./n
Issue: NCP ASG의 단일 서브넷 선택 제약으로 인한 멀티 존 구성의 어려움./n
Solution: Zone별로 독립된 ASG를 생성하고 하나의 Target Group에 바인딩하는 Multi-ASG 구조로 고가용성 구현./n
