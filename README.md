# ISO필라테스 — 무료 관리자(CMS) 사이트

의뢰자가 로그인해서 공지·일정·지부·포트폴리오 사진을 직접 편집할 수 있는 구조입니다.

## 폴더 구성
- `index.html` : 홈페이지 (data 폴더 내용을 자동으로 불러와 보여줌)
- `data/` : 편집되는 내용 (site/공지/일정/지부/포트폴리오)
- `admin/` : 관리자 페이지 (Sveltia CMS)
- `uploads/` : 관리자가 올린 사진이 저장되는 폴더

> 참고: 지금 이대로 올려도 사이트는 예전과 똑같이 보입니다.
> 아래 3단계를 끝내야 "로그인해서 편집"이 켜집니다.

---

## 관리자 켜기 — 3단계 (한 번만)

### 1단계. GitHub에 올리기
1. github.com 가입 → 초록색 **New repository** → 이름(예: `iso-pilates`), **Public** 선택 → Create
2. 저장소 화면에서 **Add file → Upload files** → 이 폴더 전체를 드래그 업로드 → Commit

### 2단계. Netlify에 연결 (자동 배포)
1. netlify.com → **Add new site → Import an existing project → GitHub**
2. 방금 만든 저장소 선택 → Deploy
3. 사이트 주소(`xxxx.netlify.app`)가 생깁니다. (도메인 연결은 나중에)

### 3단계. 로그인(인증) 붙이기
Netlify 내장 로그인이 없어져서, GitHub 로그인 + Cloudflare 무료 도우미를 씁니다.
1. **GitHub → Settings → Developer settings → OAuth Apps → New OAuth App**
   - Homepage URL: 내 사이트 주소
   - Authorization callback URL: `https://(cloudflare주소)/callback`
   - 만들면 **Client ID / Client Secret** 발급
2. **Cloudflare(무료 가입) → Workers**에 `sveltia-cms-auth` 배포
   - 코드: github.com/sveltia/sveltia-cms-auth (안내대로 Deploy)
   - 환경변수에 위 Client ID / Secret 입력 → 배포되면 Worker 주소가 생김
3. `admin/config.yml` 파일을 열어 두 줄 교체 후 저장(커밋):
   - `repo: OWNER/REPO` → `repo: 내계정/iso-pilates`
   - `base_url: https://AUTH...` → Cloudflare Worker 주소

### 완료 🎉
- `내사이트주소/admin` 접속 → GitHub로 로그인 → 공지·일정·지부·사진 편집/업로드
- **저장하면 몇 분 뒤 사이트에 자동 반영**됩니다.

---

3단계(특히 GitHub·Cloudflare)는 처음엔 어려우니, 한 단계씩 같이 진행하면 됩니다.
막히는 화면이 있으면 캡처해서 보여주세요.

---

## 관리자에서 편집 가능한 항목 (수시 변경 OK)
- **팝업창** — 켜기/끄기, 이미지·제목·내용·링크 편집 (`data/popup.json`)
- **대표 사진 / 기본정보** — 사진·성함·주소·이메일 (`data/site.json`)
- **공지사항** — 글 작성·수정 (`data/notices.json`)
- **교육·시험 일정** — 지부별 일정 (`data/schedule.json`)
- **지부현황(국내)** — 지부·지부장 (`data/branches.json`)
- **포트폴리오 사진** — 이미지 업로드 (`data/portfolio.json`)

관리자 화면(`/admin`)에서 항목을 고르고 수정 → 저장하면 사이트에 자동 반영됩니다.
