# BROM4KER - ChatGPT Account Creator

파이썬으로 정리한 계정 생성 도구입니다.  
현재 구조는 `frontend/`와 `backend/`로 분리되어 있습니다.
웹 UI용 엔드포인트는 `/backend/*` 네임스페이스를 사용합니다.

## 개요

- 이메일 계정 생성
- OTP 수신을 로컬 Mailcow IMAP에서 조회
- 계정 정보 저장
- 결과 파일 export

## 실행 환경

- Python 3.10+
- Flask
- `requests`
- 로컬 Mailcow IMAP 접근 가능

## 빠른 시작

1. 프로젝트 루트에 `.env` 파일을 만듭니다.
2. `.env.example` 내용을 복사해서 실제 값으로 채웁니다.
3. 의존성을 설치합니다.

```bash
pip install -r requirements.txt
```

4. 아래처럼 실행합니다.

```bash
python main.py
```

CLI를 쓰려면:

```bash
python main.py cli
```

## 설정

### `.env`

아래 값들을 사용합니다.

```env
BATCH_SIZE=5
OTP_TIMEOUT=90000
OTP_POLL=4000
EMAIL_DOMAINS=mail.com
MAILCOW_IMAP_HOST=127.0.0.1
MAILCOW_IMAP_PORT=993
MAILCOW_IMAP_SSL=true
MAILCOW_IMAP_USERNAME=
MAILCOW_IMAP_PASSWORD=
MAILCOW_IMAP_MAILBOX=INBOX
MAILCOW_IMAP_SCAN_LIMIT=10
MAILCOW_IMAP_LOG_LIMIT=1
BACKUP_KEEP_LIMIT=20
```

### 항목 설명

- `BATCH_SIZE`: 동시 처리 기준값
- `OTP_TIMEOUT`: OTP 대기 시간(ms)
- `OTP_POLL`: OTP 폴링 간격(ms)
- `EMAIL_DOMAINS`: 생성할 이메일 도메인 목록, 쉼표로 여러 개 입력 가능
- `MAILCOW_IMAP_HOST`: Mailcow IMAP 호스트
- `MAILCOW_IMAP_PORT`: Mailcow IMAP 포트
- `MAILCOW_IMAP_SSL`: SSL 사용 여부
- `MAILCOW_IMAP_USERNAME`: Mailcow IMAP 로그인 사용자명
- `MAILCOW_IMAP_PASSWORD`: Mailcow IMAP 로그인 비밀번호
- `MAILCOW_IMAP_MAILBOX`: 조회할 메일함
- `MAILCOW_IMAP_SCAN_LIMIT`: OTP 확인 시 최근 몇 개 메일을 검사할지 지정
- `MAILCOW_IMAP_LOG_LIMIT`: OTP 확인 시 최근 몇 개 메일을 자세히 로그로 남길지 지정
- `BACKUP_KEEP_LIMIT`: 백업 JSON 파일을 몇 개까지 보관할지 지정, `0` 이하면 제한 없음
- `CHATGPT_QUOTA_URL`: 로그인 후 호출할 quota 조회 URL

## 명령어

실행 후 메뉴에서 번호를 선택합니다.

### `1. Create accounts`

대화형으로 계정을 생성합니다.

동작 순서:

1. 생성 개수 입력
2. 이메일 suffix 입력
3. 이메일 계정 생성
4. 로컬 Mailcow IMAP에서 OTP 대기
5. 계정 등록 및 저장
6. `data/result.txt` 자동 export

### `2. Export accounts`

저장된 계정 목록을 `data/result.txt`로 내보냅니다.

## 저장 파일

- `data/accounts.json`
- `data/email-db.json`
- `data/jobs.json`
- `data/result.txt`
- `logs/latest.log`
- `logs/latest-imap.eml`

`data/accounts.json`은 계정 결과를 누적 저장합니다.
`data/result.txt`는 한 줄에 이메일 주소 하나씩 저장하며, 새 생성 시 갱신됩니다.
`data/backups/`에는 저장 파일 변경 전 백업 JSON이 생성되며, `BACKUP_KEEP_LIMIT`에 따라 오래된 파일이 자동 삭제됩니다.

## 모듈 구조

- `main.py`: 진입점
- `frontend/`: Flask 웹 UI, i18n, 화면 렌더링
- `backend/`: 도메인 로직과 CLI
- `backend/account_creation/`: 계정 생성, OTP, 등록
- `backend/jobs.py`: 웹 UI 작업 상태 저장
- `frontend/i18n/`: 언어별 JSON 번역 파일

## 참고

- `nodejsfile/`는 현재 파이썬 구현의 참고용 소스입니다.
- 파이썬 코드는 `nodejsfile`에 의존하지 않습니다.
- 이메일 주소는 로컬에서 생성됩니다.

## 실행 방식

- `python main.py`: Flask 웹 UI 실행
- `python main.py cli`: 대화형 CLI 실행

## 웹 UI 엔드포인트

- `GET /backend/jobs`
- `GET /backend/jobs/<job_id>/data`
- `GET /backend/accounts`
- `POST /backend/jobs/create`
- `POST /backend/jobs/export`
- `POST /backend/jobs/<job_id>/delete`
- `POST /backend/accounts/<email>/verify`

## 문법 확인

```bash
python3 -m py_compile main.py backend/*.py backend/account_creation/*.py frontend/*.py testing/tmp_imap_dump.py
```
