# 26FW 팬츠 핏 가이드 — iPad Air 4 / Pro Surf 버전

TCL 태블릿 버전(`../../TCL 태블릿/fit-guide-deploy-update`)을 복사해 iPad 용으로 바꾼 별도 폴더. 원본은 수정하지 않았다.

## TCL 버전과 다른 점
| 항목 | TCL 버전 | iPad 버전 |
| --- | --- | --- |
| 영상 코덱 | H.264 Main **Level 6.0** (iOS 재생 불가) | H.264 High **Level 5.1**, 1600×2400, 29.97fps, faststart |
| 오프라인 캐시 | Service Worker | 사용 안 함 (Pro Surf 등 iOS 앱 WebView 는 SW 미지원) → 메모리 Blob 사전 로드 |
| 화면 맞춤 | `dvh` CSS | JS 로 `innerWidth/innerHeight` 계산 (구형 iPadOS 호환) |
| 자동재생 막힘 | 없음 | "Tap to start" 1회 터치 |
| 멈춤 감시 | 없음 | 6초간 재생 위치 변화 없으면 자동 재시도 |

## 영상 재인코딩 명령 (원본 교체 시 동일하게)
```
ffmpeg -i 원본.mp4 -map 0:v:0 -an -map_metadata -1 -c:v libx264 -profile:v high -level:v 5.1 \
  -pix_fmt yuv420p -preset slow -crf 19 -maxrate 10M -bufsize 20M -r 30000/1001 -g 30 \
  -color_primaries bt709 -color_trc bt709 -colorspace bt709 -tag:v avc1 -movflags +faststart 출력.mp4
```

## 배포 주의
- **기존 저장소(26fw-mlb-pants-fit-guide)의 하위 폴더로 올리지 말 것.** 그 사이트의 서비스 워커(scope `/26fw-mlb-pants-fit-guide/`)가
  하위 폴더 영상 요청까지 가로채 iPad 버전에 간섭한다. **새 저장소**(예: `26fw-mlb-pants-fit-guide-ipad`)로 만들어 GitHub Pages 를 켠다.
- 영상은 6개 합계 약 28MB — GitHub 100MB/파일 제한, Cloudflare 25MiB/파일 제한 모두 통과.

## Pro Surf 권장 설정
- 시작 URL: 배포된 `.../index.html`
- 미디어 자동재생(Allow media autoplay / inline playback) 허용
- 페이지 줌·스크롤 비활성, 화면 꺼짐 방지(Keep screen on)
- iPad 설정 → 손쉬운 사용 → 사용법 유도(Guided Access) 로 앱 고정 권장
