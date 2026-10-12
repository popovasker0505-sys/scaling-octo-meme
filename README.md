<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Аскер — Мои расходы</title>

  <link rel="icon" type="image/png" href="asker-logo.png">
  <link rel="apple-touch-icon" href="asker-logo.png">

  <style>
    :root {
      --bg: #f4f6fb;
      --card: #ffffff;
      --text: #20243a;
      --muted: #778099;
      --primary: #6257e8;
      --green: #16885d;
      --red: #d64d5c;
      --border: #e9ebf2;
      --shadow: 0 8px 28px rgba(33, 39, 75, .06);
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font-family: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    .container {
      width: min(1100px, calc(100% - 32px));
      margin: 34px auto;
    }

    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      margin-bottom: 26px;
    }

    h1 { margin: 0; font-size: 28px; }
    h2 { font-size: 18px; margin: 0 0 18px; }
    .subtitle, .muted { color: var(--muted); }
    .subtitle { margin: 7px 0 0; }

    button, input, select { font: inherit; }

    button {
      border: 0;
      cursor: pointer;
      border-radius: 10px;
      padding: 11px 15px;
      transition: .15s;
    }

    button:hover { filter: brightness(.96); }
    .primary { background: var(--primary); color: white; }
    .secondary { background: #eeedff; color: var(--primary); }
    .danger { background: #fff0f1; color: var(--red); }

    .header-actions {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    input, select {
      width: 100%;
      min-height: 43px;
      padding: 10px 12px;
      background: #fff;
      border: 1px solid var(--border);
      border-radius: 9px;
      color: var(--text);
      outline: none;
    }

    input:focus, select:focus { border-color: var(--primary); }
    .month-input { width: auto; }

    .brand {
      display: flex;
      align-items: center;
      gap: 14px;
    }

    .brand-logo {
      height: 58px;
      width: auto;
      display: block;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
      margin-bottom: 18px;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 20px;
      box-shadow: var(--shadow);
    }

    .stat-label { color: var(--muted); font-size: 14px; }
    .stat-value { margin-top: 9px; font-size: 26px; font-weight: 700; }
    .income { color: var(--green); }
    .expense { color: var(--red); }

    .content-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 18px;
      align-items: start;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 13px;
    }

    .field { display: flex; flex-direction: column; gap: 7px; }
    .field label { font-size: 13px; color: var(--muted); }
    .full { grid-column: 1 / -1; }
    .form-submit { width: 100%; margin-top: 15px; }

    .transaction-list { display: flex; flex-direction: column; gap: 10px; }

    .transaction {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px 13px;
    }

    .transaction-title { font-weight: 600; }
    .transaction-meta { margin-top: 4px; font-size: 12px; color: var(--muted); }
    .transaction-right { display: flex; align-items: center; gap: 10px; }
    .transaction-amount { font-weight: 700; white-space: nowrap; }
    .delete-btn { padding: 7px 9px; font-size: 12px; }

    .empty {
      padding: 18px;
      color: var(--muted);
      text-align: center;
      border: 1px dashed var(--border);
      border-radius: 12px;
    }

    /* Диаграмма */
    .chart-wrapper {
      position: relative;
      aspect-ratio: 1;
      max-width: 300px;
      margin: 0 auto;
    }
    canvas {
      display: block;
      width: 100%;
      height: 100%;
    }
    .legend {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: center;
      margin-top: 16px;
    }
    .legend-item {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 13px;
    }
    .legend-color {
      width: 14px;
      height: 14px;
      border-radius: 4px;
    }

    .overlay {
      position: fixed;
      inset: 0;
      z-index: 10;
      display: none;
      align-items: center;
      justify-content: center;
      padding: 18px;
      background: rgba(20, 24, 45, .5);
    }
    .overlay.open { display: flex; }

    .modal {
      width: min(540px, 100%);
      max-height: 90vh;
      overflow: auto;
      padding: 24px;
      background: white;
      border-radius: 18px;
      box-shadow: 0 20px 70px rgba(0,0,0,.2);
    }
    .checks { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin: 16px 0; }
    .check {
      display: flex;
      align-items: center;
      gap: 9px;
      padding: 11px;
      border: 1px solid var(--border);
      border-radius: 10px;
    }
    .check input { width: 17px; min-height: 17px; }
    .modal-actions { display: flex; justify-content: flex-end; gap: 9px; margin-top: 18px; }
    .notice { font-size: 12px; color: var(--muted); margin-top: 12px; }

    @media (max-width: 760px) {
      .grid { grid-template-columns: 1fr; }
      .content-grid { grid-template-columns: 1fr; }
      header { align-items: flex-start; flex-direction: column; }
      .header-actions { width: 100%; }
      .month-input { flex: 1; }
    }
    @media (max-width: 420px) {
      .form-grid, .checks { grid-template-columns: 1fr; }
      .stat-value { font-size: 23px; }
      .brand-logo { height: 44px; }
      .container { width: min(100% - 20px, 1100px); margin-top: 22px; }
    }
  </style>
</head>
<body>
  <main class="container">
    <header>
      <div class="brand">
        <img src="asker-logo.png" alt="Аскер — Финансы" class="brand-logo">
        <p class="subtitle">Простой контроль личных финансов</p>
      </div>
      <div class="header-actions">
        <input id="monthFilter" class="month-input" type="month" aria-label="Выбрать месяц">
        <button id="settingsButton" class="secondary">Настроить</button>
      </div>
    </header>

    <section class="grid">
      <div class="card">
        <div class="stat-label">Доходы за месяц</div>
        <div id="incomeTotal" class="stat-value income">0 ₽</div>
      </div>
      <div class="card">
        <div class="stat-label">Расходы за месяц</div>
        <div id="expenseTotal" class="stat-value expense">0 ₽</div>
      </div>
      <div class="card">
        <div class="stat-label">Остаток за месяц</div>
        <div id="balanceTotal" class="stat-value">0 ₽</div>
      </div>
    </section>

    <section class="content-grid">
      <div class="card">
        <h2>Добавить операцию</h2>
        <form id="transactionForm">
          <div class="form-grid">
            <div class="field">
              <label for="type">Тип операции</label>
              <select id="type">
                <option value="expense">Расход</option>
                <option value="income">Доход</option>
              </select>
            </div>
            <div class="field">
              <label for="amount">Сумма, ₽</label>
              <input id="amount" type="number" min="1" step="0.01" placeholder="Например, 1500" required>
            </div>
            <div class="field">
              <label for="category">Категория</label>
              <select id="category"></select>
            </div>
            <div class="field">
              <label for="date">Дата</label>
              <input id="date" type="date" required>
            </div>
            <div class="field full">
              <label for="description">Комментарий (необязательно)</label>
              <input id="description" type="text" maxlength="80" placeholder="Например, продукты на неделю">
            </div>
          </div>
          <button class="primary form-submit" type="submit">Добавить операцию</button>
        </form>
      </div>

      <div class="card">
        <h2>Расходы по категориям (диаграмма)</h2>
        <div class="chart-wrapper">
          <canvas id="pieChart"></canvas>
        </div>
        <div id="chartLegend" class="legend"></div>
      </div>

      <div class="card full">
        <h2>Операции за выбранный месяц</h2>
        <div id="transactionList" class="transaction-list"></div>
      </div>
    </section>
  </main>

  <div id="setupOverlay" class="overlay">
    <section class="modal" role="dialog" aria-modal="true" aria-labelledby="setupTitle">
      <h2 id="setupTitle">Настроим приложение</h2>
      <p>Отметь, какие расходы относятся к твоей жизни. Их категории появятся в списке операций. Можно изменить выбор позже.</p>

      <div class="checks">
        <label class="check"><input type="checkbox" value="rent"> Снимаю жильё</label>
        <label class="check"><input type="checkbox" value="car"> Есть автомобиль</label>
        <label class="check"><input type="checkbox" value="children"> Есть дети</label>
        <label class="check"><input type="checkbox" value="pets"> Есть питомцы</label>
        <label class="check"><input type="checkbox" value="subscriptions"> Есть подписки</label>
      </div>

      <div class="modal-actions">
        <button id="saveSetup" class="primary">Сохранить настройки</button>
      </div>
      <div class="notice">Данные хранятся только в этом браузере на твоём устройстве.</div>
    </section>
  </div>

  <script>
    const STORAGE_KEY = "my-expenses-app-v1";

    const baseCategories = {
      expense: [
        "Продукты",
        "Транспорт",
        "Кафе и рестораны",
        "Здоровье и лекарства",
        "Одежда и обувь",
        "Развлечения и хобби",
        "Электроника и техника",
        "Ремонт и стройматериалы",
        "Коммунальные услуги",
        "Связь и интернет",
        "Образование и курсы",
        "Подарки и праздники",
        "Налоги и штрафы",
        "Спорт и фитнес",
        "Косметика и гигиена",
        "Книги и медиа",
        "Прочее"
      ],
      income: [
        "Зарплата",
        "Подработка",
        "Подарок",
        "Возврат долга",
        "Прочий доход"
      ]
    };

    const extraCategories = {
      rent: "Аренда жилья",
      car: "Автомобиль и топливо",
      children: "Расходы на детей",
      pets: "Расходы на питомцев",
      subscriptions: "Подписки и абонементы"
    };

    function loadState() {
      try {
        const saved = JSON.parse(localStorage.getItem(STORAGE_KEY));
        return saved && typeof saved === "object"
          ? { setupDone: false, extras: [], transactions: [], ...saved }
          : { setupDone: false, extras: [], transactions: [] };
      } catch {
        return { setupDone: false, extras: [], transactions: [] };
      }
    }

    let state = loadState();

    const $ = id => document.getElementById(id);
    const today = new Date();
    const todayString = [
      today.getFullYear(),
      String(today.getMonth() + 1).padStart(2, "0"),
      String(today.getDate()).padStart(2, "0")
    ].join("-");

    $("date").value = todayString;
    $("monthFilter").value = todayString.slice(0, 7);

    function saveState() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }

    function formatMoney(value) {
      return new Intl.NumberFormat("ru-RU", {
        maximumFractionDigits: 2
      }).format(value) + " ₽";
    }

    function escapeHtml(value) {
      return String(value).replace(/[&<>"']/g, char => ({
        "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#039;"
      })[char]);
    }

    function getCategories(type) {
      const categories = [...baseCategories[type]];
      if (type === "expense") {
        state.extras.forEach(key => {
          if (extraCategories[key]) categories.push(extraCategories[key]);
        });
      }
      return categories;
    }

    function updateCategoryOptions() {
      const type = $("type").value;
      const current = $("category").value;
      $("category").innerHTML = getCategories(type)
        .map(name => `<option value="${escapeHtml(name)}">${escapeHtml(name)}</option>`)
        .join("");

      if (getCategories(type).includes(current)) {
        $("category").value = current;
      } else {
        $("category").selectedIndex = 0;
      }
    }

    updateCategoryOptions();

    $("type").addEventListener("change", updateCategoryOptions);
    $("monthFilter").addEventListener("change", render);

    $("transactionForm").addEventListener("submit", event => {
      event.preventDefault();

      const amount = Number($("amount").value);
      if (!Number.isFinite(amount) || amount <= 0) {
        alert("Введи сумму больше нуля.");
        return;
      }

      state.transactions.push({
        id: (crypto.randomUUID ? crypto.randomUUID() : String(Date.now() + Math.random())),
        type: $("type").value,
        amount,
        category: $("category").value,
        description: $("description").value.trim(),
        date: $("date").value,
        createdAt: Date.now()
      });

      saveState();
      $("amount").value = "";
      $("description").value = "";
      render();
    });

    function render() {
      const transactions = getMonthTransactions();
      const income = transactions
        .filter(item => item.type === "income")
        .reduce((sum, item) => sum + Number(item.amount), 0);
      const expense = transactions
        .filter(item => item.type === "expense")
        .reduce((sum, item) => sum + Number(item.amount), 0);

      $("incomeTotal").textContent = formatMoney(income);
      $("expenseTotal").textContent = formatMoney(expense);
      $("balanceTotal").textContent = formatMoney(income - expense);
      $("balanceTotal").className = "stat-value " + (income - expense < 0 ? "expense" : "income");

      renderTransactions(transactions);
      drawPieChart(transactions);
    }

    function getMonthTransactions() {
      const month = $("monthFilter").value;
      return state.transactions
        .filter(item => item.date.startsWith(month))
        .sort((a, b) => b.date.localeCompare(a.date) || b.createdAt - a.createdAt);
    }

    function renderTransactions(transactions) {
      const list = $("transactionList");

      if (!transactions.length) {
        list.innerHTML = '<div class="empty">За этот месяц операций пока нет.</div>';
        return;
      }

      list.innerHTML = transactions.map(item => {
        const isIncome = item.type === "income";
        const title = item.description || item.category;
        return `
          <div class="transaction">
            <div>
              <div class="transaction-title">${escapeHtml(title)}</div>
              <div class="transaction-meta">${escapeHtml(item.category)} · ${escapeHtml(item.date)}</div>
            </div>
            <div class="transaction-right">
              <div class="transaction-amount ${isIncome ? "income" : "expense"}">
                ${isIncome ? "+" : "−"}${formatMoney(Number(item.amount))}
              </div>
              <button class="danger delete-btn" data-delete="${escapeHtml(item.id)}" aria-label="Удалить операцию">Удалить</button>
            </div>
          </div>
        `;
      }).join("");

      list.querySelectorAll("[data-delete]").forEach(button => {
        button.addEventListener("click", () => {
          state.transactions = state.transactions.filter(item => item.id !== button.dataset.delete);
          saveState();
          render();
        });
      });
    }

    const palette = [
      "#6257e8", "#f59e0b", "#10b981", "#ef4444", "#3b82f6",
      "#8b5cf6", "#ec4899", "#14b8a6", "#f97316", "#64748b",
      "#84cc16", "#e879f9", "#06b6d4", "#a855f7", "#dc2626",
      "#0ea5e9", "#65a30d", "#d946ef", "#f43f5e", "#7c3aed"
    ];

    function categoryColor(name) {
      let hash = 0;
      for (let i = 0; i < name.length; i++) {
        hash = (hash * 31 + name.charCodeAt(i)) >>> 0;
      }
      return palette[hash % palette.length];
    }

    function drawPieChart(transactions) {
      const canvas = $("pieChart");
      const ctx = canvas.getContext("2d");

      const expenses = transactions.filter(item => item.type === "expense");
      const totals = {};
      expenses.forEach(item => {
        totals[item.category] = (totals[item.category] || 0) + Number(item.amount);
      });

      const entries = Object.entries(totals).sort((a, b) => b[1] - a[1]);

      const cssSize = canvas.clientWidth || 300;
      const dpr = window.devicePixelRatio || 1;
      canvas.width = cssSize * dpr;
      canvas.height = cssSize * dpr;
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
      ctx.clearRect(0, 0, cssSize, cssSize);

      const legend = $("chartLegend");

      if (!entries.length) {
        ctx.fillStyle = "#778099";
        ctx.font = "14px Inter, sans-serif";
        ctx.textAlign = "center";
        ctx.fillText("Нет расходов в этом месяце", cssSize / 2, cssSize / 2);
        legend.innerHTML = "";
        return;
      }

      const sum = entries.reduce((acc, [, value]) => acc + value, 0);
      const centerX = cssSize / 2;
      const centerY = cssSize / 2;
      const radius = cssSize / 2 - 8;
      const innerRadius = radius * 0.55;

      let startAngle = -Math.PI / 2;

      entries.forEach(([name, value]) => {
        const sliceAngle = (value / sum) * Math.PI * 2;
        const endAngle = startAngle + sliceAngle;
        const color = categoryColor(name);

        ctx.beginPath();
        ctx.moveTo(centerX, centerY);
        ctx.arc(centerX, centerY, radius, startAngle, endAngle);
        ctx.closePath();
        ctx.fillStyle = color;
        ctx.fill();
        ctx.strokeStyle = "#ffffff";
        ctx.lineWidth = 2;
        ctx.stroke();

        startAngle = endAngle;
      });

      ctx.beginPath();
      ctx.arc(centerX, centerY, innerRadius, 0, Math.PI * 2);
      ctx.fillStyle = "#ffffff";
      ctx.fill();

      ctx.fillStyle = "#20243a";
      ctx.font = "bold 16px Inter, sans-serif";
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.fillText(formatMoney(sum), centerX, centerY);

      legend.innerHTML = entries.map(([name, value]) => `
        <div class="legend-item">
          <span class="legend-color" style="background:${categoryColor(name)}"></span>
          <span>${escapeHtml(name)} · ${Math.round(value / sum * 100)}%</span>
        </div>
      `).join("");
    }

    window.addEventListener("resize", render);

    function openSetup() {
      document.querySelectorAll('#setupOverlay input[type="checkbox"]').forEach(box => {
        box.checked = state.extras.includes(box.value);
      });
      $("setupOverlay").classList.add("open");
    }

    $("settingsButton").addEventListener("click", openSetup);

    $("saveSetup").addEventListener("click", () => {
      state.extras = [...document.querySelectorAll('#setupOverlay input[type="checkbox"]:checked')]
        .map(box => box.value);
      state.setupDone = true;

      saveState();
      updateCategoryOptions();
      $("setupOverlay").classList.remove("open");
      render();
    });

    render();

    if (!state.setupDone) openSetup();
  </script>
</body>
</html>
