## 1. LCP 개선하기

**LCP란**
   - LCP는 사용자에게 가장 중요한 콘텐츠, 보통 히어로 이미지나 헤드라인이 표시되는 데 걸리는 시간을 측정
   - 대부분의 사이트는 누적 레이아웃 이동(CLS)이나 첫 입력 지연(FID)보다 LCP에서 어려움을 겪음
   - 70-80%의 웹사이트에서 이미지 리소스 다운로드가 필요


**LCP 권장사항**
   - HTML을 구조화하여 LCP 리소스를 발견하고 다운로드를 우선순위로 설정하기.
   - 중요하지 않은 리소스의 우선순위를 낮추거나 지연시켜 브라우저가 더 중요한 리소스에 집중하게 하기.
   - LCP 이미지 자체에 우선 순위를 낮추는 행위는 피하기.


**LCP 개선** 
- **이미지 소스를 정적 HTML에서 발견할 수 있도록 수정**
   - 이미지 소스를 정적 HTML에서 발견할 수 있도록 만들어 프리로드 스캐너가 쉽게 찾고 빠르게 로드할 수 있게 한다.
   - background-image, client-side rendering, lazy loading 같은 발견 가능성 문제를 인식해야한다.
   - `<img>`나 프리로드 링크를 사용하여 리소스가 HTML에 참조되게 하여 해결할 수 있다.
   - 이미지를 HTML에 포함하면 즉시 발견되고 더 빨리 다운로드됨.
   - 크롬 개발자 도구의 로딩 워터폴은 늦게 시작하는 리소스를 식별하여 발견 가능성 문제를 나타낼 수 있다.

* **Fetch Priority API**
   - LCP 이미지를 발견하도록 만들어도 빠르게 가져오지 않을 수 있다.
   - 브라우저는 이미지보다 CSS와 JavaScript와 같은 렌더링 차단 콘텐츠를 우선시하기 때문.
   - Fetch Priority API를 사용하면 `fetchpriority=high` 속성을 이미지나 프리로드 LCP 요소에 추가하여 리소스를 높은 중요도로 표시할 수 있다.

   > - 크로미움 기반 브라우저에서 사용 가능하며, Safari와 Firefox도 구현 진행 중.
   > - Angular 및 Next.js와 같은 JavaScript 프레임워크는 fetchpriority 속성을 지원.
   > - WordPress를 사용하는 경우, 공식 WordPress Performance Lab 플러그인의 fetchpriority 모듈 사용.

* **CDN 사용으로 TTFB 최적화**
   - TTFB는 서버에서 초기 HTML 문서 응답의 첫 번째 바이트를 받는 데 걸리는 시간.
   - HTML 처리와 이후의 하위 리소스 로딩은 첫 번째 바이트가 수신될 때까지 시작할 수 없다.
   - CDN은 사용자에게 더 가까운 곳에서 콘텐츠를 제공하고 캐싱하여 빠르게 다시 검색함으로써 TTFB를 줄일 수 있다.

## 2. CLS 개선하기

**CLS란**
   - CLS는 웹페이지의 시각적 안정성을 측정한다.
     - 콘텐츠가 로드되면서 페이지가 얼마나 들썩거리는지
   - 약 1/4의 웹사이트가 권장 CLS 값에 미치지 못한다.

**CLS 개선**

   * **콘텐츠의 크기를 명시하기**
     * 콘텐츠가 처음 렌더링될 떄 정확한 크기(높이, 너비)로 렌더링 되어야 레이아웃의 움직임을 막을 수 있다.
     * 이미지뿐만 아니라 다른 콘텐츠들도 `aspect-ratio`나 `min-height`등 CSS 속성을 통해 사이징 되어야한다.

   * **Back/Forward Cache (BF cache) 사용** 
     * BF 캐시는 유저가 페이지를 떠났을 때, 기존에 완전히 렌더링된 페이지를 메모리에 잠시 저장해둔다.
     * 이를 통해 다시 방문했을 때 CLS를 없앨 수 있다.
     * 크롬에서는 기본으로 동작하지만, 몇몇 API의 사용은 이를 막을 수 있다.
     * Chrome DevTools이나 Lighthouse에서 bf캐시가 가능한지 테스트하여 가능하도록 수정.

   * **애니메이션과 트랜지션** 
     * Composite animation을 layout-inducing, non-composite animations보다 우선시 하기
       * composited animations (transform 같은 것)
       * non-composite animations(top, right, bottom, left를 변화 시키는 것)
     * Lighthouse에서 볼 수 있다.


## 3. FID/IND 개선하기

FID - First Input Delay
IND - Interaction to Next Paint

반응성을 나타내는 지표

**반응성이란**
   - 반응성(Responsiveness)은 메인 스레드가 블록되어있지 않다는 것을 보장하는 것이다.
   - 메인 스레드가 블록 상태면 유저 인풋에 브라우저가 반응할 수 없다.

**반응성 개선**

   * **긴 태스크를 식별 및 분리**
     * 많은 시간이 소요되는 태스크는 메인 스레드를 막을 수 있고 이로인해 무반응을 유발할 수 있다.
     * 긴 태스크를 사용하지 않을 수 있다면 피하는 것이 가장 좋다.
     * Chrome DevTools과 Lighthouse는 50ms 이상이 걸리는 긴 태스크를 식별할 수 있다.
     * 이런 태스크는 `setTimeout`이나 새로운 브라우저 API인 `isInputPending`, `scheduler.postTask`, `scheduler.yield`등을 이용하여 조각낼 수 있다.

   * **자바스크립트양 줄이기**
     * 큰 자바스크립트 파일은 브라우저에서 로딩하는데에 오래 걸린다.
     * Chrome DevTools의 coverage 기능은 첫 로드에 사용되지 않는 부분이 얼마나 있는지 보여준다.
     * 이런 부분을 code split하여 나중에 로드될 수 있도록 하면 개선에 도움이 된다.

   * **큰 렌더링 업데이트 피하기**
     * 큰 렌더링 업데이트는 브라우저를 느리게 한다.
     * 많은 DOM의 변화가 있을 때 발생한다.
     * DOM의 크기를 줄여서 개선할 수 있다.
     * CSS containment와 content visibility를 사용해서 off-screen 요소들에 대해 layout 작업을 막을 수 있다.
