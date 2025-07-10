---
title: "Docker + HomeLab"
date: "2025-07-10"
---

# Docker 기반 해킹 시나리오 통합 홈랩

실제 웹 취약점 공격 시나리오를 기반으로 한 **해킹 실습 환경**을 Docker 기반으로 구성하였습니다.\
ARM(맥북 M칩 기준) 기본 Docker 환경에서 작동하도록 설계되었습니다.\
필요한 추가 구성은 자유롭게 구성하여 사용이 가능합니다. (상업용 재배포X)\
문의사항은 [메일](2er0.oz1228@gmail.com)이나 댓글로 받습니다.

---

## 구성된 컨테이너

| 컨테이너       | 역할                                | 포트      |
| ---------- | --------------------------------- | ------- |
| `kali`     | 공격 컨테이너        | 내부      |
| `dvwa`     | 피해자 컨테이너 : DVWA 사용 (피해자 역할)        | `8843`  |
| `analyzer` | 분석 컨테이너 : `tcpdump` 기반 패킷 분석 | pcap 저장 |

### 다운로드
- [kali-arm.tar](../../assets/files/kali-arm.tar)
- [dvwa-arm.tar](../../assets/files/dvwa-arm.tar)
- [analyzer-arm.tar](../../assets/files/analyzer-arm.tar)
- [docker-compose.yml](../../assets/files/docker-compose.yml)

---

## 폴더 구조
```
folder_name/
├── docker-compose.yml
├── kali-arm.tar
├── dvwa-arm.tar
├── analyzer-arm.tar
├── logs/
│   └── capture.pcap
```

---

## 구성도

```
[Kali] ←── reverse shell ──→ [DVWA] ←── monitored by ──→ [tcpdump]
```

---

## 사용 방법

### 1. Docker 이미지 불러오기

```bash
docker load -i kali-arm.tar
docker load -i dvwa-arm.tar
docker load -i analyzer-arm.tar
```

### 2. 컨테이너 실행

```bash
docker-compose up -d
```

### 3. 접속 정보

- DVWA 접속 : [http://localhost:8843/setup.php](http://localhost:8843/setup.php)

- Kali Shell 접속 :

  ```bash
  docker exec -it kali /bin/bash
  ```

- 패킷 결과 : `logs/capture.pcap`

---

## 개발 환경

- **Apple MacBook (M2)**
- **Docker Desktop (ARM64)**
- 모든 컨테이너는 `linux/arm64` 아키텍쳐 기반으로 동작

---

## 참고

본 컨테이너는 웹 취약점 진단 및 보안 실습을 위한 목적으로 제작되었습니다.\
DVWA 기반 실습용 Docker 컨테이너(피해자 컨테이너)는 [Damn Vulnerable Web Application (DVWA)](https://github.com/digininja/DVWA)를 기반으로 하며, [GPLv3 라이선스](https://www.gnu.org/licenses/gpl-3.0.html)를 따릅니다.

---

> ⚠️ **주의사항**
>
> - 본 프로젝트는 보안 교육 및 연구 목적 외의 용도로 사용 금지합니다.
> - DVWA는 취약하게 만들어진 오픈소스 웹 환경으로, 실제 서비스 환경에 배포하지 마세요.  
> - 피해자 대상 컨테이너는 DVWA를 기반으로 구성되었으며, 저작권 및 라이선스를 준수합니다.
> - 해당 실습 환경의 사용으로 인한 **어떠한 보안 사고나 피해에 대해 작성자는 책임지지 않습니다.**

