<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>공부 유형 검사기</title>
  <link rel="stylesheet" href="style.css"/>
</head>
<body>
  <div class="container">
    <h1>📘 공부 유형 검사기</h1>

    <form id="quizForm">
      <div class="question">
        <p>1. 공부 계획을 미리 세우고 계획에 맞춰 공부하는 편인가요?</p>
        <label><input type="radio" name="q1" value="y" required> 예</label>
        <label><input type="radio" name="q1" value="n"> 아니요</label>
      </div>

      <div class="question">
        <p>2. 공부할 때, 개념을 구조화시켜 이해하는 편인가요?</p>
        <label><input type="radio" name="q2" value="y" required> 예</label>
        <label><input type="radio" name="q2" value="n"> 아니요</label>
      </div>

      <div class="question">
        <p>3. 친구와 함께 공부할 때, 더 집중이 잘 되나요?</p>
        <label><input type="radio" name="q3" value="y" required> 예</label>
        <label><input type="radio" name="q3" value="n"> 아니요</label>
      </div>

      <div class="question">
        <p>4. 한번 공부를 시작하면 1시간 이상 집중을 이어갈 수 있나요?</p>
        <label><input type="radio" name="q4" value="y" required> 예</label>
        <label><input type="radio" name="q4" value="n"> 아니요</label>
      </div>

      <button type="submit">결과 보기</button>
    </form>

    <div id="result" class="hidden">
      <h2>🎯 결과</h2>
      <p id="typeCode"></p>
      <p id="typeDescription"></p>
      <p id="studyTip"></p>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>

body {
  font-family: 'Arial', sans-serif;
  background: #f2f2f2;
  padding: 2rem;
}
.container {
  max-width: 600px;
  margin: auto;
  background: white;
  padding: 2rem;
  border-radius: 10px;
}
.question {
  margin-bottom: 1.5rem;
}
button {
  padding: 0.7rem 1.5rem;
  font-size: 1rem;
}
.hidden {
  display: none;
}
const descriptions = {
  "PILS": "계획적으로 혼자 공부하며, 필기 정리를 통해 짧은 시간에 집중하는 분석적 몰입형 학습자입니다.",
  "PILA": "계획적으로 혼자 공부하며, 필기 정리를 통해 긴 시간 몰입하는 자기주도형 학습자입니다.",
  "PIRS": "계획적으로 혼자 공부하고, 읽기를 중심으로 짧게 집중하는 독서 기반 학습자입니다.",
  "PIRA": "계획적으로 혼자 공부하고, 읽기를 통해 오래 집중하는 탐구형 학습자입니다.",
  "PALS": "계획적으로 친구와 함께 공부하며 필기 정리를 통해 짧게 집중하는 협동 정리형 학습자입니다.",
  "PALA": "계획적으로 친구와 함께 공부하며 필기 정리를 통해 오래 집중하는 전략적 협동 학습자입니다.",
  "PARS": "계획적으로 친구와 함께 읽기를 중심으로 짧게 집중하는 읽기 협동형 학습자입니다.",
  "PARA": "계획적으로 친구와 함께 읽기를 중심으로 오래 집중하는 탐구 협동형 학습자입니다.",
  "FILS": "자율적으로 혼자 공부하며, 필기 정리를 통해 짧게 집중하는 자유 몰입형 학습자입니다.",
  "FILA": "자율적으로 혼자 공부하며, 필기를 통해 오래 집중하는 유연한 자기주도형입니다.",
  "FIRS": "자율적으로 혼자 읽기를 통해 짧게 공부하는 감각형 학습자입니다.",
  "FIRA": "자율적으로 혼자 읽기를 통해 오래 집중하는 자유 독립형 학습자입니다.",
  "FALS": "자율적으로 친구와 함께 공부하며 필기 정리를 통해 짧게 집중하는 협업형 학습자입니다.",
  "FALA": "자율적으로 친구와 함께 공부하며 필기를 통해 오래 집중하는 사회적 집중형 학습자입니다.",
  "FARS": "자율적으로 친구와 함께 읽기를 중심으로 짧게 공부하는 가벼운 협동형 학습자입니다.",
  "FARA": "자율적으로 친구와 함께 읽기를 중심으로 오래 집중하는 자유 협업형 학습자입니다."
};

const studyTips = {
  "PILS": "✅ 하루 학습 계획을 시간 단위로 세우고, 정리 노트나 마인드맵을 활용해 개념을 시각화하세요.",
  "PILA": "✅ 주간 단위 플래너를 사용하고, Pomodoro 기법으로 장시간 집중을 훈련하세요.",
  "PIRS": "✅ 계획표에 독서 시간을 포함하고, 읽은 내용을 짧게 요약해보세요.",
  "PIRA": "✅ 학습 후 요점 필기를 하지 말고, 스스로 요약 에세이를 작성해보세요.",
  "PALS": "✅ 친구와 역할을 나눠 함께 정리하고, 짧은 시간 문제풀이를 함께 하세요.",
  "PALA": "✅ 친구와 스터디 그룹을 꾸려 공부 계획을 공유하고, 긴 시간 집중을 분담하세요.",
  "PARS": "✅ 읽은 내용을 친구에게 설명하는 형식으로 함께 정리해보세요.",
  "PARA": "✅ 긴 독서 시간이 필요한 과목을 스터디로 나눠서 같이 읽으세요.",
  "FILS": "✅ 필기할 때 색상과 아이콘을 다양하게 활용해 흥미를 유지하세요.",
  "FILA": "✅ 자율 루틴을 만들고, 하루에 1~2시간 몰입할 수 있는 환경(카페, 독서실 등)을 찾아보세요.",
  "FIRS": "✅ 짧은 시간 자주 반복하는 '슬라이스 독서법'으로 학습량을 유지하세요.",
  "FIRA": "✅ 긴 독서 시간을 확보해 흐름을 유지하고, 느낀 점이나 요약을 기록장에 남겨보세요.",
  "FALS": "✅ 친구와 번갈아 가며 개념을 설명해주고, 간단한 필기 퀴즈를 만들어 보세요.",
  "FALA": "✅ 협업 노트를 만들어 같이 정리하고, 타이머를 공유해 집중력을 유지하세요.",
  "FARS": "✅ 함께 읽고 간단한 요약을 공유하는 독서 스터디에 참여해보세요.",
  "FARA": "✅ 긴 시간 동안 함께 읽고 토론하는 활동이 효과적이에요. 그룹 학습을 적극 활용하세요."
};

document.getElementById("quizForm").addEventListener("submit", function(e) {
  e.preventDefault();

  const q1 = document.querySelector('input[name="q1"]:checked').value;
  const q2 = document.querySelector('input[name="q2"]:checked').value;
  const q3 = document.querySelector('input[name="q3"]:checked').value;
  const q4 = document.querySelector('input[name="q4"]:checked').value;

  let code = "";
  code += (q1 === "y") ? "P" : "F";
  code += (q2 === "y") ? "I" : "R";
  code += (q3 === "y") ? "A" : "L";
  code += (q4 === "y") ? "A" : "S";

  document.getElementById("typeCode").innerText = `유형 코드: ${code}`;
  document.getElementById("typeDescription").innerText = `📄 설명: ${descriptions[code] || "설명이 준비되지 않았습니다."}`;
  document.getElementById("studyTip").innerText = `${studyTips[code] || "공부법이 준비되지 않았습니다."}`;

  document.getElementById("result").classList.remove("hidden");
});
