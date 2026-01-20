# Shift-Left Security Implementation

> GitHub Actions를 활용한 Checkov + CodeQL 통합 보안 파이프라인

---

## 🎯 프로젝트 개요

배포 전 단계에서 인프라(IaC)와 애플리케이션 코드의 보안 취약점을 자동으로 탐지하는 DevSecOps 워크플로우입니다.

### 핵심 특징
- 🛡️ **자동화된 보안 게이트**: 취약점 발견 시 배포 자동 차단
- ⚡ **병렬 실행 최적화**: Infrastructure와 Application 스캔 동시 수행
- 📊 **통합 대시보드**: GitHub Security 탭에서 모든 취약점 통합 관리
- 🔄 **SARIF 표준**: 이기종 도구 결과의 표준화된 집계

---

## 🔧 워크플로우 구성

### Job 구조

```yaml
jobs:
  iac-security:     # Infrastructure 보안 (Checkov)
  app-security:     # Application 보안 (CodeQL) 
  security-summary: # 통합 리포트
```

### 스캔 도구

| 도구 | 대상 | 탐지 항목 |
|------|------|----------|
| **Checkov** | Terraform (IaC) | S3 암호화, Security Group 설정, 리소스 정책 |
| **CodeQL** | Python | SQL Injection, 하드코딩된 시크릿, Command Injection |

---

## 📊 실행 결과

### Actions 탭
```
Status: ❌ Failed

├─ Infrastructure Security (Checkov)  ❌ 10 vulnerabilities
├─ Application Security (CodeQL)      ✅ 5-8 vulnerabilities  
└─ Security Scan Summary              ✅ Report
```

### Security 탭
모든 취약점이 GitHub Security 탭에 통합되어 표시됩니다:
- `Detected by: checkov-iac-scan`
- `Detected by: codeql-python`

---

## 🧪 데모 파일

### sample_vulnerable.tf
의도적으로 취약하게 작성된 Terraform 코드 (교육 목적):
- ❌ S3 버킷 암호화 미설정
- ❌ S3 버킷 버전 관리 비활성화
- ❌ Security Group SSH(22) 전체 공개 (0.0.0.0/0)

### sample_vulnerable_app.py
의도적으로 취약하게 작성된 Python 코드 (교육 목적):
- ❌ 하드코딩된 비밀번호 및 API 키
- ❌ SQL Injection 취약점
- ❌ Command Injection 취약점
- ❌ Path Traversal 취약점

> ⚠️ **주의**: 이 파일들은 보안 스캔 데모용 샘플이며, 실제 프로덕션 환경에서 사용하지 마십시오.

---

## 🚀 사용 방법

### 1. 저장소 클론
```bash
git clone https://github.com/jys705/Shift-Left-Security-Implementation.git
```

### 2. 자동 실행
모든 `push` 및 `pull_request` 시 GitHub Actions가 자동으로 실행됩니다.

### 3. 결과 확인
- **Actions 탭**: 워크플로우 실행 상태
- **Security 탭**: 탐지된 취약점 상세 정보

---

## 📖 참고 자료

- [GitHub Actions 문서](https://docs.github.com/en/actions)
- [Checkov 문서](https://www.checkov.io/)
- [CodeQL 문서](https://codeql.github.com/docs/)
- [SARIF 표준](https://docs.oasis-open.org/sarif/sarif/v2.1.0/)

---

## 📝 기술적 의의

### Shift-Left Security
```
전통적: 개발 → 빌드 → 배포 → [보안 스캔] → 롤백 (비용 高)
Shift-Left: 개발 → [보안 스캔] → 빌드 → 배포 (비용 低)
```

### Single Pane of Glass
여러 보안 도구의 결과를 GitHub Security 탭 하나로 통합하여, 전체 위험 수준을 한눈에 파악할 수 있습니다.
