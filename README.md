# Django 이미지 블로그 (Mobile/Web Service Project) 
by kiokahn@khu.ac.kr

텍스트 블로그 + 이미지 모델(./blog/models.py) 만 추가된 테스트용 코드입니다.

## 실습 과제 수행 내용 (ch03 이미지 블로그 + RESTful API)

예비 코드([kiokahn/web_blog](https://github.com/kiokahn/web_blog)) 위에 실습 전체를 진행했습니다.

- 배포 URL (PythonAnywhere): https://minhyuk.pythonanywhere.com
- API Root: https://minhyuk.pythonanywhere.com/api_root/

### 이미지 블로그
- `blog/models.py` 의 `image` 필드 변경분 마이그레이션 추가 (`0004_alter_post_image.py`)
- `post_list.html` / `post_detail.html` 에 본문과 이미지 출력 (`blog-image-list` 클래스, `blog.css`)
- 웹 화면의 새 글/수정 폼에서도 이미지 업로드 가능 (`PostForm` 에 `image`, `request.FILES`, `enctype="multipart/form-data"`)

### RESTful API (새 게시글 작성)
- `blog/serializers.py` — `PostSerializer` (작성자는 요청한 사용자로 자동 지정)
- `blog/views.py` — `blogImage` ViewSet, `blog/urls.py` — `api_root/Post/` 라우팅
- `mysite/urls.py` — `api-token-auth/` 토큰 발급
- 인증: 강의자료의 `Authorization: JWT <token>` 헤더를 그대로 쓸 수 있도록 키워드를 `JWT`로 바꾼 토큰 인증(`blog/authentication.py`) 추가.
  `Token <token>`, 세션 로그인, ID/비밀번호(Basic)도 허용
- 권한: 조회는 누구나, 게시글 작성·수정·삭제는 인증된 사용자만 (`IsAuthenticatedOrReadOnly`)

### API 테스트 (`test_api/`)

```bash
cd test_api
cp .env.example .env            # BLOG_USERNAME / BLOG_PASSWORD 입력
./curl_test.sh                  # curl: ID/비밀번호 → 토큰 발급 → 토큰으로 게시글 작성
python test_api.py              # Python requests: 토큰 발급 → 이미지 포함 게시글 작성
python test_api.py https://minhyuk.pythonanywhere.com   # 배포 서버 대상
```

---

## 가상환경

Django 프로젝트를 위한 가상환경 생성
```bash
mkdir web-blog
cd mysite
python3 -m venv ./venv
source ./venv/bin/activate # for linux or macos
./venv/bin/activate.ps1 # for windows PowerShell
./venv/bin/activate.bat # for windows CMD
```

MS Windows PowerShell을 사용 할 경우, 실행 정책 보호를 해제 하기위해 PowerShell을 관리자모드로 실행하여 아래를 실행합니다.
```bash
Set-ExecutionPolicy Unrestricted
```

실행 후 프로프트 확인 필요
```
(venv)$
```

## 라이브러리 설치
```bash
pip install -r requirements.txt
```

## DB 마이그레이션
```bash
python manage.py migrate
```

## 관리자 계정 생성
```bash
python manage.py createsuperuser
```

## 서버 실행
```bash
python manage.py runserver
```

## 브라우저 
관리자 접속 후 권한 획득 이후 "+" 버튼이 우 상단에 보여짐

관리자[http://127.0.0.1:8000/admin/]

사용자[http://127.0.0.1:8000/]

Api Root[http://127.0.0.1:8000/api_root/]


## telnet 테스트

```bash
telnet 127.0.0.1 8000
```

아래 "Host"의 경우 ".mysite/settings.py"파일 내 "ALLOWED_HOSTS"에 등록되지 않은 호스트는 http 400 error 응답 함 
```
GET /media/images/pig.png HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0


```