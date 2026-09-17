<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bus-Tok | 5001-1번 스마트 솔루션</title>
  <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
  <style>
    :root {
      --bg: #f3f4f6;
      --primary: #2563eb;
      --card-bg: #ffffff;
      --text-main: #111111;
      --text-sub: #4b5563;
      --text-muted: #9ca3af;
      --danger: #ef4444;
      --border: #e5e7eb;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: "Pretendard", -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
      scroll-behavior: smooth;
    }

    body { background-color: var(--bg); color: var(--text-main); line-height: 1.6; }

    /* 상단 내비 */
    .top-header {
      width: 100%; height: 70px; background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(10px); border-bottom: 1px solid var(--border);
      display: flex; align-items: center; justify-content: space-between;
      padding: 0 10%; position: sticky; top: 0; z-index: 100;
    }
    .top-header .logo { font-size: 1.4rem; font-weight: 800; color: var(--primary); }
    .nav-links { display: flex; gap: 32px; list-style: none; }
    .nav-links a { text-decoration: none; color: var(--text-sub); font-weight: 600; }

    /* 히어로 */
    .hero { padding: 100px 10% 60px; text-align: center; background: white; }
    .hero h1 { font-size: 3rem; font-weight: 800; margin-bottom: 20px; letter-spacing: -0.04em; }
    .hero h1 span { color: var(--primary); }
    .btn-main {
      padding: 18px 36px; border-radius: 14px; border: none; background: var(--primary);
      color: #fff; font-weight: 700; font-size: 1.1rem; cursor: pointer; box-shadow: 0 10px 20px rgba(37, 99, 235, 0.2);
    }

    /* 데모 영역 */
    .demo-container { padding: 60px 20px; max-width: 1200px; margin: 0 auto; }
    .workspace { display: flex; flex-wrap: wrap; gap: 40px; justify-content: center; align-items: flex-start; }

    /* 승객용 휴대폰 */
    .phone-mockup {
      width: 380px; height: 720px; background: #fff; border-radius: 45px;
      border: 10px solid #111; display: flex; flex-direction: column;
      position: relative; box-shadow: 0 30px 60px rgba(0,0,0,0.1); overflow: hidden;
    }
    .phone-body { flex: 1; padding: 24px 20px 0; display: flex; flex-direction: column; overflow: hidden; }

    /* 하차 상태 카드 */
    .status-card {
      background: #fff; border: 1px solid var(--border); border-radius: 20px;
      padding: 18px; margin-bottom: 12px; transition: 0.3s;
    }
    .status-card.active { border-color: var(--danger); background: #fff8f8; }
    .status-indicator { font-size: 0.8rem; font-weight: 700; color: var(--text-muted); }
    .status-card.active .status-indicator { color: var(--danger); }
    .target-name { font-size: 1.3rem; font-weight: 800; }

    /* 광고(혜택) 영역 */
    .perk-section {
      display: none; flex-direction: column; gap: 8px; margin-bottom: 16px;
      animation: fadeIn 0.4s ease-out;
    }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
    
    .perk-header { display: flex; justify-content: space-between; align-items: center; padding: 0 4px; }
    .perk-label { font-size: 0.75rem; font-weight: 800; color: var(--primary); }
    .btn-view-all { background: none; border: none; color: var(--primary); font-weight: 700; font-size: 0.8rem; cursor: pointer; }

    .perk-card {
      background: #111; color: #fff; border-radius: 16px; padding: 16px;
      display: flex; justify-content: space-between; align-items: center; cursor: pointer;
    }
    .perk-title { font-size: 0.95rem; font-weight: 700; }

    /* 정류장 목록 (스크롤) */
    .list-section { flex: 1; overflow-y: auto; padding-bottom: 30px; }
    .list-section::-webkit-scrollbar { width: 4px; }
    .list-section::-webkit-scrollbar-thumb { background: #eee; border-radius: 10px; }

    .station-item {
      background: #fff; border: 1px solid var(--border); border-radius: 16px;
      padding: 14px 16px; display: flex; justify-content: space-between;
      align-items: center; margin-bottom: 8px; transition: 0.2s;
    }
    .station-item.passed { opacity: 0.4; background: #f9f9f9; }
    .station-item.selected { border: 2px solid var(--danger); }
    .btn-reserve {
      background: #111; color: #fff; border: none; border-radius: 10px;
      padding: 8px 12px; font-weight: 700; font-size: 0.8rem; cursor: pointer;
    }
    .station-item.selected .btn-reserve { background: var(--danger); }

    /* 바텀시트 모달 */
    .modal-overlay {
      position: absolute; inset: 0; background: rgba(0,0,0,0.5);
      display: none; z-index: 200; align-items: flex-end;
    }
    .modal-overlay.show { display: flex; }
    .modal-sheet {
      background: #fff; width: 100%; border-radius: 25px 25px 0 0;
      padding: 24px; animation: slideUp 0.3s ease-out;
    }
    @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
    .modal-item { border-bottom: 1px solid #eee; padding: 12px 0; }

    /* 운전석 단말기 */
    .bms-terminal {
      width: 400px; height: 720px; background: #111; border-radius: 40px;
      border: 8px solid #222; padding: 40px 30px; color: #fff; display: flex; flex-direction: column;
    }
    .terminal-stop {
      width: 200px; height: 200px; border-radius: 50%; background: #222;
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      margin: 60px auto; transition: 0.4s;
    }
    .terminal-stop.on { background: var(--danger); box-shadow: 0 0 40px rgba(239,68,68,0.5); }
    .terminal-stop-text { font-size: 2.2rem; font-weight: 900; color: #333; }
    .terminal-stop.on .terminal-stop-text { color: #fff; }
    .terminal-info { margin-top: auto; border-top: 1px solid #333; padding-top: 20px; }
    .t-row { display: flex; justify-content: space-between; margin-bottom: 12px; }
    .t-label { color: #777; font-size: 0.9rem; }
  </style>
</head>
<body>

  <header class="top-header">
    <div class="logo">Bus-Tok.</div>
    <ul class="nav-links">
      <li><a href="#demo">데모 실행</a></li>
      <li><a href="#">기술 소개</a></li>
    </ul>
  </header>

  <section class="hero">
    <h1>벨 누르지 마세요.<br><span>Bus-Tok</span>으로 예약하세요.</h1>
    <p>5001-1번 승객을 위한 스마트 하차 시스템. 미리 예약하고 정류장 혜택까지 받으세요.</p>
    <button class="btn-main" onclick="scrollToDemo()">지금 시작하기</button>
  </section>

  <div id="demo" class="demo-container">
    <div class="workspace">
      <!-- 휴대폰 목업 -->
      <div class="phone-mockup">
        <div class="phone-body">
          <div id="statusCard" class="status-card">
            <div class="status-indicator" id="statusLabel">운행 중</div>
            <div class="target-name" id="targetName">정류장을 선택하세요</div>
            <button id="btnCancel" onclick="cancelBooking()" style="display:none; width:100%; margin-top:10px; padding:8px; border-radius:8px; border:1px solid #ef4444; color:#ef4444; background:none; font-weight:700; cursor:pointer;">예약 취소</button>
          </div>

          <!-- 광고 섹션 (예약 시 나타남) -->
          <div id="perkSection" class="perk-section">
            <div class="perk-header">
              <span class="perk-label">LOCAL BENEFIT</span>
              <button class="btn-view-all" onclick="openModal()">전체보기 ›</button>
            </div>
            <div class="perk-card" onclick="openModal()">
              <div class="perk-title" id="mainPerkTitle">정류장 주변 혜택</div>
              <span style="color:#aaa;">›</span>
            </div>
          </div>

          <!-- 정류장 리스트 스크롤 -->
          <div class="list-section" id="stationList"></div>
        </div>

        <!-- 혜택 모달 -->
        <div id="adModal" class="modal-overlay" onclick="closeModal()">
          <div class="modal-sheet" onclick="event.stopPropagation()">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px;">
              <h3 id="modalTargetName" style="font-weight:800;">혜택 상세</h3>
              <button onclick="closeModal()" style="border:none; background:none; font-size:1.2rem; cursor:pointer;">✕</button>
            </div>
            <div id="modalList"></div>
          </div>
        </div>
      </div>

      <!-- BMS 단말기 -->
      <div class="bms-terminal">
        <div style="text-align:center; color:#444; font-weight:800; font-size:0.7rem;">5001-1 SMART BUS</div>
        <div class="terminal-stop" id="termStop">
          <div class="terminal-stop-text">STOP</div>
          <div id="termStopSub" style="font-weight:700; color:#333; font-size:0.8rem;">STANDBY</div>
        </div>
        <div class="terminal-info">
          <div class="t-row"><span class="t-label">노선</span><span style="font-weight:700; color:#3b82f6;">직행 5001-1</span></div>
          <div class="t-row"><span class="t-label">다음 정류장</span><span id="termNext" style="font-weight:700;">이마트·상공회의소</span></div>
          <div class="t-row"><span class="t-label">예약 인원</span><span id="termCount" style="font-weight:700;">0 명</span></div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const stations = [
      { id: 1, name: "명지대학교 (기점)", passed: true, perks: [] },
      { id: 2, name: "이마트·상공회의소", passed: false, perks: [{t: "이마트 3,000원 할인권", d: "5만원 이상 구매 시"}, {t: "이디야 사이즈업", d: "하차 후 1시간 내"}] },
      { id: 3, name: "역북동행정복지센터", passed: false, perks: [{t: "역북 카페 10% 할인", d: "Bus-Tok 승객 전용"}] },
      { id: 5, name: "용인대입구", passed: false, perks: [{t: "대학가 토스트 500원 할인", d: "학생증 지참 시"}] },
      { id: 7, name: "삼가역·두산위브", passed: false, perks: [{t: "삼가역 헬스 1일권", d: "무료 체험 바코드"}] },
      { id: 10, name: "한국민속촌", passed: false, perks: [{t: "민속촌 입장권 20% 할인", d: "모바일 예약증 제시"}, {t: "전통찻집 식혜 무료", d: "식사 시"}] },
      { id: 12, name: "상갈역", passed: false, perks: [{t: "편의점 음료 1+1", d: "상갈역점 단독"}] },
      { id: 16, name: "신논현역", passed: false, perks: [{t: "교보문고 2,000원권", d: "도서 구매 시"}] },
      { id: 17, name: "강남역 (중)", passed: false, perks: [{t: "스타벅스 픽업 주문", d: "도착 5분 전 자동연결"}, {t: "올리브영 증정권", d: "강남타운점 전용"}] },
      { id: 19, name: "양재역", passed: false, perks: [{t: "SPC스퀘어 15% 할인", d: "해피포인트 앱 연동"}] },
      { id: 21, name: "양재시민의숲 (종점)", passed: false, perks: [{t: "꽃시장 10% 쿠폰", d: "전 품목 적용"}] }
    ];

    let selectedId = null;

    function scrollToDemo() { document.getElementById('demo').scrollIntoView(); }

    function renderStations() {
      const list = document.getElementById("stationList");
      list.innerHTML = "";
      stations.forEach(st => {
        const item = document.createElement("div");
        item.className = `station-item ${st.passed ? 'passed' : ''} ${selectedId === st.id ? 'selected' : ''}`;
        item.innerHTML = `
          <div><div style="font-weight:700;">${st.name}</div><div style="font-size:0.75rem; color:#999;">${st.passed?'통과':'정상운행'}</div></div>
          ${!st.passed ? `<button class="btn-reserve" onclick="bookStation(${st.id})">${selectedId===st.id?'취소':'예약'}</button>` : ''}
        `;
        list.appendChild(item);
      });
    }

    function bookStation(id) {
      if (selectedId === id) { cancelBooking(); return; }
      selectedId = id;
      const st = stations.find(s => s.id === id);

      // 승객 UI 업데이트
      document.getElementById("statusCard").classList.add("active");
      document.getElementById("statusLabel").innerText = "하차 예약 접수됨";
      document.getElementById("targetName").innerText = st.name;
      document.getElementById("btnCancel").style.display = "block";

      // 광고(혜택) 로직 처리
      const perkSec = document.getElementById("perkSection");
      if (st.perks && st.perks.length > 0) {
        perkSec.style.display = "flex";
        document.getElementById("mainPerkTitle").innerText = st.perks[0].t;
      } else {
        perkSec.style.display = "none";
      }

      // BMS 업데이트
      const stop = document.getElementById("termStop");
      stop.classList.add("on");
      document.getElementById("termStopSub").innerText = st.name;
      document.getElementById("termNext").innerText = st.name;
      document.getElementById("termCount").innerText = "1 명";
      document.getElementById("termCount").style.color = "#ef4444";

      renderStations();
    }

    function cancelBooking() {
      selectedId = null;
      document.getElementById("statusCard").classList.remove("active");
      document.getElementById("statusLabel").innerText = "운행 중";
      document.getElementById("targetName").innerText = "정류장을 선택하세요";
      document.getElementById("btnCancel").style.display = "none";
      document.getElementById("perkSection").style.display = "none";

      const stop = document.getElementById("termStop");
      stop.classList.remove("on");
      document.getElementById("termStopSub").innerText = "STANDBY";
      document.getElementById("termCount").innerText = "0 명";
      document.getElementById("termCount").style.color = "#fff";
      renderStations();
    }

    function openModal() {
      const st = stations.find(s => s.id === selectedId);
      document.getElementById("modalTargetName").innerText = st.name + " 혜택";
      const mList = document.getElementById("modalList");
      mList.innerHTML = "";
      st.perks.forEach(p => {
        mList.innerHTML += `<div class="modal-item"><div style="font-weight:700;">${p.t}</div><div style="font-size:0.85rem; color:#666;">${p.d}</div></div>`;
      });
      document.getElementById("adModal").classList.add("show");
    }

    function closeModal() { document.getElementById("adModal").classList.remove("show"); }

    renderStations();
  </script>
</body>
</html>