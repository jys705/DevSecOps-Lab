# DevSecOps Lab

## 프로젝트 개요
클라우드 보안 실습을 위한 DevSecOps 파이프라인 구축 프로젝트입니다.

## 1단계: Shift-Left Security (GHAS + Checkov 통합)

### 아키텍처 구성
- **GitHub Advanced Security (GHAS)**: 애플리케이션 소스 코드 보안 (SAST/SCA)
- **Checkov**: Infrastructure as Code (IaC) 보안 스캔
- **통합 지점**: GitHub Security 탭을 통한 통합 보안 대시보드 (Single Pane of Glass)

### 보안 검사 항목

#### 🔧 Infrastructure (Terraform) - Checkov 스캔
본 프로젝트의 `main.tf`는 **의도적으로 취약하게 작성**되어 다음 보안 이슈를 포함합니다:
- ❌ S3 버킷 암호화 미설정
- ❌ S3 버킷 버전 관리 비활성화
- ❌ S3 퍼블릭 액세스 차단 미설정
- ❌ Security Group SSH(22번) 포트 전체 공개 (0.0.0.0/0)

#### 💻 Application (Python) - CodeQL 스캔
본 프로젝트의 `app.py`는 **의도적으로 취약하게 작성**되어 다음 보안 이슈를 포함합니다:
- ❌ 하드코딩된 비밀번호 및 API 키
- ❌ SQL Injection 취약점
- ❌ Command Injection 취약점
- ❌ Path Traversal 취약점
- ❌ Server-Side Template Injection (SSTI)
- ❌ 취약한 암호화 알고리즘 (MD5)
- ❌ Insecure Deserialization
- ❌ Debug 모드 활성화

### 워크플로우 실행
모든 push 및 pull request 시 자동으로 보안 스캔이 실행됩니다.

```bash
# 워크플로우 확인
GitHub Actions 탭에서 실행 상태 확인

# 보안 결과 확인
GitHub Security > Code scanning 탭에서 Checkov 결과 확인
```

### SARIF 통합
Checkov 스캔 결과는 SARIF(Static Analysis Results Interchange Format) 포맷으로 GitHub Security 탭에 자동 업로드되어 통합 관리됩니다.

## 실습 목표
1. ✅ 취약한 IaC 코드 커밋
2. ⏳ GitHub Actions에서 Checkov 스캔 실패 확인
3. ⏳ Security 탭에서 취약점 목록 확인
4. ⏳ 취약점 수정 후 스캔 통과 확인

---

> **멘토링 포인트**: 본 아키텍처는 애플리케이션 보안(SAST/SCA)과 인프라 보안(IaC Scan)을 GitHub라는 단일 플랫폼 내에서 **SARIF 포맷을 통해 통합**하였습니다. 이를 통해 보안 담당자는 여러 솔루션을 번거롭게 확인할 필요 없이, GitHub의 **통합 보안 대시보드(Single Pane of Glass)**를 통해 프로젝트의 전체적인 위험 수준을 한눈에 파악할 수 있는 체계를 구축하였습니다.
