<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Schichtkalender</title>
  <style>
    /* Globales Styling */
    body {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #121212;
      color: #fff;
      overflow-x: hidden;
    }
    /* Homescreen Styling */
    #homeScreen {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      text-align: center;
      background: linear-gradient(135deg, #1e1e1e, #000);
      animation: fadeIn 1s ease-in;
      padding: 20px;
    }
    #homeScreen h1 {
      font-size: 2.5em;  /* kleinerer Titel */
      letter-spacing: 2px;
      margin-bottom: 20px;
      text-transform: uppercase;
    }
    #startButton {
      padding: 15px 30px;
      font-size: 1.2em;
      background-color: #007BFF;
      color: #fff;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: transform 0.3s ease, background-color 0.3s ease;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    }
    #startButton:hover {
      background-color: #0056b3;
      transform: scale(1.05);
    }
    #homeScreen p {
      position: absolute;
      bottom: 20px;
      font-size: 1em;
      color: #888;
      line-height: 1.5;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to   { opacity: 1; }
    }
    @keyframes fadeOut {
      from { opacity: 1; }
      to   { opacity: 0; }
    }
    /* Kalender Screen */
    #calendarScreen {
      display: none;
      background-color: #121212;
      color: #ccc;
      padding: 20px;
      animation: fadeIn 0.5s ease-in;
    }
    .calendar-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .calendar-header {
      width: 100%;
      max-width: 800px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      background-color: #222;
      border-radius: 8px;
      margin-bottom: 10px;
    }
    .nav-button, .settings-toggle {
      background: none;
      border: none;
      color: #fff;
      cursor: pointer;
      font-size: 1.5em;
    }
    .calendar {
      width: 100%;
      max-width: 800px;
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 10px;
    }
    .calendar-day {
      background-color: #fff;
      color: #000;
      border-radius: 8px;
      padding: 8px;
      text-align: center;
      position: relative;
      transition: transform 0.3s, background-color 0.3s;
      min-height: 60px;
    }
    .calendar-day:hover {
      transform: scale(1.05);
    }
    .calendar-day .weekday {
      font-size: 0.7em;
      color: #555;
      margin-bottom: 4px;
    }
    .calendar-day .day-number {
      font-size: 1.5em;
      font-weight: bold;
    }
    .info-icon {
      position: absolute;
      bottom: 4px;
      right: 4px;
      color: yellow;
      font-size: 1.2em;
    }
    /* Einstellungen-Menü */
    .settings-menu {
      position: fixed;
      top: 20px;
      right: 20px;
      background-color: #222;
      border: 1px solid #444;
      border-radius: 8px;
      padding: 20px;
      width: 300px;
      display: none;
      z-index: 100;
      font-size: 1.1em;
    }
    .settings-menu.active {
      display: block;
    }
    .settings-menu label {
      display: block;
      margin-top: 10px;
      font-size: 0.9em;
    }
    .settings-menu input,
    .settings-menu button {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border: none;
      border-radius: 4px;
      font-size: 1em;
    }
    .settings-menu input {
      background-color: #333;
      color: #fff;
    }
    .settings-menu button {
      background-color: #007BFF;
      color: #fff;
      cursor: pointer;
    }
    .settings-menu button:hover {
      background-color: #0056b3;
    }
    /* Day Modal */
    .day-modal {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #222;
      border: 1px solid #444;
      border-radius: 8px;
      padding: 20px;
      width: 350px;
      z-index: 200;
      display: none;
    }
    .day-modal.active {
      display: block;
    }
    .day-modal label {
      font-size: 0.9em;
      margin-top: 10px;
      display: block;
    }
    .day-modal input,
    .day-modal textarea,
    .day-modal button {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border-radius: 4px;
      border: none;
      font-size: 1em;
    }
    .day-modal textarea {
      resize: vertical;
      height: 100px;
    }
    .day-modal button {
      background-color: #007BFF;
      color: #fff;
      cursor: pointer;
      margin-top: 10px;
    }
    .day-modal button:hover {
      background-color: #0056b3;
    }
    .day-modal .delete-button {
      background-color: #cc0000;
    }
    .day-modal .delete-button:hover {
      background-color: #a30000;
    }
  </style>
</head>
<body>

  <!-- Homescreen -->
  <div id="homeScreen">
    <h1>Schichtkalender</h1>
    <button id="startButton">Start</button>
    <p>founded by Omar Yacoubi Jakobi</p>
  </div>

  <!-- Kalender Screen -->
  <div id="calendarScreen">
    <div class="calendar-container">
      <div class="calendar-header">
        <button id="prevMonth" class="nav-button">←</button>
        <span id="monthYear">Januar 2025</span>
        <button id="nextMonth" class="nav-button">→</button>
        <button id="settingsToggle" class="settings-toggle">⋮</button>
      </div>
      <div id="calendar" class="calendar"></div>
    </div>
  </div>

  <!-- Einstellungen-Menü -->
  <div id="settingsMenu" class="settings-menu">
    <label for="startDate">Startdatum (ab wann Turnus beginnen soll):</label>
    <input type="date" id="startDate" value="2025-01-01">
    <label for="turnusPattern">Turnusplan (nur A und F, z.B. A,F):</label>
    <input type="text" id="turnusPattern" placeholder="A,F" value="A,F">
    <label for="colorA">Farbe für A (Arbeit):</label>
    <input type="color" id="colorA" value="#ff0000">
    <label for="colorF">Farbe für F (Frei):</label>
    <input type="color" id="colorF" value="#00ff00">
    <button id="applySettings">Akzeptieren</button>
  </div>

  <!-- Day Modal -->
  <div id="dayModal" class="day-modal">
    <label for="dayNote">Notiz (optional):</label>
    <textarea id="dayNote" placeholder="Hier Notiz eingeben..."></textarea>
    <label for="dayColor">Farbe für diesen Tag (optional):</label>
    <input type="color" id="dayColor">
    <button id="saveDay">Speichern</button>
    <button id="cancelDay">Abbrechen</button>
    <button id="deleteDay" class="delete-button">Notiz &amp; Farbe löschen</button>
  </div>

  <script>
    // Globale Variablen
    const weekdays = ['MO','DI','MI','DO','FR','SA','SO'];
    let currentMonth = 0; // Januar
    let currentYear = 2025;
    let turnusPattern = ['A','F'];
    let colorA = '#ff0000';
    let colorF = '#00ff00';
    let startDate = new Date('2025-01-01');
    let dayNotes = {};
    let customDayColors = {};

    // Daten aus localStorage laden
    function loadData() {
      const storedNotes = localStorage.getItem('dayNotes');
      const storedColors = localStorage.getItem('customDayColors');
      const storedPattern = localStorage.getItem('turnusPattern');
      const storedStart = localStorage.getItem('startDate');
      const storedColorA = localStorage.getItem('colorA');
      const storedColorF = localStorage.getItem('colorF');
      if (storedNotes) dayNotes = JSON.parse(storedNotes);
      if (storedColors) customDayColors = JSON.parse(storedColors);
      if (storedPattern) turnusPattern = JSON.parse(storedPattern);
      if (storedStart) startDate = new Date(storedStart);
      if (storedColorA) colorA = storedColorA;
      if (storedColorF) colorF = storedColorF;
    }
    loadData();

    // Daten speichern
    function saveData() {
      localStorage.setItem('dayNotes', JSON.stringify(dayNotes));
      localStorage.setItem('customDayColors', JSON.stringify(customDayColors));
      localStorage.setItem('turnusPattern', JSON.stringify(turnusPattern));
      localStorage.setItem('startDate', startDate.toISOString());
      localStorage.setItem('colorA', colorA);
      localStorage.setItem('colorF', colorF);
    }

    // Kalender aktualisieren
    function updateCalendar() {
      const calendar = document.getElementById('calendar');
      const monthYearDisplay = document.getElementById('monthYear');
      let monthDate = new Date(currentYear, currentMonth);
      monthYearDisplay.textContent = monthDate.toLocaleString('de', { month: 'long' }) + ' ' + currentYear;
      calendar.innerHTML = '';

      let firstDayIndex = new Date(currentYear, currentMonth, 1).getDay();
      firstDayIndex = (firstDayIndex + 6) % 7;
      if (currentYear === 2025 && currentMonth === 0) {
        firstDayIndex = 2; // Erzwinge: 1. Januar 2025 = Mittwoch
      }

      const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();
      for (let i = 0; i < firstDayIndex; i++) {
        const emptyDiv = document.createElement('div');
        calendar.appendChild(emptyDiv);
      }

      for (let day = 1; day <= daysInMonth; day++) {
        const dayDiv = document.createElement('div');
        dayDiv.className = 'calendar-day';
        dayDiv.dataset.day = day;

        const weekdayAbbr = weekdays[(firstDayIndex + day - 1) % 7];
        dayDiv.innerHTML = `
          <div class="weekday">${weekdayAbbr}</div>
          <div class="day-number">${day}</div>
        `;
        
        const dateKey = `${currentYear}-${(currentMonth+1).toString().padStart(2,'0')}-${day.toString().padStart(2,'0')}`;
        const thisDate = new Date(currentYear, currentMonth, day);
        let diffDays = Math.floor((thisDate - startDate) / (1000*60*60*24));
        
        if (diffDays >= 0) {
          if (customDayColors[dateKey]) {
            dayDiv.style.backgroundColor = customDayColors[dateKey];
            dayDiv.style.color = '#fff';
          } else {
            let patternIndex = diffDays % turnusPattern.length;
            let currentPattern = turnusPattern[patternIndex];
            if (currentPattern === 'A') {
              dayDiv.style.backgroundColor = colorA;
              dayDiv.style.color = '#fff';
            } else if (currentPattern === 'F') {
              dayDiv.style.backgroundColor = colorF;
              dayDiv.style.color = '#fff';
            }
          }
        } else {
          dayDiv.style.backgroundColor = '#fff';
          dayDiv.style.color = '#000';
        }
        
        if (dayNotes[dateKey]) {
          const infoIcon = document.createElement('div');
          infoIcon.className = 'info-icon';
          infoIcon.textContent = '•';
          dayDiv.appendChild(infoIcon);
        }
        
        dayDiv.addEventListener('click', () => openDayModal(dateKey, dayDiv));
        calendar.appendChild(dayDiv);
      }
      saveData();
    }
    
    function openDayModal(dateKey, dayDiv) {
      const dayModal = document.getElementById('dayModal');
      dayModal.classList.add('active');
      dayModal.dataset.dateKey = dateKey;
      document.getElementById('dayNote').value = dayNotes[dateKey] || '';
      if (customDayColors[dateKey]) {
        document.getElementById('dayColor').value = customDayColors[dateKey];
      } else {
        document.getElementById('dayColor').value = '#ffffff';
      }
    }
    
    function closeDayModal() {
      document.getElementById('dayModal').classList.remove('active');
    }
    
    document.getElementById('saveDay').addEventListener('click', () => {
      const dayModal = document.getElementById('dayModal');
      const dateKey = dayModal.dataset.dateKey;
      const note = document.getElementById('dayNote').value;
      const customColor = document.getElementById('dayColor').value;
      if (note.trim() !== '') {
        dayNotes[dateKey] = note;
      } else {
        delete dayNotes[dateKey];
      }
      if (customColor) {
        customDayColors[dateKey] = customColor;
      }
      closeDayModal();
      updateCalendar();
    });
    
    document.getElementById('cancelDay').addEventListener('click', () => {
      closeDayModal();
    });
    
    document.getElementById('deleteDay').addEventListener('click', () => {
      const dayModal = document.getElementById('dayModal');
      const dateKey = dayModal.dataset.dateKey;
      delete dayNotes[dateKey];
      delete customDayColors[dateKey];
      closeDayModal();
      updateCalendar();
    });
    
    document.getElementById('settingsToggle').addEventListener('click', () => {
      document.getElementById('settingsMenu').classList.toggle('active');
    });
    
    document.getElementById('applySettings').addEventListener('click', () => {
      const startDateInput = document.getElementById('startDate').value;
      const patternInput = document.getElementById('turnusPattern').value;
      colorA = document.getElementById('colorA').value;
      colorF = document.getElementById('colorF').value;
      if (startDateInput) {
        startDate = new Date(startDateInput);
      }
      if (patternInput) {
        turnusPattern = patternInput.split(',').map(e => e.trim());
      }
      document.getElementById('settingsMenu').classList.remove('active');
      updateCalendar();
    });
    
    document.getElementById('prevMonth').addEventListener('click', () => {
      currentMonth--;
      if (currentMonth < 0) {
        currentMonth = 11;
        currentYear--;
      }
      updateCalendar();
    });
    
    document.getElementById('nextMonth').addEventListener('click', () => {
      currentMonth++;
      if (currentMonth > 11) {
        currentMonth = 0;
        currentYear++;
      }
      updateCalendar();
    });
    
    // Homescreen: Klick auf Start wechselt zum Kalender-Screen mit Animation
    document.getElementById('startButton').addEventListener('click', () => {
      const homeScreen = document.getElementById('homeScreen');
      homeScreen.style.animation = 'fadeOut 0.5s forwards';
      setTimeout(() => {
        homeScreen.style.display = 'none';
        document.getElementById('calendarScreen').style.display = 'block';
        updateCalendar();
      }, 500);
    });
    
    updateCalendar();
  </script>

</body>
</html>