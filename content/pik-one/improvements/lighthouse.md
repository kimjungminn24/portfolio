## 왜 개선했나

Lighthouse로 화면별 성능을 측정해 보니 `/find`는 Performance 55점, 한 측정에서는 LCP 요소가 5,610ms였고 메인 스레드 작업은 2.1초나 걸렸습니다.

원인을 찾으려고 source-map-explorer로 번들을 분석했습니다. 전체 JS 473.88KB 중 Leaflet이 145.57KB(30.7%)를 차지했고, 지도를 쓰지 않는 화면에서도 앱이 시작될 때 항상 로드되고 있었습니다.

![개선 전 Lighthouse 점수](assets/pik-one/lighthouse-before.png)

## 개선 과정

- Leaflet 지도를 `React.lazy`와 `Suspense`로 분리해, 지도 화면에 들어갈 때 로드하도록 했습니다. (코드 스플리팅)
- 웹폰트를 서브셋 woff2로 바꾸고, 폰트 CSS를 비동기로 로드하면서 필요한 서버에 preconnect를 걸었습니다.
- Google Analytics는 페이지 로드가 끝난 뒤에 불러오도록 미뤘습니다.

## 결과

Performance는 60점에서 91점으로, Best Practices는 75점에서 100점으로 올랐습니다.

![개선 후 Lighthouse 점수](assets/pik-one/lighthouse-after.png)
