# 프로젝트

## 1. 스마트팩토리 MES/ERP 통합 시스템 구축
**기간:** 2024.07 ~ 2025.02   
**개요:** 데이터 파편화 해소를 위해 생산(MES) 및 운영(ERP) 데이터를 통합하는 온프레미스 시스템 E2E 구축

### Role
- **Infra:** Rocky Linux 베어메탈 서버 구축, RAID 구성, Nginx GeoIP 보안 적용, Docker 환경 셋업
- **Dev:** React를 통한 화면 개발, Spring Boot 백엔드 API 설계, 데이터 통합 배치(Batch) 개발, Rust 기반 로그 분석 툴 제작
- **Ops:** Jenkins & Docker Compose 기반의 CI/CD 파이프라인 및 백업 자동화 구축

### Tech Stack
- **Language/Framework:** Java 17, Spring Boot, React, Rust
- **Infrastructure:** Rocky Linux 9, Docker, Docker Compose, Nginx
- **Database/Cache:** MySQL, Caffeine (Local Cache)
- **DevOps:** Jenkins, Shell Script, Crontab

### Troubleshooting
- **캐시 전략 최적화 (Redis vs Caffeine)**
  - **Problem:** 복잡한 집계 쿼리로 대시보드 로딩 **3초 지연**, DB 부하 심화
  - **Action:** 데이터 갱신 주기(1h) 분석 후, 오버헤드가 큰 Redis 대신 **Caffeine(JVM Local Cache)** 도입
  - **Result:** 로딩 속도 **0.1초(97% 개선)** 단축 및 DB I/O 비용 최소화
    
- **인프라 구축 및 RAID 전략 수립**
  - **Problem:** 노후 하드웨어의 RAID-1 메타데이터 잔여 문제로 인한 **OS(Rocky Linux) 설치 불가**
  - **Action:** 파티션 테이블 초기화로 컨트롤러 오류를 해결하고, **RAID-0 재구성** 및 **별도 백업 파이프라인**을 구축하여 성능과 안전성 동시 확보
    
- **로그 모니터링 자동화**
  - **Result:** **Rust**로 서버 로그 파싱 CLI 도구를 직접 개발, Crontab에 등록하여 서버 이상 징후 자동 탐지 체계 구현

---

## 2. PMS(프로젝트 관리) 서비스 운영
**기간:** 2025.04 ~ 현재  
**개요:** 대규모 트래픽이 발생하는 PMS 서비스의 백엔드 유지보수 및 운영 안정성 확보

### Role
- **Backend:** 레거시 코드 리팩토링, SaaS 기능 커스터마이징, 신규 기능 API 개발
- **DevOps:** 장애 대응 프로세스 정립, 배포 파이프라인 복구, 시스템 관측성(Observability) 개선

### Tech Stack
- **Language/Framework:** Java(17), Spring Boot, Mybatis
- **Infrastructure:** Linux(redhat)
- **Database:** postgresql
- **DevOps:** Jenkins, Shell Script
- **SaaS:** ASANA, WORKATO

### Troubleshooting 
- **멀티스레드 환경 로그 가시성(Observability) 확보**
  - **Problem:** 동시성 요청이 많아 로그가 뒤섞여 특정 에러의 트래킹이 불가능
  - **Action:** 스레드별 고유 ID와 실행 맥락(Context)을 로그에 남기도록 아키텍처 개선
  - **Result:** 이슈 원인 파악 시간 **50% 단축** 및 디버깅 효율 증대
    
- **SaaS 아키텍처 최적화 (Timeout 해결)**
  - **Problem:** 동기(Sync) 방식의 대량 데이터 처리로 인한 **Timeout 발생 및 비용 폭증**
  - **Action:** 비동기 프로세스로 전환 및 **SaaS 자체 Batch 기능**을 활용한 대량 처리 구조로 재설계
  - **Result:** Timeout 이슈 완전 해결 및 API 호출 비용 절감
    
- **CI/CD 파이프라인 복구 (Shell Script)**
  - **Problem:** Jenkins 플러그인 호환성 문제로 인한 배포 중단 위기
  - **Action:** `sshpass` 및 **Shell Script**를 활용해 배포 로직을 직접 구현하여 파이프라인 대체
  - **Result:** 플러그인 **종속성(Dependency) 제거**를 통해 외부 영향 없는 배포 유연성 확보
