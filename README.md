[Uploading index.html…]
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>포카리스트</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <h1>📸 포카리스트</h1>

  <div id="controls">
    <select id="groupSelect">
      <option value="">그룹 선택</option>
    </select>

    <select id="memberSelect">
      <option value="">멤버 선택</option>
    </select>
  </div>

  <div id="pocaList">
    <!-- 포카 리스트 출력 -->
  </div>

  <script src="script.js"></script>
</body>
</html>
()

const sampleData = {
  "NewJeans": {
    "민지": ["OMG Ver. A", "OMG Ver. B"],
    "하니": ["OMG Ver. A", "OMG Ver. B"]
  },
  "IVE": {
    "장원영": ["LOVE DIVE A", "LOVE DIVE B"],
    "리즈": ["LOVE DIVE A", "LOVE DIVE B"]
  }
};

const groupSelect = document.getElementById("groupSelect");
const memberSelect = document.getElementById("memberSelect");
const pocaList = document.getElementById("pocaList");

// 그룹 리스트 생성
for (let group in sampleData) {
  const option = document.createElement("option");
  option.value = group;
  option.textContent = group;
  groupSelect.appendChild(option);
}

// 그룹 선택 시 멤버 표시
groupSelect.addEventListener("change", () => {
  memberSelect.innerHTML = '<option value="">멤버 선택</option>';
  pocaList.innerHTML = '';
  const group = groupSelect.value;
  if (!group) return;

  const members = sampleData[group];
  for (let member in members) {
    const option = document.createElement("option");
    option.value = member;
    option.textContent = member;
    memberSelect.appendChild(option);
  }
});

// 멤버 선택 시 포카 리스트 출력
memberSelect.addEventListener("change", () => {
  pocaList.innerHTML = '';
  const group = groupSelect.value;
  const member = memberSelect.value;
  if (!group || !member) return;

  const cards = sampleData[group][member];
  cards.forEach((card, index) => {
    const checked = localStorage.getItem(`${group}-${member}-${card}`) === 'true';
    const div = document.createElement("div");
    div.className = "poca-card";
    div.innerHTML = `
      <div>${card}</div>
      <input type="checkbox" id="card-${index}" ${checked ? 'checked' : ''}/>
    `;
    const checkbox = div.querySelector("input");
    checkbox.addEventListener("change", () => {
      localStorage.setItem(`${group}-${member}-${card}`, checkbox.checked);
    });
    pocaList.appendChild(div);
  });
});

body {
  font-family: 'Arial', sans-serif;
  padding: 20px;
  background-color: #f4f4f4;
}

h1 {
  text-align: center;
}

#controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

#pocaList {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 10px;
  padding: 10px;
}

.poca-card {
  background-color: white;
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
}

.poca-card input {
  margin-top: 5px;
}
