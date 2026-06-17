# ChatGPT Account Generator

[English](README.md) | [한국어](README_KO.md)

이 프로젝트는 ChatGPT 관련 계정 정보를 만들고 저장하는 도구입니다.
웹 화면으로도, 글자만 나오는 창으로도 사용할 수 있습니다.
컴퓨터가 익숙하지 않아도 아래 순서만 따라가면 됩니다.

## 주요 기능

- 계정 정보 만들기
- OTP 메일 기다리기
- 결과를 내 컴퓨터에 저장하기
- 저장된 내용을 밖으로 내보내기
- 웹 화면 또는 명령창으로 실행하기

## 준비물

- Python이 설치된 컴퓨터
- 이 프로젝트 폴더
- OTP 메일을 받을 수 있는 환경

## 아주 쉽게 시작하기

1. 프로젝트 폴더를 엽니다.
2. 필요한 파일을 설치합니다.

```bash
pip install -r requirements.txt
```

3. 프로그램을 실행합니다.

```bash
python main.py
```

4. 글자만 나오는 방식이 좋으면 이것을 사용합니다.

```bash
python main.py cli
```

## 사용 순서

1. 만들 계정 수를 넣습니다.
2. 필요하면 이메일 뒤에 붙일 짧은 글자를 넣습니다.
3. 프로그램이 계정 정보를 만듭니다.
4. OTP 메일이 오기를 기다립니다.
5. OTP를 확인합니다.
6. 결과를 저장하거나 내보냅니다.

## 저장 파일

- `data/accounts.json`
- `data/email-db.json`
- `data/jobs.json`
- `data/result.txt`
- `data/backups/`
- `logs/latest.log`
- `logs/latest-imap.eml`
- `logs/resend-post.log`

## 설정

메일 연결이나 대기 시간을 바꾸고 싶으면 `.env.example`을 `.env`로 복사해서 사용하면 됩니다.
대부분은 기본값 그대로 써도 됩니다.

## 참고

- 이 프로젝트는 내 컴퓨터에 데이터를 저장합니다.
- 결과는 읽기 쉬운 파일 형식으로 저장됩니다.
- 이전 내용은 자동으로 백업됩니다.

