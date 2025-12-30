# 🌐 NCP 기반 고가용성(HA) 웹 서비스 아키텍처 구축

> **Naver Cloud Platform(VPC) 환경에서 장애에 강하고 탄력적인 웹 인프라를 설계 및 구축한 프로젝트입니다.**
> 


## 1. 아키텍처 다이어그램
<img width="465" height="1171" alt="고가용성 웹 서비스 아키텍처 다이어그램" src="https://github.com/user-attachments/assets/f384c3fd-6de8-4817-b00c-51e4dd88354d" />

## 2. 프로젝트 개요
단순한 서버 운영을 넘어, 특정 데이터 센터 장애나 갑작스러운 트래픽 증가에도 서비스 중단 없이 대응할 수 있는 **고가용성(High Availability)** 인프라 구축을 목표로 합니다.

## 3. 핵심 기술 스택
- **Cloud**: Naver Cloud Platform (VPC)
- **Computing**: Server, Auto Scaling, Launch Configuration
- **Networking**: Load Balancer (ALB), NAT Gateway, Global DNS
- **Database**: Cloud DB for MySQL (HA 옵션)
- **Web Server**: Nginx, Ubuntu 24.04 LTS

## 4. 주요 특징 (Key Features)
- **Multi-Zone 배치**: KR-1, KR-2 가용 영역에 인프라를 분산하여 데이터 센터 급 장애에 대비.
- **네트워크 격리**: 외부 접속은 로드밸런서(Public)를 통해서만 가능하며, 실제 서버와 DB는 Private Subnet에 배치하여 보안 강화.
- **자동 확장(Auto Scaling)**: CPU 부하에 따라 서버 대수가 자동 조절되며, 장애 서버 발생 시 자동으로 새 인프라가 교체되는 Self-healing 구현.
- **관리형 DB 고가용성**: Master-Standby 구성을 통해 메인 DB 장애 시 자동으로 대기 서버가 서비스를 승격받는 Failover 환경 구축.

## 5. 트러블슈팅 (Troubleshooting)
### ⚠️ Nginx IPv6 소켓 에러
- **문제**: Ubuntu 24.04 환경에서 Nginx 설치 시 IPv6 지원 문제로 서비스 기동 실패.
- **해결**: `/etc/nginx/sites-available/default` 파일 내 `listen [::]:80` 라인을 주석 처리하여 IPv4 환경으로 최적화.

### ⚠️ NCP ASG 멀티 서브넷 선택 제약
- **문제**: NCP VPC의 ASG는 하나의 서브넷만 지정 가능한 설계적 특성 확인.
- **해결**: 존별로 독립된 ASG(`asg-kr1`, `asg-kr2`)를 생성하고, 동일한 `Target Group`에 바인딩하여 멀티 존 고가용성 달성.

## 6. 결과 검증
- **장애 복구 테스트**: 운영 중인 서버 1대를 임의로 반납 처리 시, Auto Scaling 그룹이 이를 감지하고 약 5분 내에 새로운 서버를 자동으로 생성하여 기대 용량(Desired Capacity)을 복원하는 것을 확인.
