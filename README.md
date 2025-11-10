# 2D 메쉬 커팅 시스템 문서

## 개요

Three.js와 Matter.js를 사용한 2D 메쉬 절단 시스템입니다. 마우스로 절단선을 그어 도형을 분할하고, 물리 엔진으로 자연스러운 낙하와 충돌을 시뮬레이션합니다.

## 주요 기능

- ✂️ **2D 메쉬 절단**: 마우스 드래그로 도형을 자유롭게 절단
- ⚡ **물리 시뮬레이션**: Matter.js 기반 중력, 충돌, 마찰
- 📦 **OBJ 파일 지원**: 3D OBJ 파일을 2D로 투영
- 🎨 **텍스처 매핑**: UV 좌표 기반 텍스처 지원
- 💫 **파티클 효과**: 작은 조각은 파티클로 변환
- 🔍 **디버그 모드**: 물리 바디 시각화
- 🎥 **카메라 컨트롤**: 줌 인/아웃, 팬 이동

## 기술 스택

- **Three.js**: 3D 그래픽스 렌더링 (OrthographicCamera 사용)
- **Matter.js**: 2D 물리 엔진

## 사용 방법

### 기본 조작

- **좌클릭 드래그**: 절단선 그리기
- **우클릭 드래그**: 카메라 팬 (이동)
- **마우스 휠**: 줌 인/아웃

### UI 컨트롤

- 도형 선택: 기본 도형, OBJ 파일, SVG 경로 지원
- 와이어프레임 모드: 메쉬 구조 확인
- 물리 품질: 정점 수 조절 (80/150/200)
- 디버그 모드: 물리 바디 시각화

## 주요 함수

### 초기화
- `init()` - 애플리케이션 초기화 (씬, 카메라, 물리 엔진 설정)
- `setupLights()` - 조명 설정
- `setupBackground()`  - 배경 이미지 설정
- `setupPhysics()`  - Matter.js 물리 엔진 초기화
- `createBoundaryWalls()` - 경계 벽 생성

### 도형 생성
- `createMeshFromShape()` - Shape로부터 메쉬와 물리 바디 생성
- `createLeafShape()` - SVG Path 기반 나뭇잎 도형
- `createTriangleShape()`, `createSquareShape()` (564줄), `createPentagonShape()` (576줄), `createCircleShape()` (591줄) - 기본 도형
- `createHamShape()` - 햄 OBJ 파일 로드 (prefab/wholer-ham.obj)
- `processObjToShape()` - OBJ를 2D Shape로 변환
- `createShapeFromSVGPath()`- SVG Path를 THREE.Shape로 변환

### 절단 로직
- `performCut(start, end)` - 절단선을 따라 메쉬 절단
- `splitMeshSimple2D()` - 메쉬를 두 조각으로 분할
- `createShapeFromVertices2D()` - 정점 배열로 Shape 생성

### 물리 및 유틸리티
- `applyCutForce()` - 절단된 조각에 힘 적용
- `calculatePolygonArea()` - Shoelace 공식으로 면적 계산
- `simplifyVertices()` - 정점 간소화 (성능 최적화)
- `applyPhysicsProperties()` - 물리 속성 설정

### 파티클 시스템
- `createParticleEffect()` - 작은 조각을 파티클로 변환
- `updateParticles()'- 파티클 애니메이션 업데이트
- `isFragmentTooSmall()`- 조각 크기 확인

### 알고리즘
- `computeConcaveHull()` - 오목 껍질 알고리즘 (OBJ 외곽선 추출)
- `computeConvexHull()` - 볼록 껍질 알고리즘 (Graham's Scan)
- `computeAverageNearestNeighborDistance()` - 평균 최근접 이웃 거리 계산

### 디버그
- `renderDebugPhysics()`- 물리 바디 시각화
- `setupDebugLog()`- 화면 디버그 로그 설정
- `toggleDebugMode()`  - 디버그 모드 토글
- `addDebugLogLine()` - 디버그 로그 라인 추가
- `toggleDebugLog()` - 디버그 로그 토글
- `clearDebugLog()`- 디버그 로그 지우기
- `toggleDebugLogPause()` - 디버그 로그 일시정지/재개

### 이벤트 핸들러
- `setupEventListeners()`  - 이벤트 리스너 설정
- `onMouseDown()`  - 마우스 다운 이벤트
- `onMouseMove() - 마우스 이동 이벤트
- `onMouseUp()` - 마우스 업 이벤트
- `onWheel()`  - 마우스 휠 이벤트 (줌)
- `onWindowResize()` - 윈도우 리사이즈 이벤트
- `updateBoundaryWalls()` - 경계 벽 업데이트

### UI
- `loadSelectedShape()`  - 선택된 도형 로드
- `resetScene()`  - 씬 초기화
- `clearAllMeshes()`  - 모든 메쉬 제거
- `updateStats()`  - 통계 업데이트
- `toggleWireframe()` - 와이어프레임 모드
- `resetCamera()`  - 카메라 리셋
- `updateVertexQuality()`  - 물리 정점 품질 변경
- `getRandomColor()`  - 랜덤 색상 생성

### 애니메이션
- `animate()`  - 메인 애니메이션 루프 (60fps)

## 주요 변수

### Three.js 관련
- `scene` - Three.js 씬 객체
- `camera` - OrthographicCamera (2D 전용)
- `renderer` - WebGL 렌더러
- `raycaster` - 마우스 피킹용

### Matter.js 관련
- `engine` - Matter.js 물리 엔진
- `world` - 물리 월드
- `walls` - 경계 벽 배열

### 상태 관리
- `meshes` - 메쉬 데이터 배열
- `isDrawing` - 절단선 그리기 중 여부
- `particles` - 파티클 배열
- `debugMode` - 디버그 모드 활성화 여부

## 절단 알고리즘

1. **절단선 그리기**: 마우스 드래그로 시작점과 끝점 설정
2. **교차 감지**: 메쉬 정점이 절단선 양쪽에 있는지 확인
3. **정점 분류**: 절단선 기준으로 양수/음수 그룹 분류
4. **교차점 계산**: 선분과 절단선의 교차점 삽입
5. **Shape 생성**: 각 그룹의 정점으로 새 Shape 생성
6. **물리 바디 생성**: Matter.js Body 생성 및 물리 속성 적용
7. **힘 적용**: 절단된 조각에 분리 힘 적용

## 작은 조각 처리

조각이 원본의 1/40 이하면:
- 메쉬 대신 파티클 효과로 변환
- 사방으로 흩어지는 애니메이션
- 1.2초 후 페이드 아웃

## 성능 최적화

- **정점 간소화**: 복잡한 도형의 정점 수 감소 (80/150/200 선택)
- **디버그 모드**: 기본값 OFF (성능 향상)
- **디버그 업데이트**: 0.1초마다만 업데이트
- **Z축 레이어링**: 메쉬 겹침 방지

## 좌표계

- **Three.js**: Y축 위가 양수
- **Matter.js**: Y축 아래가 양수 (변환 필요)
- **절단**: 2D 평면 (XY)에서만 수행

## 파일 구조

```
mesh-cutting-with-matter.js
├── 전역 변수 선언
├── 초기화 함수
├── 조명 및 배경 설정
├── 물리 엔진 설정
├── 도형 생성 함수들
├── 물리 속성 함수
├── 메쉬 생성 함수
├── 이벤트 리스너
├── 절단 로직
├── 파티클 시스템
├── 디버그 시각화
├── UI 함수들
└── 애니메이션 루프
```

## 버전 정보
- **제작**: Graphics Project
- **최종 업데이트**: 2025

            <button class="btn-reset" onclick="resetScene()">🔄 씬 초기화</button>
            <button class="btn-load" onclick="loadSelectedShape()">📥 도형 불러오기</button>
            <button class="btn-clear" onclick="clearAllMeshes()">🗑️ 모든 도형 제거</button>
            <button class="btn-wireframe" onclick="toggleWireframe()">🔍 와이어프레임</button>
            <button class="btn-camera" onclick="resetCamera()">📷 카메라 리셋</button>
            <button class="btn-debug" onclick="toggleDebugMode()" style="background: linear-gradient(135deg, #ff0000 0%, #ff6b6b 100%);">🔍 물리 충돌 표시 (ON)</button>
            <button class="btn-debug" onclick="toggleDebugLog()" style="background: linear-gradient(135deg, #00ff00 0%, #00aa00 100%);">📺 디버그 로그 (OFF)</button>
