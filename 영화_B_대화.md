# 🎬 영화 티어리스트 웹앱 프로젝트 대화 기록

### 👤 나
아래와 같은 기획서로 코딩해줘

# 🎬 프로젝트 기획서: 영화 티어리스트

## 1. 프로젝트 소개
사용자가 직접 영화를 추가하고, 각 영화를 S, A, B, C, D, 미분류로 나누어 정리할 수 있는 간단한 웹 애플리케이션입니다.

## 2. 목표
- 사용자가 쉽게 영화 목록을 관리할 수 있다.
- 영화마다 티어를 지정해 나만의 취향을 정리할 수 있다.
- 드래그 앤 드롭으로 빠르게 위치를 변경할 수 있다.
- 새로고침해도 데이터가 유지되도록 한다.

## 3. 핵심 기능
### 3.1 영화 추가
- 입력창에 영화 제목을 입력한다.
- 추가 버튼을 누르면 목록에 영화가 추가된다.
- 같은 제목은 중복 추가하지 않는다.

### 3.2 티어 분류
- 영화는 아래 순서로 배열된다.
  - S
  - A
  - B
  - C
  - D
  - 미분류
- S는 가장 위쪽에 배치하고, D는 가장 아래쪽에 배치한다.
- 미분류는 D 아래에 별도로 구분해서 표시한다.
- 영화 카드를 드래그해서 원하는 티어 영역에 놓으면 이동된다.

### 3.3 저장 기능
- 사용자가 선택한 티어 정보는 브라우저의 localStorage에 저장된다.
- 새로고침해도 영화 목록과 티어 위치가 유지된다.

## 4. 화면 구성
- 상단: S 등급 영역
- 아래: A, B, C, D 등급 영역
- 가장 하단: 미분류 영역
- 하단 입력창: 영화 추가

## 5. 기술 스택
- HTML5
- CSS3
- JavaScript (Vanilla JS)
- localStorage

## 6. 구현 범위
- 영화 추가
- 드래그 앤 드롭으로 티어 이동
- 티어별 개수 표시
- 새로고침 유지
- S → D → 미분류 순서 UI 구성

## 7. 제외 범위
- 로그인/회원가입
- 사용자별 계정 관리
- 친구 목록
- Firebase 연동
- 실시간 공유 기능

## 8. 완료 기준
1. 영화를 추가할 수 있다.
2. 영화를 드래그해서 티어에 배치할 수 있다.
3. 티어는 S → A → B → C → D → 미분류 순으로 보인다.
4. 티어 이동 후 새로고침해도 유지된다.

---

### 🤖 AI 도우미
영화 티어리스트 프로젝트 기획서가 아주 깔끔하고 명확하네요! 이 기획을 바탕으로 **Vanilla JS**와 **HTML/CSS**만을 사용하여 기능을 구현해 보았습니다.

이 코드는 하나의 HTML 파일에 모든 스타일과 로직을 포함하고 있어, 바로 실행해서 확인해 보실 수 있습니다.

### 🎬 영화 티어리스트 구현 코드

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>영화 티어리스트 (Movie Tier List)</title>
    <style>
        :root {
            --bg-color: #1a1a1a;
            --card-color: #333;
            --text-color: #fff;
            --s-tier: #ff7f7f; --a-tier: #ffbf7f; --b-tier: #ffff7f;
            --c-tier: #7fff7f; --d-tier: #7fbfff; --un-tier: #aaaaaa;
        }

        body {
            font-family: 'Arial', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        h1 { margin-bottom: 20px; }

        /* 티어 컨테이너 스타일 */
        .tier-container {
            width: 100%;
            max-width: 800px;
            border: 2px solid #444;
            margin-bottom: 20px;
        }

        .tier-row {
            display: flex;
            min-height: 80px;
            border-bottom: 1px solid #444;
        }

        .tier-label {
            width: 100px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            font-size: 1.2rem;
            color: #000;
            text-align: center;
        }

        .tier-label span { font-size: 0.8rem; }

        .tier-content {
            flex-grow: 1;
            display: flex;
            flex-wrap: wrap;
            padding: 10px;
            gap: 10px;
            background-color: #222;
        }

        /* 티어별 색상 */
        .s { background-color: var(--s-tier); }
        .a { background-color: var(--a-tier); }
        .b { background-color: var(--b-tier); }
        .c { background-color: var(--c-tier); }
        .d { background-color: var(--d-tier); }
        .unclassified { background-color: var(--un-tier); }

        /* 영화 카드 스타일 */
        .movie-card {
            background-color: var(--card-color);
            padding: 8px 15px;
            border-radius: 5px;
            cursor: grab;
            user-select: none;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
            transition: transform 0.2s;
        }

        .movie-card:active { cursor: grabbing; }
        .movie-card:hover { transform: scale(1.05); }

        /* 입력창 스타일 */
        .input-section {
            margin-top: 20px;
            display: flex;
            gap: 10px;
        }

        input {
            padding: 10px;
            border-radius: 5px;
            border: none;
            width: 250px;
        }

        button {
            padding: 10px 20px;
            border-radius: 5px;
            border: none;
            background-color: #e50914;
            color: white;
            cursor: pointer;
            font-weight: bold;
        }

        button:hover { background-color: #b20710; }
    </style>
</head>
<body>

    <h1>🎬 My Movie Tier List</h1>

    <div class="tier-container" id="tier-board">
        <!-- 티어 행들이 여기에 생성됩니다 -->
    </div>

    <div class="input-section">
        <input type="text" id="movie-input" placeholder="영화 제목을 입력하세요...">
        <button id="add-btn">추가하기</button>
    </div>

    <script>
        // 1. 초기 데이터 및 상태 관리
        const tiers = ['S', 'A', 'B', 'C', 'D', '미분류'];
        const tierColors = { 'S': 's', 'A': 'a', 'B': 'b', 'C': 'c', 'D': 'd', '미분류': 'unclassified' };
        
        // 로컬 스토리지에서 데이터 불러오기 (없으면 빈 객체)
        let movieData = JSON.parse(localStorage.getItem('movieTierData')) || {
            'S': [], 'A': [], 'B': [], 'C': [], 'D': [], '미분류': []
        };

        const tierBoard = document.getElementById('tier-board');
        const movieInput = document.getElementById('movie-input');
        const addBtn = document.getElementById('add-btn');

        // 2. 화면 그리기 (Render)
        function render() {
            tierBoard.innerHTML = ''; // 기존 내용 삭제

            tiers.forEach(tier => {
                const row = document.createElement('div');
                row.className = 'tier-row';
                
                // 티어 라벨 (왼쪽)
                const label = document.createElement('div');
                label.className = `tier-label ${tierColors[tier]}`;
                label.innerHTML = `${tier} <span>(${movieData[tier].length})</span>`;
                
                // 영화 배치 영역 (오른쪽)
                const content = document.createElement('div');
                content.className = 'tier-content';
                content.dataset.tier = tier; // 드롭 대상 구분을 위한 데이터 속성

                // 드래그 앤 드롭 이벤트 리스너
                content.ondragover = (e) => e.preventDefault();
                content.ondrop = (e) => handleDrop(e, tier);

                // 해당 티어의 영화 카드들 생성
                movieData[tier].forEach(movieTitle => {
                    const card = document.createElement('div');
                    card.className = 'movie-card';
                    card.textContent = movieTitle;
                    card.draggable = true;
                    
                    // 드래그 시작 시 데이터 설정
                    card.ondragstart = (e) => {
                        e.dataTransfer.setData('text/plain', movieTitle);
                        e.dataTransfer.setData('fromTier', tier);
                    };

                    content.appendChild(card);
                });

                row.appendChild(label);
                row.appendChild(content);
                tierBoard.appendChild(row);
            });

            // 데이터 저장
            localStorage.setItem('movieTierData', JSON.stringify(movieData));
        }

        // 3. 영화 추가 기능
        function addMovie() {
            const title = movieInput.value.trim();
            if (!title) return;

            // 중복 체크 (모든 티어 검사)
            const allMovies = Object.values(movieData).flat();
            if (allMovies.includes(title)) {
                alert('이미 목록에 있는 영화입니다!');
                return;
            }

            movieData['미분류'].push(title);
            movieInput.value = '';
            render();
        }

        // 4. 드롭 처리 (티어 이동)
        function handleDrop(e, toTier) {
            e.preventDefault();
            const movieTitle = e.dataTransfer.getData('text/plain');
            const fromTier = e.dataTransfer.getData('fromTier');

            if (fromTier === toTier) return; // 같은 티어로 이동 시 무시

            // 기존 티어에서 제거
            movieData[fromTier] = movieData[fromTier].filter(m => m !== movieTitle);
            // 새 티어에 추가
            movieData[toTier].push(movieTitle);

            render();
        }

        // 이벤트 연결
        addBtn.onclick = addMovie;
        movieInput.onkeypress = (e) => { if (e.key === 'Enter') addMovie(); };

        // 초기 실행
        render();
    </script>
</body>
</html>
```

### 💡 주요 구현 설명

1.  **데이터 구조 (`movieData`)**:
    *   `{ 'S': ['영화A'], 'A': [], ... }` 형태의 객체를 사용하여 각 티어별로 영화 제목 리스트를 관리합니다.
    *   이 구조 덕분에 `localStorage`에 저장하고 불러오기가 매우 간편합니다.

2.  **드래그 앤 드롭 (Drag and Drop API)**:
    *   `draggable="true"` 속성을 부여하여 카드를 움직일 수 있게 했습니다.
    *   `dragstart` 시점에 영화 제목과 현재 티어 정보를 `dataTransfer`에 담아 전달합니다.
    *   `drop` 이벤트가 발생하면 데이터를 읽어와서 배열의 위치를 옮겨주고 다시 화면을 그립니다(`render`).

3.  **중복 방지**:
    *   새로운 영화를 추가할 때 `Object.values(movieData).flat()`을 사용하여 모든 티어에 해당 제목이 있는지 확인합니다.

4.  **로컬 스토리지**:
    *   `render()` 함수가 호출될 때마다 현재 상태를 `localStorage`에 저장하므로, 브라우저를 새로고침해도 데이터가 그대로 유지됩니다.

### 🌟 학습 팁
*   **CSS Flexbox**: 티어의 레이아웃을 잡기 위해 `display: flex`를 적극 활용했습니다.
*   **State-based Rendering**: 데이터를 먼저 수정하고 `render()` 함수를 호출해 화면을 갱신하는 방식은 현대 웹 프레임워크(React 등)의 핵심 원리와 비슷합니다. 이 방식을 익혀두면 나중에 큰 도움이 될 거예요!

이 코드를 실행해 보시고, 혹시 디자인을 수정하고 싶거나 추가하고 싶은 기능(예: 영화 삭제 버튼)이 있다면 언제든 말씀해 주세요! 화이팅입니다! 👍