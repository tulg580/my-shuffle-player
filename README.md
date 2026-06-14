# my-shuffle-player
# 🎵 YouTube Custom Playlist Shuffler

유튜브 웹(PC) 버전의 고질적인 문제인 **지연 로딩(Lazy Loading)** 및 **추천 알고리즘 기반 셔플**에서 벗어나, 순수한 무작위(True Random)로 재생목록을 섞어주는 전용 웹 플레이어입니다. 

HTML5, JavaScript(Vanilla JS), 그리고 YouTube API v3를 활용하여 백엔드 서버 없이 동작하는 싱글 페이지 애플리케이션(SPA)입니다.

---

## 🔥 왜 만들었는가? (개발 동기)

유튜브 웹 버전에서 대형 재생목록을 셔플하면 아래와 같은 킹받는 버그들이 발생합니다:
1. 전체 목록 중 상위 50~100곡(화면에 로드된 큐) 내에서만 무한 반복됨.
2. 사용자의 시청 기록 및 가중치 알고리즘이 개입하여 순수 무작위 셔플이 불가능함.
3. 세션 만료나 광고 차단기(애드가드 등) 충돌로 인해 셔플 상태가 자꾸 초기화됨.

이를 해결하기 위해, **재생목록의 모든 영상 ID를 통째로 메모리에 긁어온 뒤 알고리즘 단에서 완벽하게 뒤흔드는 플레이어**를 직접 구현했습니다.

---

## 🛠️ 주요 기능

- **Google OAuth 2.0 연동**: 안전한 구글 로그인 후 본인 계정의 재생목록을 실시간 동기화
- **재생목록 전체 전수조사**: YouTube Data API v3의 페이지네이션을 활용해 리스트 내 모든 곡 ID 로드
- **진정한 무작위 셔플**: `Fisher-Yates Shuffle` 알고리즘을 적용하여 편향 없는 순수 Random 큐 생성
- **유튜브 IFrame API 연동**: 공식 플레이어를 웹에 임베드하여 연속 재생 및 스킵 제어 가능
- **광고 프리 패스**: 유튜브 프리미엄 계정 로그인 시, API 플레이어에서도 광고 없이 깔끔하게 재생 가능

---

## 💻 기술 스택

- **Frontend**: `HTML5`, `CSS3`, `JavaScript (ES6+)`
- **Authentication**: `Google Identity Services (GIS)`
- **API**: `YouTube Data API v3`, `YouTube IFrame Player API`
- **Hosting**: `GitHub Pages`

---

## 🧠 핵심 로직 (Shuffle Algorithm)

유튜브의 Dynamic Queue 알고리즘을 씹어먹기 위해, 셔플 버튼을 누르는 순간 자바스크립트 내부에서 **피셔-예이츠 셔플(Fisher-Yates Shuffle)** 배열 제어를 수행합니다. 시간 복잡도 $O(n)$으로 완벽한 무작위성을 보장합니다.

```javascript
function fisherYatesShuffle(array) {
    let arr = [...array];
    for (let i = arr.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]]; // 스왑
    }
    return arr;
}
