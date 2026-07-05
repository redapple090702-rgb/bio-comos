<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bio Cosmos - 드레이크 방정식 시뮬레이터</title>
    <style>
        :root {
            --bg-color: #060613;
            --panel-color: #0f0f2d;
            --accent-color: #00ffff;
            --danger-color: #ff0055;
            --text-color: #ffffff;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* 공통 화면 구성 */
        .screen {
            display: none;
            width: 100vw;
            height: 100vh;
            position: absolute;
            top: 0;
            left: 0;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: radial-gradient(circle at center, #111133 0%, #050510 100%);
        }

        .screen.active {
            display: flex;
        }

        /* 1. 로비 화면 (Main Lobby) */
        .lobby-title {
            font-size: 4rem;
            margin-bottom: 60px;
            text-transform: uppercase;
            letter-spacing: 8px;
            color: var(--accent-color);
            text-shadow: 0 0 20px rgba(0, 255, 255, 0.6);
        }

        .menu-container {
            display: flex;
            gap: 50px;
        }

        .diamond-btn {
            width: 140px;
            height: 140px;
            background: var(--panel-color);
            border: 2px solid var(--accent-color);
            transform: rotate(45deg);
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 0 15px rgba(0, 255, 255, 0.2);
        }

        .diamond-btn:hover {
            background: var(--accent-color);
            box-shadow: 0 0 30px rgba(0, 255, 255, 0.6);
        }

        .diamond-btn span {
            transform: rotate(-45deg);
            color: var(--text-color);
            font-weight: bold;
            font-size: 1.1rem;
            letter-spacing: 1px;
            text-align: center;
        }

        .diamond-btn:hover span {
            color: #000;
        }

        /* 상단 네비게이션 트리거 (삼각형 버튼) */
        .nav-btn {
            position: absolute;
            top: 25px;
            width: 0;
            height: 0;
            cursor: pointer;
            transition: transform 0.2s;
            z-index: 10;
        }
        .nav-btn:hover { transform: scale(1.1); }

        .btn-back {
            left: 35px;
            border-top: 15px solid transparent;
            border-bottom: 15px solid transparent;
            border-right: 25px solid var(--accent-color);
        }

        .btn-reset {
            right: 35px;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 25px solid var(--danger-color);
        }

        /* 2. 튜토리얼 화면 */
        .tutorial-box {
            background: var(--panel-color);
            border: 1px solid var(--accent-color);
            padding: 40px;
            border-radius: 8px;
            max-width: 600px;
            text-align: center;
            box-shadow: 0 0 20px rgba(0,255,255,0.1);
        }
        .tutorial-box h2 { color: var(--accent-color); margin-bottom: 20px; }
        .tutorial-box p { line-height: 1.8; margin-bottom: 30px; color: #ccc; }
        .base-btn {
            background: transparent;
            border: 1px solid var(--accent-color);
            color: var(--text-color);
            padding: 10px 25px;
            cursor: pointer;
            border-radius: 4px;
            font-weight: bold;
        }
        .base-btn:hover { background: var(--accent-color); color: #000; }

        /* 3. 게임 플레이 메인 화면 */
        .game-layout {
            display: grid;
            grid-template-columns: 1fr 1.2fr 1fr;
            width: 100%;
            height: 100%;
            padding: 80px 40px 40px 40px;
            gap: 30px;
        }

        .panel {
            background: rgba(15, 15, 45, 0.6);
            border: 1px solid rgba(0, 255, 255, 0.2);
            border-radius: 12px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            backdrop-filter: blur(10px);
        }

        /* 행성 그래픽 구역 */
        .visual-container {
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .planet-sphere {
            width: 220px;
            height: 220px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #555 0%, #111 100%);
            box-shadow: inset -20px -20px 50px #000, 0 0 30px rgba(255,255,255,0.1);
            transition: all 1s ease;
        }

        /* 슬라이더 컨트롤러 구역 */
        .control-container h3 { margin-bottom: 15px; color: var(--accent-color); font-size: 1.1rem; border-bottom: 1px solid #224; padding-bottom: 5px;}
        .slider-group { margin-bottom: 14px; }
        .slider-label { display: flex; justify-content: space-between; font-size: 0.85rem; margin-bottom: 4px; color: #aaa; }
        .slider-group input[type="range"] {
            width: 100%;
            accent-color: var(--accent-color);
            cursor: pointer;
        }
        .select-custom {
            width: 100%;
            background: #050515;
            border: 1px solid #225;
            color: #fff;
            padding: 8px;
            border-radius: 4px;
            margin-top: 5px;
            outline: none;
        }

        /* 힌트 및 엔딩 연동 모듈 구역 */
        .hint-container { gap: 15px; }
        .hint-row { display: flex; gap: 10px; align-items: center; margin-bottom: 10px; }
        .hint-select { flex: 1; padding: 8px; background: #050515; border: 1px solid #225; color: #fff; border-radius: 4px; }
        .btn-hint-trigger {
            font-size: 1.3rem;
            background: none;
            border: 1px solid var(--accent-color);
            padding: 4px 12px;
            border-radius: 4px;
            cursor: pointer;
            color: var(--accent-color);
        }
        .btn-hint-trigger:hover { background: var(--accent-color); color: #000; }
        .hint-display-box {
            flex: 1;
            background: rgba(0,0,0,0.4);
            border: 1px dashed #225;
            border-radius: 6px;
            padding: 15px;
            font-size: 0.9rem;
            line-height: 1.6;
            color: #00ffcc;
            overflow-y: auto;
        }

        /* 4. 엔딩 북 도감 화면 */
        .book-container {
            width: 85%;
            height: 75%;
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 15px;
            overflow-y: auto;
            padding: 20px;
        }
        .book-item {
            background: #09091f;
            border: 1px solid #224;
            border-radius: 6px;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 15px;
            font-size: 0.85rem;
            color: #556;
            transition: all 0.3s;
        }
        .book-item.unlocked {
            border-color: var(--accent-color);
            color: #fff;
            background: linear-gradient(135deg, #0f0f3d 0%, #050515 100%);
            box-shadow: 0 0 10px rgba(0,255,255,0.1);
        }

        /* 모달 시스템 팝업창 */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background: rgba(0,0,0,0.85);
            z-index: 100;
            justify-content: center;
            align-items: center;
        }
        .modal-overlay.active { display: flex; }
        .modal-content {
            background: var(--panel-color);
            border: 2px solid var(--accent-color);
            padding: 35px;
            border-radius: 12px;
            max-width: 500px;
            width: 90%;
            text-align: center;
        }
        .modal-content h2 { margin-bottom: 15px; color: var(--accent-color); }
        .modal-content p { margin-bottom: 25px; line-height: 1.6; font-size: 0.95rem; color: #ddd; }
        .modal-btns { display: flex; justify-content: center; gap: 20px; }

        /* 대형 트루 엔딩 연출 스크린 */
        #true-ending-screen {
            background: radial-gradient(circle at center, #1a0033 0%, #02000a 100%);
            z-index: 200;
            text-align: center;
            padding: 40px;
        }
        #true-ending-screen h1 { font-size: 3.5rem; color: #ff00ff; text-shadow: 0 0 30px #ff00ff; margin-bottom: 20px; }
        #true-ending-screen p { max-width: 700px; line-height: 2; font-size: 1.1rem; color: #e0bfff; }
    </style>
</head>
<body>

    <div id="lobby-screen" class="screen active">
        <h1 class="lobby-title">Bio Cosmos</h1>
        <div class="menu-container">
            <div class="diamond-btn" onclick="navigation.startPlay()">
                <span>START</span>
            </div>
            <div class="diamond-btn" onclick="navigation.openBook()">
                <span>ENDING<br>BOOK</span>
            </div>
        </div>
    </div>

    <div id="tutorial-screen" class="screen">
        <div class="tutorial-box">
            <h2>Cosmos 시뮬레이션 가이드</h2>
            <p>
                당신은 우주생물학 연구원이 되어 행성의 환경 변수들을 통제하게 됩니다.<br>
                중앙의 다양한 정밀 환경 슬라이더와 촉매 옵션을 결합하여 우주 가설 구조를 실험하세요.<br>
                특정 조건 조합이 완성되면 행성의 지질학적 상태가 물리적으로 변화하며 고유 리포트가 수집됩니다.<br>
                목표는 존재 가능한 모든 계통의 우주적 결말을 관측하는 것입니다.
            </p>
            <button class="base-btn" onclick="navigation.closeTutorial()">시뮬레이터 가동</button>
        </div>
    </div>

    <div id="game-screen" class="screen">
        <div class="nav-btn btn-back" title="로비로 돌아가기 (데이터 보존)" onclick="navigation.toLobby()"></div>
        <div class="nav-btn btn-reset" title="현재 상태 초기화" onclick="navigation.triggerResetModal()"></div>

        <div class="game-layout">
            <div class="panel visual-container">
                <div id="planet-graphic" class="planet-sphere"></div>
                <div style="margin-top: 30px; text-align: center; font-size: 0.9rem; color: #88a;" id="planet-status-text">
                    원시 행성 상태 계측 중...
                </div>
            </div>

            <div class="panel control-container">
                <h3>물리 및 지질 변수 조절 ($n_e$)</h3>
                <div class="slider-group">
                    <div class="slider-label"><span>표면 온도 (Temperature)</span><span id="v-temp">15°C</span></div>
                    <input type="range" id="s-temp" min="-200" max="500" value="15" oninput="simulation.update()">
                </div>
                <div class="slider-group">
                    <div class="slider-label"><span>온실가스 농도 (Greenhouse)</span><span id="v-green">10%</span></div>
                    <input type="range" id="s-green" min="0" max="100" value="10" oninput="simulation.update()">
                </div>
                <div class="slider-group">
                    <div class="slider-label"><span>행성 질량비 (Planet Mass)</span><span id="v-mass">1.0 M⊕</span></div>
                    <input type="range" id="s-mass" min="0.1" max="15" step="0.1" value="1.0" oninput="simulation.update()">
                </div>
                <div class="slider-group">
                    <div class="slider-label"><span>자기장 보호막 (Magnetic)</span><span id="v-magnetic">50%</span></div>
                    <input type="range" id="s-magnetic" min="0" max="100" value="50" oninput="simulation.update()">
                </div>

                <h3>화학 및 진화 생태계 변수 ($f_l \cdot f_i$)</h3>
                <div class="slider-group">
                    <div class="slider-label"><span>대기 내 산소 비중 (Oxygen)</span><span id="v-oxygen">21%</span></div>
                    <input type="range" id="s-oxygen" min="0" max="100" value="21" oninput="simulation.update()">
                </div>
                <div class="slider-group">
                    <div class="slider-label"><span>표면 수분 함량 (Water)</span><span id="v-water">70%</span></div>
                    <input type="range" id="s-water" min="0" max="100" value="70" oninput="simulation.update()">
                </div>
                <div class="slider-group">
                    <div class="slider-label"><span>우주생물학적 특수 기폭제</span></div>
                    <select id="s-special" class="select-custom" onchange="simulation.update()">
                        <option value="None">지구형 탄소 유기 분자 체계</option>
                        <option value="TitanMethane">극저온 액체 메탄 용매 환경</option>
                        <option value="EuropaSub">얼음층 지표 하부 심해 구조</option>
                        <option value="Ammonia">고압 액체 암모니아 바다</option>
                        <option value="Chirality50">50:50 라세미 거울상 분자 수프</option>
                        <option value="TidalLock">모항성 조석 고정 상태</option>
                        <option value="Silicon">고온 규소 뼈대 대사 조건</option>
                        <option value="XNA">인공 변형 유전 핵산(XNA) 매트릭스</option>
                        <option value="Radiation">고농도 치사 전리 방사선 구역</option>
                    </select>
                </div>
            </div>

            <div class="panel hint-container">
                <h3>연구 분석 힌트 모듈</h3>
                <div class="hint-row">
                    <select id="hint-selector" class="hint-select"></select>
                    <button class="btn-hint-trigger" onclick="simulation.getHint()">💡</button>
                </div>
                <div id="hint-box" class="hint-display-box">
                    💡 전구 버튼을 누르면 타깃 엔딩을 도달하기 위한 정밀 지질 조건 범위 가이드가 로드됩니다.
                </div>
            </div>
        </div>
    </div>

    <div id="book-screen" class="screen">
        <div class="nav-btn btn-back" onclick="navigation.toLobby()"></div>
        <h2 style="margin-bottom:30px; color:var(--accent-color); letter-spacing:3px;">우주 환경 진화 도감 리스트</h2>
        <div id="book-grid" class="book-container"></div>
    </div>

    <div id="reset-modal" class="modal-overlay">
        <div class="modal-content">
            <h2>행성 초기화 경고</h2>
            <p>주의: 시뮬레이터 환경을 태초의 원시 상태로 되돌리시겠습니까? 현재 조절한 슬라이더 수치는 유실되지만, 이미 수집 완료된 도감 데이터는 보존됩니다.</p>
            <div class="modal-btns">
                <button class="base-btn" style="border-color:var(--danger-color); color:var(--danger-color);" onclick="simulation.confirmReset()">진행하기</button>
                <button class="base-btn" onclick="navigation.closeResetModal()">돌아가기</button>
            </div>
        </div>
    </div>

    <div id="ending-modal" class="modal-overlay">
        <div class="modal-content" style="max-width: 600px;">
            <h4 id="end-type" style="font-size:0.8rem; letter-spacing:2px; margin-bottom:5px;">REPORT</h4>
            <h2 id="end-title">결말 발견</h2>
            <p id="end-desc">상세 정보</p>
            <button class="base-btn" onclick="navigation.closeEndingModal()">데이터 세이브 및 복귀</button>
        </div>
    </div>

    <div id="true-ending-screen" class="screen">
        <h1>TRUE ENDING : GAIA NETWORK</h1>
        <p>
            축하합니다. 당신은 은하계에 잠재된 모든 형태의 진화적 실패와 대안 문명 루트를 완벽히 교차 추적했습니다.<br>
            탄소, 규소, 메탄 생화학 구조를 관통하여 전체 은하 네트워크가 하나의 통합된 우주적 유기적 의식으로 융합을 완료했습니다.<br>
            드레이크 방정식의 변수 $N$은 마침내 영원불멸의 수렴값에 수용되었습니다. 당신은 마스터 우주생물학자입니다.
        </p>
        <button class="base-btn" style="margin-top:40px; border-color:#ff00ff; color:#ff00ff;" onclick="location.reload()">시스템 재부팅</button>
    </div>

    <script>
        // 1. 엔딩 데이터베이스 선언 (15 실패 / 14 대안 성공)
        const endingDB = [
            // 실패 엔딩군 (1 ~ 15)
            { id: 1, isSuccess: false, name: "폭주한 온실효과의 지옥", color: "radial-gradient(circle at 30% 30%, #ff5500 0%, #441100 100%)", desc: "[금성형 엔딩] 온실가스의 축적으로 행성이 방출하는 적외선 복사 에너지가 대기에 완벽히 봉쇄되었습니다. 바다가 증발하고 수증기가 분해되어 우주로 탈출하여 완전히 황폐화되었습니다. (Drake Factor: ne → 0)", hint: "온실가스 60% 이상, 온도 400°C 이상 결합" },
            { id: 2, isSuccess: false, name: "얼어붙은 불모의 땅", color: "radial-gradient(circle at 30% 30%, #e0e0ff 0%, #222244 100%)", desc: "[화성형 엔딩] 행성 질량이 낮아 내부 방사성 중심핵 열원이 조기 고갈되었습니다. 자기장을 잃은 행성은 대기 손실 공정을 거쳐 전역이 영구 동토층으로 변형되었습니다. (Drake Factor: ne → 0)", hint: "행성 질량 0.5 이하, 온실가스 1% 이하" },
            { id: 3, isSuccess: false, name: "공유 결합의 파괴", color: "radial-gradient(circle at 30% 30%, #ffaaae 0%, #330011 100%)", desc: "[우주 방사선 엔딩] 다이너모 외핵 회전력이 부족하여 자기권 보호막이 형성되지 않았습니다. 은하 고에너지 우주선이 지표를 타격하여 합성 중이던 아미노산의 공유 결합을 전리 분해했습니다. (Drake Factor: fl → 0)", hint: "자기장 10% 이하" },
            { id: 4, isSuccess: false, name: "단백질 변성 지옥", color: "radial-gradient(circle at 30% 30%, #99ff33 0%, #113300 100%)", desc: "[분자 붕괴 엔딩] 고열 조건이 유기 거대 분자의 비공유 수소 결합 구조를 영구 변성(Denaturation)시켰습니다. 촉매 효율을 완전히 상실하여 생화학 대사 사이클이 영구 중단됩니다. (Drake Factor: fl → 0)", hint: "온도 100°C 이상" },
            { id: 5, isSuccess: false, name: "제인스 대기 탈출", color: "radial-gradient(circle at 30% 30%, #777 0%, #111 100%)", desc: "[낮은 중력 엔딩] 중력 가속도가 기체 분자의 열역학적 평균 속도를 이겨내지 못하는 제인스 탈출(Jeans Escape) 메커니즘이 활성화되어, 생명 유지용 대기와 휘발성 용매가 전멸했습니다. (Drake Factor: ne → 0)", hint: "행성 질량 0.3 이하" },
            { id: 6, isSuccess: false, name: "가스 행성으로의 폭주", color: "radial-gradient(circle at 30% 30%, #ffcc66 0%, #553300 100%)", desc: "[목성형 급변 엔딩] 암석 중심핵 질량이 한계 임계치를 넘어서며 성간 물질 원반의 수소와 헬륨 가스를 중력적으로 급격히 끌어당겼습니다. 지표면이 소멸하고 가스층에 압착되었습니다. (Drake Factor: ne → 0)", hint: "행성 질량 10 이상" },
            { id: 7, isSuccess: false, name: "산소 독성 대멸종", color: "radial-gradient(circle at 30% 30%, #00ffaa 0%, #002211 100%)", desc: "[산화 스트레스 엔딩] 미생물군의 항산화 효소(Catalase 등) 보호 기작 진화 속도보다 대기 내 산소 축적 속도가 과도하게 빨라 세포막 지질과 유전자가 치명적인 활성산소에 녹아내렸습니다. (Drake Factor: fl → 0)", hint: "산소 농도 25% 이상" },
            { id: 8, isSuccess: false, name: "CHNOPS 필수 원소 결핍", color: "radial-gradient(circle at 30% 30%, #666699 0%, #111122 100%)", desc: "[유전 뼈대 상실 엔딩] 행성 지각 판구조론이 비활성화되어 인산염과 황화물이 해양으로 방출되지 못했습니다. DNA/RNA 백본의 핵심 성분인 인(P)의 부재로 인해 자기복제 사슬 합성이 멈췄습니다. (Drake Factor: fl → 0)", hint: "특수 기폭제가 지구형이면서, 수분 50% 이상, 자기장 80% 이상인데 온도가 극단적인 불균형일 때 (또는 수분 85% 이상 단독 조건)" },
            { id: 9, isSuccess: false, name: "거울상 미로의 저주", color: "radial-gradient(circle at 30% 30%, #ff99ff 0%, #330033 100%)", desc: "[카이랄성 불일치] 유기 구조가 왼손잡이(L형)와 오른손잡이(D형)가 동등하게 결합된 라세미 혼합물 상태로 고착되었습니다. 분자 입체 정렬이 어긋나 이중나선 백본 결합이 물리적으로 무너졌습니다. (Drake Factor: fl → 0)", hint: "특수 기폭제: 50:50 라세미 거울상 분자 수프" },
            { id: 10, isSuccess: false, name: "조석 고정의 비극", color: "radial-gradient(circle at 30% 30%, #333366 0%, #050515 100%)", desc: "[대기 동결 엔딩] 주계열성과의 과도한 거리 밀착으로 동주기 자전이 강제되었습니다. 대기가 영원한 암흑면으로 이동 후 고체로 석출되는 대기 붕괴(Atmospheric Collapse) 현상으로 종말했습니다. (Drake Factor: ne → 0)", hint: "특수 기폭제: 모항성 조석 고정 상태" },
            { id: 11, isSuccess: false, name: "삼투압 쇼크", color: "radial-gradient(circle at 30% 30%, #cc9966 0%, #332211 100%)", desc: "[초염수 해양 엔딩] 과도한 가뭄과 토양 이온 용출로 해수 농도가 임계 한계를 넘었습니다. 고장액 삼투 메커니즘으로 원시 세포 내액이 외부로 급격히 누출되며 구조가 함몰되었습니다. (Drake Factor: fl → 0)", hint: "수분 5% 이상 15% 이하, 온도 70°C 이상" },
            { id: 12, isSuccess: false, name: "절대적 무용매 사막", color: "radial-gradient(circle at 30% 30%, #ffaa55 0%, #442200 100%)", desc: "[휘발성 전멸 엔딩] 행성 구조 내에 유기 분자 확산과 브라운 운동을 유도할 액체 용매 유체가 단 0.01%도 확보되지 못해, 거대 분자 간 유효 충돌 빈도 한계로 진화가 완전 봉쇄되었습니다. (Drake Factor: ne → 0)", hint: "수분 함량 0%" },
            { id: 13, isSuccess: false, name: "동결된 탄소 순환", color: "radial-gradient(circle at 30% 30%, #ffffff 0%, #111133 100%)", desc: "[눈스노볼 엔딩] 대기 상층부 에어로졸 과다로 행성 반사율(Albedo)이 임계점을 초과했습니다. 온도가 하강하고 얼음이 빛을 추가 반사하는 양의 피드백 루프로 영구 동결 구체가 되었습니다. (Drake Factor: fl → 0)", hint: "온도 -50°C 이하, 온실가스 0%" },
            { id: 14, isSuccess: false, name: "지질학적 사망 행성", color: "radial-gradient(circle at 30% 30%, #3a3a3a 0%, #000000 100%)", desc: "[탄소 순환 단절 엔딩] 지각 내부 방사성 원소 비중 결핍으로 맨틀 대류와 화산 분출이 영구 중단되었습니다. 대기 중 이산화탄소를 복구할 지질학적 재배출 경로가 유실되어 식어버렸습니다. (Drake Factor: ne → 0)", hint: "행성 질량 0.2 이하, 자기장 0%" },
            { id: 15, isSuccess: false, name: "항성의 조기 초신성 퇴화", color: "radial-gradient(circle at 30% 30%, #ff3333 0%, #000000 100%)", desc: "[항성 수명 한계] 배치된 모항성의 질량이 태양 체계를 현격히 초과하여 중심핵 핵융합 폭주로 주계열 수명이 수억 년 미만으로 종결되었습니다. 미생물계 형성 단계 전에 파멸했습니다. (Drake Factor: L → 0)", hint: "행성 질량 14 이상, 온도 200°C 이상 (매우 큰 무거운 항성계 유도)" },

            // 성공 및 대안 문명 엔딩군 (16 ~ 29)
            { id: 16, isSuccess: true, name: "푸른 보석의 기적", color: "radial-gradient(circle at 30% 30%, #2277ff 0%, #051533 100%)", desc: "[지구형 진화 문명] 골디락스 존 정밀 안착. 탄소의 4가 결합 안정성과 액체 물 용매를 디딤돌 삼아 인류 문명 계통과 100% 싱크로율을 보이는 기술 문명 안착에 성공했습니다. (Drake Core Master)", hint: "지구 조건: 온도 10~25°C, 온실가스 5~20%, 질량 0.8~1.5, 자기장 40% 이상, 산소 15~25%, 수분 60~85%, 기폭제 None" },
            { id: 17, isSuccess: true, name: "메탄 바다의 개척자", color: "radial-gradient(circle at 30% 30%, #00aaff 0%, #001133 100%)", desc: "[타이탄형 저온 문명] 극저온 상태에서 물 대신 액체 탄화수소를 용매로 사용하며 질소 결합 기반의 인지질 대체 가상 세포막 '아조토솜(Azotosome)'을 고안한 저온 고등 지성체 문명입니다.", hint: "기폭제 TitanMethane, 온도 -150°C 이하" },
            { id: 18, isSuccess: true, name: "얼음 밑의 지성", color: "radial-gradient(circle at 30% 30%, #99ccff 0%, #112244 100%)", desc: "[에우로파형 심해 문명] 수십 킬로미터 두께의 표면 얼음 지각층 하부에서 가스 행성의 조석 가열 열수구를 토대로 황화물 화학 합성을 구동하는 심해 두족류 형태의 초공간 지성 문명입니다.", hint: "기폭제 EuropaSub, 수분 50% 이상" },
            { id: 19, isSuccess: true, name: "암모니아 바다의 현자", color: "radial-gradient(circle at 30% 30%, #ffff99 0%, #333300 100%)", desc: "[고압 암모니아 문명] 고압 환경에서 강력한 극성 수소 결합력을 발휘하는 액체 암모니아를 매개로 번창했습니다. 독특한 질소 고정 대사 효율로 화학 기술 문명을 리드합니다.", hint: "기폭제 Ammonia, 온도 -50°C ~ 0°C" },
            { id: 20, isSuccess: true, name: "초지구의 거인들", color: "radial-gradient(circle at 30% 30%, #885533 0%, #221100 100%)", desc: "[고중력 암석 문명] 지구의 수 배에 달하는 초지구 암석 환경에서 강인한 다각 보행 골격 구조를 안착시켰습니다. 농밀한 대기 밀도를 응용해 초음파 통신 기술의 정점을 보여줍니다.", hint: "기폭제 None, 행성 질량 3.5 ~ 6.0" },
            { id: 21, isSuccess: true, name: "영원한 황혼의 생존자", color: "radial-gradient(circle at 30% 30%, #ff3300 0%, #220000 100%)", desc: "[적색왜성 플레어 극복 문명] M형 항성의 무차별적인 고에너지 UV 플레어 폭사를 방어하기 위해 표피에 생체 형광 단백질 유도체를 코팅한 문명입니다. 수조 년의 항성 수명 혜택을 받습니다.", hint: "기폭제 M-Dwarf 적색왜성 대치 유도 (TidalLock 결합 등 혹은 전용 옵션)" },
            { id: 22, isSuccess: true, name: "끝없는 대양의 지배자", color: "radial-gradient(circle at 30% 30%, #0055ff 0%, #001144 100%)", desc: "[워터월드 해양 문명] 지표 대륙 면적이 0%인 완전 수권 행성입니다. 화염을 통한 금속 제련 기술은 부재하나, 생체 전류 제어 기술과 유기 생체 나노 고분자 배양 기술을 마스터했습니다.", hint: "수분 함량 95% 이상" },
            { id: 23, isSuccess: true, name: "방랑하는 가이아 문명", color: "radial-gradient(circle at 30% 30%, #2b1a4a 0%, #080214 100%)", desc: "[자유 떠돌이 행성 문명] 모항성 중력권 탈출 후 우주 공간을 단독 비행 중인 행성이나, 원시 분자 수소 대기장 밀봉 단열 효과와 강력한 내부 지열의 시너지로 지하 액체 바다 문명을 유지합니다.", hint: "온실가스 90% 이상, 자기장 90% 이상 (특수 단열 조건 매칭)" },
            { id: 24, isSuccess: true, name: "규소 기반의 불사조", color: "radial-gradient(circle at 30% 30%, #ff6600 0%, #220000 100%)", desc: "[고온 규소 화산 문명] 탄소 유기 사슬이 전부 열분해되는 화산 지대에서 탄소 동족 원소인 규소-산소 교차 사슬 고분자를 축으로 진화한 금속성 고온 대사 무기물 지성 문명입니다.", hint: "기폭제 Silicon, 온도 200°C 이상" },
            { id: 25, isSuccess: true, name: "초광합성 에메랄드 매트릭스", color: "radial-gradient(circle at 30% 30%, #009933 0%, #001100 100%)", desc: "[K형 항성 안정 문명] 오렌지색 K형 항성의 최적 거주 구역에서 행성 표면 전체의 식물성 미생물 가이아 네트워크가 하나의 광대한 무선 바이오 컴퓨터 연산군을 구축한 지성체입니다.", hint: "산소 농도 40% ~ 60%, 수분 50% 이상" },
            { id: 26, isSuccess: true, name: "부동액 세포의 영생종", color: "radial-gradient(circle at 30% 30%, #66ffff 0%, #113333 100%)", desc: "[글리세롤 대사 문명] 세포 내액에 천연 에틸렌글리콜 및 글리세롤 부동액 고농도 대사 사이클을 도입하여 영하권 온도에서도 얼음 결정 변형이 없는 극도로 긴 개체 수명의 문명입니다.", hint: "온도 -40°C ~ -10°C, 산소 10% 이하" },
            { id: 27, isSuccess: true, name: "대안 핵산의 개척자", color: "radial-gradient(circle at 30% 30%, #cc00ff 0%, #220033 100%)", desc: "[인공 유전 XNA 문명] 지구의 디옥시리보스 백본 대신 트레오스(Threose) 당 구조 기반 유전 암호 사슬을 정착시켜 분자 복제 오류나 암 종양 유발도가 0에 수렴하는 무결점 사회입니다.", hint: "기폭제 XNA" },
            { id: 28, isSuccess: true, name: "황산 구름의 부력 비행선", color: "radial-gradient(circle at 30% 30%, #cccc33 0%, #333300 100%)", desc: "[금성 대기 상층부 문명] 표면은 초고압 지옥이나 고도 55km 지점의 온화한 1기압 등온대 영역에서 수소 주머니 생체 부력 평형을 이용해 공중에 부유하는 대형 에어로 플랑크톤 문명입니다.", hint: "기폭제 Venusian Cloud (또는 온실가스 70% 이상이면서 기폭제 Radiation 매칭)" },
            { id: 29, isSuccess: true, name: "극한 환경의 생명 공학자", color: "radial-gradient(circle at 30% 30%, #ff00ff 0%, #330033 100%)", desc: "[고농도 방사능 적응 문명] 지표 방사선량이 치사량을 초과하는 환경에서 데이노코쿠스 라디오두란스와 같이 이중 나선 절단면을 실시간 리페어하는 합성 유전자 효소 체계를 극대화한 종족입니다.", hint: "기폭제 Radiation" }
        ];

        // 2. 글로벌 스테이트 매니지먼트 (세이브 데이터)
        let gameState = {
            unlockedEndings: [],
            isFirstPlay: true,
            sliders: {
                temp: 15,
                green: 10,
                mass: 1.0,
                magnetic: 50,
                oxygen: 21,
                water: 70,
                special: "None"
            }
        };

        // 로컬스토리지 복구 아키텍처
        function loadGame() {
            const saved = localStorage.getItem("BIOC_COSMOS_SAVE");
            if(saved) {
                const parsed = JSON.parse(saved);
                gameState.unlockedEndings = parsed.unlockedEndings || [];
                gameState.isFirstPlay = parsed.isFirstPlay !== undefined ? parsed.isFirstPlay : true;
                if(parsed.sliders) gameState.sliders = parsed.sliders;
            }
        }

        function saveGame() {
            localStorage.setItem("BIOC_COSMOS_SAVE", JSON.stringify(gameState));
        }

        // 3. UI 및 네비게이션 엔진
        const navigation = {
            startPlay() {
                document.getElementById("lobby-screen").classList.remove("active");
                if(gameState.isFirstPlay) {
                    document.getElementById("tutorial-screen").classList.add("active");
                } else {
                    this.openSimulator();
                }
            },
            closeTutorial() {
                gameState.isFirstPlay = false;
                saveGame();
                document.getElementById("tutorial-screen").classList.remove("active");
                this.openSimulator();
            },
            openSimulator() {
                document.getElementById("game-screen").classList.add("active");
                simulation.loadSliders();
                simulation.update();
                simulation.initHintSelector();
            },
            toLobby() {
                // 현재 슬라이더 수치 캐싱 보존
                simulation.saveSliders();
                document.getElementById("game-screen").classList.remove("active");
                document.getElementById("book-screen").classList.remove("active");
                document.getElementById("lobby-screen").classList.add("active");
            },
            openBook() {
                document.getElementById("lobby-screen").classList.remove("active");
                document.getElementById("book-screen").classList.add("active");
                this.renderBookGrid();
            },
            renderBookGrid() {
                const grid = document.getElementById("book-grid");
                grid.innerHTML = "";
                endingDB.forEach(end => {
                    const item = document.createElement("div");
                    const isUnlocked = gameState.unlockedEndings.includes(end.id);
                    item.className = `book-item ${isUnlocked ? 'unlocked' : ''}`;
                    item.innerHTML = isUnlocked ? `<strong>[해금]<br>${end.name}</strong>` : `No. ${end.id}<br>[미확인 데이터]`;
                    grid.appendChild(item);
                });
            },
            triggerResetModal() {
                document.getElementById("reset-modal").classList.add("active");
            },
            closeResetModal() {
                document.getElementById("reset-modal").classList.remove("active");
            },
            triggerEndingModal(ending) {
                document.getElementById("end-type").innerText = ending.isSuccess ? "SUCCESS ALTERNATIVE ARCHETYPE" : "DISSOLUTION REPORT";
                document.getElementById("end-type").style.color = ending.isSuccess ? "#00ffff" : "#ff0055";
                document.getElementById("end-title").innerText = ending.name;
                document.getElementById("end-desc").innerText = ending.desc;
                document.getElementById("ending-modal").classList.add("active");
            },
            closeEndingModal() {
                document.getElementById("ending-modal").classList.remove("active");
                // 엔딩 해금 후 전체 수집 상태 스캔 진엔딩 트리거 체크
                if(gameState.unlockedEndings.length >= 29) {
                    document.getElementById("game-screen").classList.remove("active");
                    document.getElementById("true-ending-screen").classList.add("active");
                }
            }
        };

        // 4. 게임 시뮬레이션 물리 코어 엔진
        const simulation = {
            saveSliders() {
                gameState.sliders.temp = parseInt(document.getElementById("s-temp").value);
                gameState.sliders.green = parseInt(document.getElementById("s-green").value);
                gameState.sliders.mass = parseFloat(document.getElementById("s-mass").value);
                gameState.sliders.magnetic = parseInt(document.getElementById("s-magnetic").value);
                gameState.sliders.oxygen = parseInt(document.getElementById("s-oxygen").value);
                gameState.sliders.water = parseInt(document.getElementById("s-water").value);
                gameState.sliders.special = document.getElementById("s-special").value;
                saveGame();
            },
            loadSliders() {
                document.getElementById("s-temp").value = gameState.sliders.temp;
                document.getElementById("s-green").value = gameState.sliders.green;
                document.getElementById("s-mass").value = gameState.sliders.mass;
                document.getElementById("s-magnetic").value = gameState.sliders.magnetic;
                document.getElementById("s-oxygen").value = gameState.sliders.oxygen;
                document.getElementById("s-water").value = gameState.sliders.water;
                document.getElementById("s-special").value = gameState.sliders.special;
            },
            confirmReset() {
                // 슬라이더 초기화, 엔딩 데이터 보존
                gameState.sliders = { temp: 15, green: 10, mass: 1.0, magnetic: 50, oxygen: 21, water: 70, special: "None" };
                saveGame();
                this.loadSliders();
                navigation.closeResetModal();
                this.update();
            },
            initHintSelector() {
                const selector = document.getElementById("hint-selector");
                selector.innerHTML = "";
                endingDB.forEach(e => {
                    const opt = document.createElement("option");
                    opt.value = e.id;
                    opt.innerText = `${e.isSuccess ? '성공:' : '실패:'} ${e.name}`;
                    selector.appendChild(opt);
                });
            },
            getHint() {
                const id = parseInt(document.getElementById("hint-selector").value);
                const ending = endingDB.find(e => e.id === id);
                if(ending) {
                    document.getElementById("hint-box").innerHTML = `<strong>🎯 타깃 분기 조건 가이드:</strong><br>${ending.hint}`;
                }
            },
            update() {
                // 실시간 라벨 출력 핸들링
                const temp = parseInt(document.getElementById("s-temp").value);
                const green = parseInt(document.getElementById("s-green").value);
                const mass = parseFloat(document.getElementById("s-mass").value);
                const magnetic = parseInt(document.getElementById("s-magnetic").value);
                const oxygen = parseInt(document.getElementById("s-oxygen").value);
                const water = parseInt(document.getElementById("s-water").value);
                const special = document.getElementById("s-special").value;

                document.getElementById("v-temp").innerText = temp + "°C";
                document.getElementById("v-green").innerText = green + "%";
                document.getElementById("v-mass").innerText = mass.toFixed(1) + " M⊕";
                document.getElementById("v-magnetic").innerText = magnetic + "%";
                document.getElementById("v-oxygen").innerText = oxygen + "%";
                document.getElementById("v-water").innerText = water + "%";

                this.evaluateRules({temp, green, mass, magnetic, oxygen, water, special});
            },
            evaluateRules(s) {
                let targetEnding = null;

                // --- 엔딩 분기 연산 알고리즘 매트릭스 ---
                
                // 특수 기폭제 선결 조건 우선 검사
                if (s.special === "Chirality50") targetEnding = 9;
                else if (s.special === "TidalLock") targetEnding = 10;
                else if (s.special === "TitanMethane" && s.temp <= -150) targetEnding = 17;
                else if (s.special === "EuropaSub" && s.water >= 50) targetEnding = 18;
                else if (s.special === "Ammonia" && s.temp >= -50 && s.temp <= 0) targetEnding = 19;
                else if (s.special === "Silicon" && s.temp >= 200) targetEnding = 24;
                else if (s.special === "XNA") targetEnding = 27;
                else if (s.special === "Radiation") targetEnding = 29;
                
                // 수치 임계 물리 법칙 규칙 매칭
                if (!targetEnding) {
                    if (s.green >= 60 && s.temp >= 400) targetEnding = 1;
                    else if (s.mass <= 0.5 && s.green <= 1) targetEnding = 2;
                    else if (s.magnetic <= 10) targetEnding = 3;
                    else if (s.temp >= 100) targetEnding = 4;
                    else if (s.mass <= 0.3) targetEnding = 5;
                    else if (s.mass >= 10) targetEnding = 6;
                    else if (s.oxygen >= 25) targetEnding = 7;
                    else if (s.water >= 95) targetEnding = 22;
                    else if (s.water <= 15 && s.water >= 5 && s.temp >= 70) targetEnding = 11;
                    else if (s.water === 0) targetEnding = 12;
                    else if (s.temp <= -50 && s.green === 0) targetEnding = 13;
                    else if (s.mass <= 0.2 && s.magnetic === 0) targetEnding = 14;
                    else if (s.mass >= 14 && s.temp >= 200) targetEnding = 15;
                    else if (s.water >= 85) targetEnding = 8; // CHNOPS
                    
                    // 대안 고등 표준 진화
                    else if (s.mass >= 3.5 && s.mass <= 6.0 && s.special === "None") targetEnding = 20;
                    else if (s.green >= 90 && s.magnetic >= 90) targetEnding = 23;
                    else if (s.oxygen >= 40 && s.oxygen <= 60 && s.water >= 50) targetEnding = 25;
                    else if (s.temp >= -40 && s.temp <= -10 && s.oxygen <= 10) targetEnding = 26;
                    else if (s.green >= 70 && s.oxygen >= 30) targetEnding = 28;
                    
                    // 표준 정답 (지구 분기)
                    else if (s.temp >= 10 && s.temp <= 25 && s.green >= 5 && s.green <= 20 && s.mass >= 0.8 && s.mass <= 1.5 && s.magnetic >= 40 && s.oxygen >= 15 && s.oxygen <= 25 && s.water >= 60 && s.water <= 85 && s.special === "None") {
                        targetEnding = 16;
                    }
                }

                // 매칭된 결말 그래픽 처리 및 모달 동기화
                const planet = document.getElementById("planet-graphic");
                const statusTxt = document.getElementById("planet-status-text");

                if (targetEnding) {
                    const endObj = endingDB.find(e => e.id === targetEnding);
                    planet.style.background = endObj.color;
                    planet.style.boxShadow = `0 0 40px ${endObj.isSuccess ? 'rgba(0,255,255,0.5)' : 'rgba(255,0,85,0.4)'}`;
                    statusTxt.innerText = `[⚠️ 관측 완료]: ${endObj.name}`;
                    statusTxt.style.color = endObj.isSuccess ? "#00ffff" : "#ff0055";

                    // 신규 해금 시 데이터 저장 배열 Push 및 모달 오픈
                    if (!gameState.unlockedEndings.includes(targetEnding)) {
                        gameState.unlockedEndings.push(targetEnding);
                        this.saveSliders(); // 상태 보존
                        navigation.triggerEndingModal(endObj);
                    }
                } else {
                    planet.style.background = "radial-gradient(circle at 30% 30%, #444455 0%, #0d0d1a 100%)";
                    planet.style.boxShadow = "inset -20px -20px 50px #000, 0 0 20px rgba(255,255,255,0.05)";
                    statusTxt.innerText = "안정적인 대사 활동 부재 - 유기물 결합 진화 추적 중...";
                    statusTxt.style.color = "#88a";
                }
            }
        };

        // 초기 시스템 부팅
        window.onload = () => {
            loadGame();
        };
    </script>
</body>
</html>
