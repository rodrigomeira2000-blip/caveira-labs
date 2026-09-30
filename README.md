<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ciclo de Estudos Interativo</title>
  <!-- Chart.js para o gráfico de pizza -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-color: #f8fafc;
      --card-bg: #ffffff;
      --text-color: #1e293b;
      --text-muted: #64748b;
      --primary: #3b82f6;
      --border: #e2e8f0;
      --radius: 12px;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Inter', sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-color);
      padding: 2rem 1rem;
      max-width: 1200px;
      margin: 0 auto;
    }

    header {
      margin-bottom: 2rem;
      text-align: center;
    }

    header h1 {
      font-size: 2rem;
      font-weight: 700;
      color: #0f172a;
    }

    header p {
      color: var(--text-muted);
      margin-top: 0.25rem;
    }

    .dashboard {
      display: grid;
      grid-template-columns: 1fr;
      gap: 1.5rem;
    }

    @media (min-width: 900px) {
      .dashboard {
        grid-template-columns: 1fr 1fr;
      }
    }

    .card {
      background: var(--card-bg);
      border-radius: var(--radius);
      padding: 1.5rem;
      border: 1px solid var(--border);
      box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
    }

    .card h2 {
      font-size: 1.25rem;
      margin-bottom: 1rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    /* Progresso */
    .progress-section {
      margin-bottom: 1.5rem;
    }

    .progress-info {
      display: flex;
      justify-content: space-between;
      margin-bottom: 0.5rem;
      font-weight: 500;
      font-size: 0.9rem;
    }

    .progress-bar-container {
      width: 100%;
      height: 12px;
      background-color: #e2e8f0;
      border-radius: 999px;
      overflow: hidden;
    }

    .progress-bar {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #3b82f6, #10b981);
      transition: width 0.4s ease;
    }

    /* Gráfico */
    .chart-container {
      position: relative;
      height: 300px;
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    /* Formulario */
    form {
      display: flex;
      flex-direction: column;
      gap: 0.75rem;
      margin-bottom: 1.5rem;
    }

    .input-group {
      display: flex;
      gap: 0.5rem;
    }

    input[type="text"], input[type="number"] {
      padding: 0.6rem 0.8rem;
      border: 1px solid var(--border);
      border-radius: 6px;
      font-size: 0.9rem;
      outline: none;
      width: 100%;
    }

    input:focus {
      border-color: var(--primary);
    }

    button {
      padding: 0.6rem 1rem;
      background-color: var(--primary);
      color: white;
      border: none;
      border-radius: 6px;
      font-weight: 500;
      cursor: pointer;
      transition: background 0.2s;
    }

    button:hover {
      filter: brightness(0.9);
    }

    .btn-danger {
      background-color: #ef4444;
    }

    .btn-secondary {
      background-color: #64748b;
    }

    /* Lista de Matérias */
    .subject-list {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      max-height: 500px;
      overflow-y: auto;
    }

    .subject-item {
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 1rem;
      border-left: 6px solid var(--primary);
      background: #fafafa;
    }

    .subject-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 0.5rem;
    }

    .subject-title {
      font-weight: 600;
      font-size: 1rem;
    }

    .subject-meta {
      font-size: 0.85rem;
      color: var(--text-muted);
    }

    .topic-list {
      margin-top: 0.5rem;
      padding-left: 1rem;
      font-size: 0.85rem;
    }

    .topic-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0.2rem 0;
    }

    .topic-add-form {
      display: flex;
      gap: 0.4rem;
      margin-top: 0.5rem;
    }

    .topic-add-form input {
      padding: 0.3rem 0.5rem;
      font-size: 0.8rem;
    }

    .topic-add-form button {
      padding: 0.3rem 0.6rem;
      font-size: 0.8rem;
    }

    .actions {
      display: flex;
      gap: 0.4rem;
      align-items: center;
    }

    .btn-sm {
      padding: 0.25rem 0.5rem;
      font-size: 0.75rem;
    }

    /* Timer Modal / Area */
    .timer-card {
      margin-top: 1.5rem;
      text-align: center;
      background: #f1f5f9;
      padding: 1rem;
      border-radius: 8px;
    }

    .timer-display {
      font-size: 1.8rem;
      font-weight: 700;
      margin: 0.5rem 0;
      font-family: monospace;
    }
  </style>
</head>
<body>

  <header>
    <h1>Ciclo de Estudos Interativo</h1>
    <p>Organize suas matérias, acompanhe seu progresso e mantenha o ritmo.</p>
  </header>

  <div class="card progress-section">
    <div class="progress-info">
      <span>Progresso Total do Ciclo</span>
      <span id="progress-text">0 / 0 horas (0%)</span>
    </div>
    <div class="progress-bar-container">
      <div class="progress-bar" id="progress-bar"></div>
    </div>
  </div>

  <div class="dashboard">
    <!-- Coluna Esquerda: Gráfico e Sessão de Estudo -->
    <div>
      <div class="card">
        <h2>Visualização do Ciclo</h2>
        <div class="chart-container">
          <canvas id="cycleChart"></canvas>
        </div>
      </div>

      <div class="card timer-card">
        <h3>Cronômetro de Estudo</h3>
        <p id="timer-subject" style="color: var(--text-muted); font-size: 0.9rem;">Selecione "Estudar" em uma matéria abaixo</p>
        <div class="timer-display" id="timer-display">00:00:00</div>
        <div style="display: flex; justify-content: center; gap: 0.5rem;">
          <button id="btn-start" disabled>Iniciar</button>
          <button id="btn-pause" class="btn-secondary" disabled>Pausar</button>
          <button id="btn-save" class="btn-secondary" disabled>Concluir e Salvar Hora</button>
        </div>
      </div>
    </div>

    <!-- Coluna Direita: Gerenciamento de Matérias -->
    <div class="card">
      <h2>Matérias do Ciclo</h2>

      <form id="subject-form">
        <input type="text" id="subject-name" placeholder="Nome da Matéria (ex: Direito Constitucional)" required>
        <div class="input-group">
          <input type="number" id="subject-hours" placeholder="Carga Horária (horas)" min="0.5" step="0.5" required>
          <input type="color" id="subject-color" value="#3b82f6" style="width: 50px; height: 38px; padding: 2px; cursor: pointer;">
        </div>
        <button type="submit">Adicionar Matéria ao Ciclo</button>
      </form>

      <div class="subject-list" id="subject-list">
        <!-- Itens inseridos dinamicamente -->
      </div>
    </div>
  </div>

  <script>
    // Configurações de paleta padrão para novas matérias
    const defaultColors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#ec4899', '#06b6d4'];

    // Estado da aplicação
    let subjects = JSON.parse(localStorage.getItem('study_cycle_data')) || [];
    let activeSubjectId = null;
    let timerInterval = null;
    let secondsElapsed = 0;

    // Inicialização do Gráfico Chart.js
    const ctx = document.getElementById('cycleChart').getContext('2d');
    let cycleChart = new Chart(ctx, {
      type: 'doughnut',
      data: {
        labels: [],
        datasets: [{
          data: [],
          backgroundColor: [],
          borderWidth: 2,
          borderColor: '#ffffff'
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: 'bottom'
          },
          tooltip: {
            callbacks: {
              label: function(context) {
                const label = context.label || '';
                const value = context.raw || 0;
                return ` ${label}: ${value} h`;
              }
            }
          }
        }
      }
    });

    // Salvar Dados no LocalStorage
    function saveData() {
      localStorage.setItem('study_cycle_data', JSON.stringify(subjects));
      render();
    }

    // Adicionar Nova Matéria
    document.getElementById('subject-form').addEventListener('submit', (e) => {
      e.preventDefault();
      const name = document.getElementById('subject-name').value.trim();
      const hours = parseFloat(document.getElementById('subject-hours').value);
      const color = document.getElementById('subject-color').value;

      if (!name || isNaN(hours)) return;

      const newSubject = {
        id: Date.now().toString(),
        name,
        targetHours: hours,
        studiedHours: 0,
        color,
        topics: []
      };

      subjects.push(newSubject);
      
      // Reseta form
      document.getElementById('subject-name').value = '';
      document.getElementById('subject-hours').value = '';
      document.getElementById('subject-color').value = defaultColors[subjects.length % defaultColors.length];

      saveData();
    });

    // Remover Matéria
    function deleteSubject(id) {
      subjects = subjects.filter(s => s.id !== id);
      if (activeSubjectId === id) resetTimer();
      saveData();
    }

    // Adicionar Assunto/Tópico
    function addTopic(subjectId, topicText) {
      if (!topicText.trim()) return;
      const subject = subjects.find(s => s.id === subjectId);
      if (subject) {
        subject.topics.push({ id: Date.now().toString(), text: topicText, done: false });
        saveData();
      }
    }

    // Alternar Checkbox do Assunto
    function toggleTopic(subjectId, topicId) {
      const subject = subjects.find(s => s.id === subjectId);
      if (subject) {
        const topic = subject.topics.find(t => t.id === topicId);
        if (topic) topic.done = !topic.done;
        saveData();
      }
    }

    // Remover Tópico
    function deleteTopic(subjectId, topicId) {
      const subject = subjects.find(s => s.id === subjectId);
      if (subject) {
        subject.topics = subject.topics.filter(t => t.id !== topicId);
        saveData();
      }
    }

    // Atualizar Gráfico e Barra de Progresso
    function updateProgressAndChart() {
      const labels = subjects.map(s => s.name);
      const data = subjects.map(s => s.targetHours);
      const colors = subjects.map(s => s.color);

      cycleChart.data.labels = labels;
      cycleChart.data.datasets[0].data = data;
      cycleChart.data.datasets[0].backgroundColor = colors;
      cycleChart.update();

      // Progresso Geral
      const totalTarget = subjects.reduce((acc, curr) => acc + curr.targetHours, 0);
      const totalStudied = subjects.reduce((acc, curr) => acc + curr.studiedHours, 0);
      const percentage = totalTarget > 0 ? Math.min(100, (totalStudied / totalTarget) * 100) : 0;

      document.getElementById('progress-bar').style.width = `${percentage}%`;
      document.getElementById('progress-text').innerText = `${totalStudied.toFixed(1)} / ${totalTarget.toFixed(1)} horas (${percentage.toFixed(1)}%)`;
    }

    // Lógica do Cronômetro
    function selectSubjectForStudy(id) {
      activeSubjectId = id;
      const subject = subjects.find(s => s.id === id);
      document.getElementById('timer-subject').innerText = `Estudando agora: ${subject.name}`;
      document.getElementById('btn-start').disabled = false;
      resetTimer();
    }

    function formatTime(sec) {
      const h = Math.floor(sec / 3600).toString().padStart(2, '0');
      const m = Math.floor((sec % 3600) / 60).toString().padStart(2, '0');
      const s = (sec % 60).toString().padStart(2, '0');
      return `${h}:${m}:${s}`;
    }

    document.getElementById('btn-start').addEventListener('click', () => {
      if (!timerInterval) {
        timerInterval = setInterval(() => {
          secondsElapsed++;
          document.getElementById('timer-display').innerText = formatTime(secondsElapsed);
        }, 1000);
        document.getElementById('btn-start').disabled = true;
        document.getElementById('btn-pause').disabled = false;
        document.getElementById('btn-save').disabled = false;
      }
    });

    document.getElementById('btn-pause').addEventListener('click', () => {
      clearInterval(timerInterval);
      timerInterval = null;
      document.getElementById('btn-start').disabled = false;
      document.getElementById('btn-pause').disabled = true;
    });

    document.getElementById('btn-save').addEventListener('click', () => {
      clearInterval(timerInterval);
      timerInterval = null;

      const hoursAdded = secondsElapsed / 3600;
      const subject = subjects.find(s => s.id === activeSubjectId);
      if (subject) {
        subject.studiedHours += hoursAdded;
        saveData();
      }

      resetTimer();
    });

    function resetTimer() {
      clearInterval(timerInterval);
      timerInterval = null;
      secondsElapsed = 0;
      document.getElementById('timer-display').innerText = '00:00:00';
      document.getElementById('btn-start').disabled = !activeSubjectId;
      document.getElementById('btn-pause').disabled = true;
      document.getElementById('btn-save').disabled = true;
    }

    // Renderizar Interface
    function render() {
      updateProgressAndChart();
      const listContainer = document.getElementById('subject-list');
      listContainer.innerHTML = '';

      if (subjects.length === 0) {
        listContainer.innerHTML = `<p style="color: var(--text-muted); font-size: 0.9rem; text-align: center;">Nenhuma matéria cadastrada no ciclo.</p>`;
        return;
      }

      subjects.forEach(subject => {
        const item = document.createElement('div');
        item.className = 'subject-item';
        item.style.borderLeftColor = subject.color;

        const topicHTML = subject.topics.map(t => `
          <div class="topic-item">
            <label style="display: flex; align-items: center; gap: 0.4rem; cursor: pointer;">
              <input type="checkbox" ${t.done ? 'checked' : ''} onchange="toggleTopic('${subject.id}', '${t.id}')">
              <span style="${t.done ? 'text-decoration: line-through; color: var(--text-muted);' : ''}">${t.text}</span>
            </label>
            <button class="btn-sm btn-danger" style="padding: 0 4px;" onclick="deleteTopic('${subject.id}', '${t.id}')">&times;</button>
          </div>
        `).join('');

        item.innerHTML = `
          <div class="subject-header">
            <div>
              <div class="subject-title">${subject.name}</div>
              <div class="subject-meta">${subject.studiedHours.toFixed(1)}h / ${subject.targetHours}h concluídas</div>
            </div>
            <div class="actions">
              <button class="btn-sm" onclick="selectSubjectForStudy('${subject.id}')">Estudar</button>
              <button class="btn-sm btn-danger" onclick="deleteSubject('${subject.id}')">Excluir</button>
            </div>
          </div>
          <div class="topic-list">
            ${topicHTML}
            <div class="topic-add-form">
              <input type="text" id="input-topic-${subject.id}" placeholder="Adicionar assunto/edital...">
              <button type="button" onclick="
                const val = document.getElementById('input-topic-${subject.id}').value;
                addTopic('${subject.id}', val);
              ">Add</button>
            </div>
          </div>
        `;

        listContainer.appendChild(item);
      });
    }

    // Render Inicial
    render();
  </script>
</body>
</html>
