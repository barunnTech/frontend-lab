# 🎨 이미지 컬러 팔레트 생성기

이미지에서 색상을 추출하여 **5가지 다양한 용도의 컬러 팔레트**를 자동으로 생성하는 웹 애플리케이션입니다.

## ✨ 주요 기능

- 🖼️ **드래그 앤 드롭** 또는 클릭을 통한 이미지 업로드
- 🎯 **K-means 클러스터링** 알고리즘 기반 색상 추출
- 🌈 **5가지 다양한 팔레트** 동시 생성
- 📱 **반응형 디자인** (모바일/데스크톱 지원)
- 📋 **클릭 한 번으로 HEX 코드 복사**

## 🎨 팔레트 유형 및 색상 이론

### 1. 🎨 **대표 컬러 (Dominant Colors)**

**색상 이론**: K-means 클러스터링을 통한 주요 색상 추출

**알고리즘**:
```javascript
// 1. 이미지 픽셀 데이터 샘플링 (4픽셀 간격)
// 2. K-means 클러스터링으로 5개 그룹화
// 3. 각 클러스터의 중심점(평균색상) 계산
// 4. 유클리드 거리 30 이하의 비슷한 색상 제거

const distance = Math.sqrt(
    (r1-r2)² + (g1-g2)² + (b1-b2)²
);
```

**활용**: 브랜딩, 주요 UI 컬러, 디자인 기준색

---

### 2. 🌈 **보색 팔레트 (Complementary Colors)**

**색상 이론**: 색상환에서 180° 정반대에 위치한 색상들

**알고리즘**:
```javascript
// RGB → HSL 변환 후 색상(Hue) 180° 회전
const complementaryHue = (originalHue + 180) % 360;
const complementaryRGB = hslToRgb(complementaryHue, saturation, lightness);
```

**특징**:
- 최대한의 **시각적 대비** 제공
- 강렬하고 역동적인 느낌
- 주목도가 높은 디자인에 적합

**활용**: 액센트 컬러, 강조 버튼, 대비가 필요한 UI 요소

---

### 3. 🌅 **배경용 팔레트 (Background Colors)**

**색상 이론**: 높은 명도와 낮은 채도를 통한 가독성 최적화

**알고리즘**:
```javascript
// 여러 밝기 레벨로 변형
const variations = [
    { lightness: 90-95%, saturation: 15-30% },  // 매우 밝음
    { lightness: 85-90%, saturation: 20-40% },  // 밝음  
    { lightness: 80-85%, saturation: 25-35% }   // 중간 밝기
];

// 부족한 경우 기본 파스텔 색상 추가
const defaultPastels = [
    [248, 249, 250], // 연한 회색
    [248, 250, 252], // 연한 블루
    [254, 252, 247], // 연한 베이지
    // ...
];
```

**특징**:
- **텍스트 가독성** 최우선 고려
- 눈의 피로도 최소화
- 부드럽고 차분한 파스텔 톤

**활용**: 웹/앱 배경, 카드 배경, 섹션 배경

---

### 4. 🌿 **아날로그 대비 (Analogous Contrast)**

**색상 이론**: 색상환에서 인접한 색상들을 활용한 조화로운 대비

**알고리즘**:
```javascript
// 원본 색상 기준 ±30°, ±60° 색상 생성
const analogousHues = [
    (originalHue + 30) % 360,   // +30°
    (originalHue + 60) % 360,   // +60°
    (originalHue - 30 + 360) % 360,  // -30°
    (originalHue - 60 + 360) % 360   // -60°
];

// 강조용으로 채도와 밝기 강화
const enhancedSaturation = Math.min(85, Math.max(60, original * 1.2));
const enhancedLightness = Math.min(70, Math.max(40, original * 0.8 + 20));
```

**특징**:
- **자연스럽고 조화로운** 색상 조합
- 부드러운 그라데이션 효과
- 안정감 있는 시각적 흐름

**활용**: UI 액센트, 부드러운 강조, 그라데이션, 아이콘 컬러

---

### 5. 🌡️ **온도 대비 (Temperature Contrast)**

**색상 이론**: 색상의 온도감(따뜻함/차가움)을 기반으로 한 감정적 대비

**알고리즘**:
```javascript
// 색상 온도 판단
const isWarm = (hue >= 315 || hue <= 45) || (hue >= 45 && hue <= 135);

if (isWarm) {
    // 따뜻한 색상 → 차가운 대비색 생성
    contrastHues = [
        180 ± 30,  // 청록 계열
        210 ± 30,  // 파랑 계열  
        240 ± 30,  // 남색 계열
        270 ± 30   // 보라 계열
    ];
} else {
    // 차가운 색상 → 따뜻한 대비색 생성
    contrastHues = [
        0 ± 20,    // 빨강 계열
        30 ± 20,   // 주황 계열
        60 ± 20,   // 노랑 계열
        300 ± 20   // 마젠타 계열
    ];
}

// 강한 강조 효과를 위한 채도/밝기 증폭
const enhancedSaturation = Math.min(90, Math.max(70, original * 1.3));
```

**특징**:
- **감정적 임팩트**가 강한 색상 대비
- 시각적 긴장감과 활력 제공
- 브랜드 개성 표현에 효과적

**활용**: CTA 버튼, 하이라이트, 브랜드 액센트, 강한 시각적 포인트

## 🔧 기술적 구현

### 핵심 기술 스택
- **Vanilla JavaScript** - 외부 라이브러리 의존성 없음
- **Canvas API** - 이미지 픽셀 데이터 처리
- **CSS3** - 모던한 UI 및 애니메이션
- **HTML5** - 드래그 앤 드롭 API

### 색상 변환 알고리즘

**RGB ↔ HSL 변환**:
```javascript
function rgbToHsl(r, g, b) {
    r /= 255; g /= 255; b /= 255;
    const max = Math.max(r, g, b);
    const min = Math.min(r, g, b);
    let h, s, l = (max + min) / 2;
    
    if (max === min) {
        h = s = 0; // 무채색
    } else {
        const d = max - min;
        s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
        
        switch (max) {
            case r: h = (g - b) / d + (g < b ? 6 : 0); break;
            case g: h = (b - r) / d + 2; break;
            case b: h = (r - g) / d + 4; break;
        }
        h /= 6;
    }
    
    return [h * 360, s * 100, l * 100];
}
```

### K-means 클러스터링
```javascript
// 1. 랜덤 초기 중심점 설정
// 2. 각 픽셀을 가장 가까운 중심점에 할당
// 3. 클러스터 평균으로 중심점 업데이트
// 4. 수렴할 때까지 반복 (최대 10회)

for (let iter = 0; iter < 10; iter++) {
    const clusters = Array.from({ length: k }, () => []);
    
    colors.forEach(color => {
        let minDistance = Infinity;
        let closestCentroid = 0;
        
        centroids.forEach((centroid, index) => {
            const distance = getColorDistance(color, centroid);
            if (distance < minDistance) {
                minDistance = distance;
                closestCentroid = index;
            }
        });
        
        clusters[closestCentroid].push(color);
    });
    
    // 중심점 업데이트
    centroids = clusters.map(cluster => {
        const r = cluster.reduce((sum, color) => sum + color[0], 0) / cluster.length;
        const g = cluster.reduce((sum, color) => sum + color[1], 0) / cluster.length;
        const b = cluster.reduce((sum, color) => sum + color[2], 0) / cluster.length;
        return [Math.round(r), Math.round(g), Math.round(b)];
    });
}
```

## 🚀 사용 방법

### 1. **이미지 업로드**
- 드래그 앤 드롭으로 이미지 파일을 업로드 영역에 놓기
- 또는 "이미지 선택" 버튼 클릭하여 파일 선택

### 2. **팔레트 생성**
- 자동으로 5가지 팔레트가 동시에 생성됩니다
- 로딩 시간: 약 1-2초 (이미지 크기에 따라 다름)

### 3. **색상 코드 복사**
- 원하는 색상을 클릭하면 HEX 코드가 클립보드에 복사
- 토스트 알림으로 복사 완료 확인

## 📁 프로젝트 구조

```
color-palette-generator/
├── color-palette-generator.html    # 컬러 팔레트 생성기
├── imgMap.html                     # 이미지맵 생성기
├── README.md                       # 프로젝트 문서
└── (이미지 파일들)                  # 테스트용 이미지들
```

## 🗺️ 이미지맵 생성기 (imgMap.html)

컬러 팔레트 생성기와 함께 제공되는 **이미지맵 생성 도구**입니다. 이미지 위에 클릭 가능한 영역을 설정하고 PC/모바일용 HTML 코드를 자동 생성합니다.

### ✨ 주요 기능

- 🖼️ **이미지 업로드** 및 실시간 미리보기
- 📍 **클릭으로 영역 설정** (두 점으로 사각형 생성)
- 🔄 **드래그&드롭으로 영역 이동**
- 📏 **모서리 드래그로 크기 조절**
- 🗑️ **개별 영역 삭제** 기능
- 🔗 **영역별 링크 URL 설정**
- 💻 **PC/모바일 구분 코드 생성**

### 🔧 기술적 구현

#### 좌표 변환 시스템
```javascript
// 1. 클릭 좌표를 비율로 변환
const x = (e.clientX - rect.left) / rect.width;
const y = (e.clientY - rect.top) / rect.height;

// 2. 실제 이미지 크기로 스케일링
const scaleX = img.naturalWidth / wrapper.offsetWidth;
const scaleY = img.naturalHeight / wrapper.offsetHeight;
const realX = pixelX * scaleX;
const realY = pixelY * scaleY;
```

#### 반응형 코드 생성

**PC용 (절대 좌표)**:
```html
<area shape='rect' target='_blank' href='링크' 
      coords='x1,y1,x2,y2' alt='링크 바로가기'>
```

**모바일용 (퍼센트 좌표)**:
```html
<a href='링크' target='_blank' class='event_btn' 
   style='left:10%;top:20%;width:30%;height:15%'></a>
```

#### 드래그 & 리사이즈 알고리즘
```javascript
// 드래그 이동
document.addEventListener('mousemove', (e) => {
    if (isDragging && dragTarget) {
        const x = e.clientX - wrapperRect.left - offsetX;
        const y = e.clientY - wrapperRect.top - offsetY;
        const left = (x / wrapper.offsetWidth) * 100;
        const top = (y / wrapper.offsetHeight) * 100;
        dragTarget.style.left = `${left}%`;
        dragTarget.style.top = `${top}%`;
    }
});

// 크기 조절
if (isResizing && resizeTarget) {
    const newWidth = e.clientX - wrapperRect.left - left;
    const newHeight = e.clientY - wrapperRect.top - top;
    const widthPercent = (newWidth / wrapper.offsetWidth) * 100;
    const heightPercent = (newHeight / wrapper.offsetHeight) * 100;
}
```

### 🎯 활용 사례

#### 웹 디자인
- **배너 이미지**에 여러 링크 영역 설정
- **인포그래픽**의 각 섹션별 상세 페이지 연결
- **제품 카탈로그** 이미지의 개별 제품 링크

#### 마케팅
- **이벤트 배너**의 참여 버튼 영역
- **프로모션 이미지**의 구매/상세보기 링크
- **지도 이미지**의 지역별 정보 페이지

#### UI/UX
- **복합적인 네비게이션** 이미지
- **대시보드** 각 영역별 기능 링크
- **메뉴 이미지**의 카테고리별 페이지 이동

## 🔗 두 도구의 연계 활용

### 워크플로우 제안
1. **이미지맵 생성기**로 배너/인포그래픽 영역 설정
2. **컬러 팔레트 생성기**로 해당 이미지의 색상 추출
3. 추출된 색상으로 **연결 페이지들의 일관된 디자인** 구성

### 실무 적용 예시
```html
<!-- 이미지맵으로 생성된 영역 -->
<area coords="100,50,300,150" href="/product-detail">

<!-- 해당 페이지에서 추출된 색상으로 디자인 -->
<style>
.product-detail {
    background: #f8f9fa;      /* 배경용 팔레트 */
    color: #2c3e50;           /* 대표 컬러 */
}
.cta-button {
    background: #e74c3c;      /* 온도 대비 색상 */
    border: 2px solid #3498db; /* 보색 */
}
</style>
```

### 통합 디자인 시스템
- **이미지맵**: 사용자 인터랙션 정의
- **컬러 팔레트**: 시각적 일관성 확보
- **결과**: 완성도 높은 통합 웹 경험

## 🎯 활용 사례

### 웹 디자인
- **대표 컬러**: 메인 브랜드 컬러, 네비게이션
- **보색 팔레트**: 액션 버튼, 알림, 강조 요소
- **배경용**: 섹션 배경, 카드 배경
- **아날로그 대비**: 호버 효과, 부드러운 강조
- **온도 대비**: CTA 버튼, 프로모션 배너

### 브랜딩
- 로고에서 추출한 색상으로 브랜드 가이드라인 수립
- 일관된 브랜드 색상 시스템 구축

### UI/UX 디자인
- 이미지 기반 동적 테마 생성
- 콘텐츠에 맞는 적응형 컬러 스킴

## 🔍 기술적 특징

### 성능 최적화
- **픽셀 샘플링**: 4픽셀 간격으로 처리 속도 최적화
- **중복 제거**: 유클리드 거리 기반 비슷한 색상 필터링
- **메모리 효율**: Canvas API를 통한 효율적인 이미지 처리

### 브라우저 호환성
- **Modern Browsers**: Chrome, Firefox, Safari, Edge
- **Required APIs**: Canvas, FileReader, Clipboard API
- **Responsive**: 모든 디바이스에서 최적화된 UX

## 🎨 색상 이론 배경

이 프로젝트는 다음과 같은 색상 이론을 기반으로 설계되었습니다:

- **Itten's Color Theory**: 12색상환 기반 보색 관계
- **Munsell Color System**: HSL 모델의 이론적 기반
- **Accessibility Guidelines**: WCAG 가독성 기준 적용
- **Psychological Color Theory**: 온도 대비의 감정적 효과

## 📝 라이선스

MIT License - 자유롭게 사용, 수정, 배포 가능합니다.

---

### 🔗 참고 자료

- [Color Theory Basics](https://www.interaction-design.org/literature/topics/color-theory)
- [K-means Clustering Algorithm](https://en.wikipedia.org/wiki/K-means_clustering)
- [HSL Color Model](https://en.wikipedia.org/wiki/HSL_and_HSV)
- [Web Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)