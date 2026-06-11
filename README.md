<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Grafik miesięczny</title>
  <style>
    :root {
      --bg: #eef3fb;
      --card: #ffffff;
      --card-soft: #f8fafc;
      --text: #101828;
      --muted: #667085;
      --line: #d6deea;
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --green-bg: #e9f9f0;
      --green-border: #98d9b5;
      --green-text: #166534;
      --yellow-bg: #fff7df;
      --yellow-border: #facc15;
      --red-bg: #fee2e2;
      --red-text: #991b1b;
      --shadow: 0 12px 32px rgba(15, 23, 42, 0.08);
      --radius: 18px;
    }

    * {
      box-sizing: border-box;
    }

    html {
      -webkit-text-size-adjust: 100%;
    }

    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.35;
      overflow-x: hidden;
    }

    button,
    input,
    select {
      font: inherit;
    }

    button {
      border: 0;
      cursor: pointer;
    }

    .app {
      width: min(1280px, calc(100% - 24px));
      margin: 0 auto;
      padding: 18px 0;
    }

    .hero {
      display: grid;
      grid-template-columns: 1.25fr 0.75fr;
      gap: 18px;
      align-items: stretch;
      margin-bottom: 18px;
    }

    header {
      background: linear-gradient(135deg, #1d4ed8, #60a5fa);
      color: white;
      padding: 24px;
      border-radius: 24px;
      box-shadow: var(--shadow);
      min-height: 150px;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    header h1 {
      margin: 0 0 8px;
      font-size: clamp(26px, 3vw, 40px);
      letter-spacing: -0.04em;
    }

    header p {
      margin: 0;
      max-width: 780px;
      color: rgba(255, 255, 255, 0.92);
      font-size: 16px;
    }

    .toolbar {
      background: var(--card);
      padding: 18px;
      border-radius: 24px;
      box-shadow: var(--shadow);
      display: grid;
      grid-template-columns: 1fr 0.7fr;
      gap: 12px;
      align-content: center;
    }

    .toolbar .wide {
      grid-column: 1 / -1;
    }

    label {
      display: block;
      margin-bottom: 6px;
      color: var(--muted);
      font-size: 12px;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 0.03em;
    }

    select,
    input[type="number"],
    .time-select {
      width: 100%;
      min-height: 42px;
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 9px 11px;
      background: white;
      color: var(--text);
      outline: none;
    }

    select:focus,
    input:focus,
    .time-select:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.12);
    }

    .btn-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
    }

    .btn {
      min-height: 42px;
      border-radius: 12px;
      padding: 10px 12px;
      background: var(--primary);
      color: white;
      font-weight: 800;
      transition: transform 0.15s ease, background 0.15s ease;
      white-space: nowrap;
    }

    .btn:hover {
      background: var(--primary-dark);
      transform: translateY(-1px);
    }

    .btn.secondary {
      background: #eef4ff;
      color: #1e40af;
    }

    .btn.secondary:hover {
      background: #dceafe;
    }

    .btn.danger {
      background: var(--red-bg);
      color: var(--red-text);
    }

    .btn.danger:hover {
      background: #fecaca;
    }

    .summary {
      display: grid;
      grid-template-columns: repeat(4, minmax(0, 1fr));
      gap: 14px;
      margin-bottom: 18px;
    }

    .summary-card {
      background: var(--card);
      border: 1px solid rgba(214, 222, 234, 0.85);
      border-radius: var(--radius);
      padding: 17px;
      box-shadow: var(--shadow);
      min-width: 0;
    }

    .summary-card .label-text {
      color: var(--muted);
      font-size: 12px;
      font-weight: 900;
      text-transform: uppercase;
      letter-spacing: 0.03em;
      margin-bottom: 8px;
    }

    .summary-card .value {
      font-size: clamp(24px, 2.6vw, 34px);
      font-weight: 950;
      letter-spacing: -0.04em;
      white-space: nowrap;
    }

    .summary-card small {
      display: block;
      margin-top: 6px;
      color: var(--muted);
      font-size: 12px;
    }

    .calendar-shell {
      background: var(--card);
      border: 1px solid rgba(214, 222, 234, 0.9);
      border-radius: 24px;
      box-shadow: var(--shadow);
      padding: 10px;
      overflow: visible;
      width: 100%;
    }

    .weekdays,
    .calendar {
      display: grid;
      grid-template-columns: repeat(7, minmax(0, 1fr));
      gap: 8px;
      width: 100%;
    }

    .weekdays {
      display: none;
      margin-bottom: 10px;
      padding: 0 2px;
    }

    .weekday {
      text-align: center;
      color: var(--muted);
      font-size: 12px;
      font-weight: 950;
      text-transform: uppercase;
      letter-spacing: 0.04em;
      padding: 6px 0;
    }

    .day {
      min-width: 0;
      min-height: 206px;
      background: #fff;
      border: 1px solid var(--line);
      border-radius: 15px;
      padding: 8px;
      display: flex;
      flex-direction: column;
      gap: 7px;
      overflow: hidden;
    }

    .day.empty {
      background: #f6f8fb;
      border-style: dashed;
      box-shadow: none;
      min-height: 206px;
    }

    .day.free {
      background: var(--green-bg);
      border-color: var(--green-border);
    }

    .day.weekend:not(.free) {
      background: #fbfcff;
    }

    .day-head {
      display: flex;
      align-items: start;
      justify-content: space-between;
      gap: 8px;
      min-width: 0;
    }

    .date-block {
      min-width: 0;
    }

    .date {
      font-size: 18px;
      line-height: 1;
      font-weight: 950;
    }

    .day-name {
      margin-top: 3px;
      color: var(--muted);
      font-size: 12px;
      font-weight: 800;
    }

    .badge {
      flex: 0 0 auto;
      max-width: 70px;
      overflow: hidden;
      text-overflow: ellipsis;
      border-radius: 999px;
      padding: 3px 6px;
      font-size: 10px;
      font-weight: 900;
      background: #eef4ff;
      color: #1e40af;
      white-space: nowrap;
    }

    .badge.free-badge {
      background: #dcfce7;
      color: var(--green-text);
    }

    .inputs {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 5px;
      min-width: 0;
    }

    .inputs label {
      margin-bottom: 4px;
      font-size: 11px;
    }

    .inputs .time-select {
      min-width: 0;
      padding: 0 4px;
      min-height: 34px;
      font-size: 12px;
      appearance: auto;
    }

    .actions {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 7px;
    }

    .actions .btn {
      min-height: 33px;
      padding: 7px 6px;
      font-size: 11.5px;
      white-space: normal;
    }

    .shifts {
      display: flex;
      flex-direction: column;
      gap: 6px;
      margin-top: auto;
      min-width: 0;
    }

    .empty-info {
      color: var(--muted);
      font-size: 12px;
      text-align: center;
      padding: 9px 6px;
      border: 1px dashed #d6deea;
      border-radius: 12px;
      background: #f8fafc;
    }

    .day-break-info {
      color: #854d0e;
      background: #fff7df;
      border: 1px solid #facc15;
      border-radius: 12px;
      padding: 7px 6px;
      font-size: 11px;
      font-weight: 850;
      line-height: 1.25;
      text-align: center;
      white-space: pre-line;
    }






    .shift {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 26px;
      gap: 7px;
      align-items: center;
      background: var(--card-soft);
      border: 1px solid #e2e8f0;
      border-radius: 12px;
      padding: 7px;
      min-width: 0;
    }

    .shift.late {
      background: var(--yellow-bg);
      border-color: var(--yellow-border);
    }

    .shift strong {
      display: block;
      margin-bottom: 2px;
      font-size: 13px;
      white-space: nowrap;
    }

    .shift small {
      display: block;
      color: var(--muted);
      font-size: 11px;
      overflow-wrap: anywhere;
      white-space: pre-line;
      line-height: 1.2;
    }

    .remove {
      width: 26px;
      height: 26px;
      border-radius: 9px;
      background: var(--red-bg);
      color: var(--red-text);
      font-weight: 950;
      line-height: 1;
    }

    .remove:hover {
      background: #fecaca;
    }

    .note {
      margin-top: 18px;
      color: var(--muted);
      font-size: 14px;
      background: white;
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 15px 17px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.04);
    }

    .note strong {
      color: var(--text);
    }

    .signature {
      display: block;
      margin-top: 10px;
      text-align: right;
      color: var(--muted);
      font-weight: 900;
      letter-spacing: 0.12em;
    }

    @media (max-width: 1200px) {
      .app {
        width: min(1000px, calc(100% - 20px));
      }

      .hero {
        grid-template-columns: 1fr;
      }

      .toolbar {
        grid-template-columns: repeat(4, minmax(0, 1fr));
      }

      .toolbar .wide {
        grid-column: span 2;
      }

      .summary {
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }

      .day {
        min-height: 198px;
      }
    }

    @media (max-width: 980px) {
      .weekdays,
      .calendar {
        grid-template-columns: repeat(4, minmax(0, 1fr));
      }

      .day.empty {
        display: none;
      }
    }

    @media (max-width: 760px) {
      .app {
        width: calc(100% - 20px);
        padding: 12px 0;
      }

      .hero {
        gap: 12px;
      }

      header {
        min-height: unset;
        padding: 20px;
        border-radius: 20px;
      }

      header p {
        font-size: 14px;
      }

      .toolbar {
        grid-template-columns: 1fr 1fr;
        border-radius: 20px;
        padding: 14px;
      }

      .toolbar .wide {
        grid-column: 1 / -1;
      }

      .btn-row {
        grid-template-columns: 1fr;
      }

      .summary {
        grid-template-columns: 1fr 1fr;
        gap: 10px;
      }

      .summary-card {
        padding: 13px;
        border-radius: 16px;
      }

      .summary-card .value {
        font-size: 23px;
      }

      .summary-card small {
        display: none;
      }

      .calendar-shell {
        padding: 0;
        border: 0;
        background: transparent;
        box-shadow: none;
        overflow: visible;
      }

      .weekdays {
        display: none;
      }

      .calendar {
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 9px;
        min-width: 0;
      }

      .day.empty {
        display: none;
      }

      .day {
        min-height: 200px;
        border-radius: 16px;
        padding: 8px;
        box-shadow: 0 6px 18px rgba(15, 23, 42, 0.06);
      }

      .day-head {
        align-items: center;
      }

      .date-block {
        display: flex;
        align-items: baseline;
        gap: 6px;
      }

      .date {
        font-size: 20px;
      }

      .day-name {
        font-size: 12px;
      }

      .badge {
        max-width: none;
      }

      .inputs {
        grid-template-columns: 1fr 1fr;
      }

      .actions {
        grid-template-columns: 1fr 1fr;
      }

      .actions .btn {
        font-size: 13px;
        white-space: nowrap;
      }

      .shift {
        grid-template-columns: minmax(0, 1fr) 30px;
      }

      .remove {
        width: 30px;
        height: 30px;
      }
    }

    @media (max-width: 430px) {
      .app {
        width: calc(100% - 12px);
      }

      .summary {
        grid-template-columns: 1fr 1fr;
      }

      .toolbar {
        grid-template-columns: 1fr;
      }

      .calendar {
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 7px;
      }

      .day {
        padding: 7px;
        gap: 6px;
        min-height: 194px;
      }

      .inputs {
        grid-template-columns: 1fr 1fr;
        gap: 4px;
      }

      .actions {
        grid-template-columns: 1fr 1fr;
        gap: 4px;
      }





      .actions .btn {
        min-height: 30px;
        padding: 6px 4px;
        font-size: 11px;
      }

      .inputs label {
        font-size: 10px;
      }

      .inputs .time-select {
        font-size: 11px;
        min-height: 32px;
        padding: 0 2px;
      }

      .badge {
        max-width: 55px;
        font-size: 9px;
        padding: 3px 5px;
      }

      .empty-info,
      .day-break-info {
        font-size: 10.5px;
        padding: 6px 4px;
      }

      .shift strong {
        font-size: 12px;
      }

      .shift small {
        font-size: 10px;
      }

      .summary-card small {
        display: none;
      }
    }

    @media print {
      @page {
        size: A4 landscape;
        margin: 4mm;
      }

      * {
        box-shadow: none !important;
        text-shadow: none !important;
        -webkit-print-color-adjust: exact !important;
        print-color-adjust: exact !important;
      }

      html,
      body {
        width: auto !important;
        height: auto !important;
        margin: 0 !important;
        padding: 0 !important;
        background: #fff !important;
        color: #000 !important;
        overflow: visible !important;
      }

      body {
        transform: none !important;
        font-family: Arial, Helvetica, sans-serif !important;
      }

      .app {
        width: 100% !important;
        max-width: none !important;
        margin: 0 !important;
        padding: 0 !important;
      }

      .hero {
        display: block !important;
        margin: 0 0 2mm 0 !important;
      }

      header {
        display: block !important;
        min-height: 0 !important;
        padding: 2mm 3mm !important;
        border: 1px solid #000 !important;
        border-radius: 0 !important;
        background: #fff !important;
        color: #000 !important;
      }

      header h1 {
        margin: 0 !important;
        padding: 0 !important;
        font-size: 12pt !important;
        line-height: 1.1 !important;
        color: #000 !important;
        letter-spacing: 0 !important;
      }

      header p {
        display: none !important;
      }

      .toolbar,
      .note,
      .actions,
      .inputs,
      .remove {
        display: none !important;
      }

      .summary {
        display: grid !important;
        grid-template-columns: repeat(4, 1fr) !important;
        gap: 1.5mm !important;
        margin: 0 0 2mm 0 !important;
      }

      .summary-card {
        border: 1px solid #000 !important;
        border-radius: 0 !important;
        background: #fff !important;
        padding: 1.2mm 1.8mm !important;
        min-height: 0 !important;
      }

      .summary-card .label-text {
        margin: 0 0 0.5mm 0 !important;
        color: #000 !important;
        font-size: 6pt !important;
        line-height: 1 !important;
        letter-spacing: 0 !important;
      }

      .summary-card .value {
        color: #000 !important;
        font-size: 12pt !important;
        line-height: 1 !important;
        letter-spacing: 0 !important;
      }

      .summary-card small {
        display: none !important;
      }

      .calendar-shell {
        width: 100% !important;
        border: 0 !important;
        border-radius: 0 !important;
        background: #fff !important;
        padding: 0 !important;
        margin: 0 !important;
        overflow: visible !important;
      }

      .weekdays,
      .calendar {
        display: grid !important;
        grid-template-columns: repeat(7, minmax(0, 1fr)) !important;
        width: 100% !important;
        min-width: 0 !important;
        gap: 1mm !important;
      }

      .weekdays {
        margin: 0 0 1mm 0 !important;
        padding: 0 !important;
      }

      .weekday {
        display: block !important;
        border: 1px solid #000 !important;
        background: #fff !important;
        color: #000 !important;
        padding: 0.6mm 0 !important;
        font-size: 6pt !important;
        line-height: 1 !important;
        text-align: center !important;
        font-weight: 800 !important;
      }

      .day {
        height: 20.5mm !important;
        min-height: 20.5mm !important;
        max-height: 20.5mm !important;
        min-width: 0 !important;
        overflow: hidden !important;
        border: 1px solid #000 !important;
        border-radius: 0 !important;
        background: #fff !important;
        color: #000 !important;
        padding: 1mm !important;
        gap: 0.5mm !important;
        break-inside: avoid !important;
        page-break-inside: avoid !important;
      }

      .day.empty {
        display: block !important;
        height: 20.5mm !important;
        min-height: 20.5mm !important;
        max-height: 20.5mm !important;
        border: 1px solid #bbb !important;
        background: #fff !important;
      }

      .day.free,
      .day.weekend,
      .day.weekend:not(.free) {
        background: #fff !important;
        border-color: #000 !important;
      }

      .day-head {
        display: flex !important;
        align-items: center !important;
        justify-content: space-between !important;
        gap: 1mm !important;
        margin: 0 !important;
        padding: 0 !important;
      }

      .date-block {
        display: flex !important;
        align-items: baseline !important;
        gap: 1mm !important;
        min-width: 0 !important;
      }

      .date {
        color: #000 !important;
        font-size: 8pt !important;
        line-height: 1 !important;
        font-weight: 900 !important;
      }

      .day-name {
        color: #000 !important;
        font-size: 5.5pt !important;
        line-height: 1 !important;
        margin: 0 !important;
        font-weight: 700 !important;
      }

      .badge,
      .badge.free-badge {
        flex: 0 0 auto !important;
        max-width: none !important;
        border: 1px solid #000 !important;
        border-radius: 0 !important;
        background: #fff !important;
        color: #000 !important;
        padding: 0.3mm 0.8mm !important;
        font-size: 5pt !important;
        line-height: 1 !important;
        font-weight: 800 !important;
      }

      .shifts {
        display: flex !important;
        flex-direction: column !important;
        gap: 0.4mm !important;
        margin: 0 !important;
        min-width: 0 !important;
      }

      .empty-info,
      .day-break-info {
        border: 1px solid #000 !important;
        border-radius: 0 !important;
        background: #fff !important;
        color: #000 !important;
        padding: 0.4mm !important;
        font-size: 4.6pt !important;
        line-height: 1.02 !important;
        font-weight: 700 !important;
        text-align: center !important;
        overflow: hidden !important;
      }

      .shift,
      .shift.late {
        display: block !important;
        border: 0 !important;
        border-top: 1px solid #999 !important;
        border-radius: 0 !important;
        background: #fff !important;
        color: #000 !important;
        padding: 0.4mm 0 0 0 !important;
        margin: 0 !important;
        min-width: 0 !important;
        overflow: hidden !important;
      }

      .shift strong {
        display: block !important;
        color: #000 !important;
        font-size: 5.8pt !important;
        line-height: 1.05 !important;
        margin: 0 !important;
        white-space: nowrap !important;
      }

      .shift small {
        display: block !important;
        color: #000 !important;
        font-size: 4.5pt !important;
        line-height: 1.05 !important;
        margin: 0 !important;
        overflow: hidden !important;
        white-space: nowrap !important;
        text-overflow: clip !important;
      }
    }
  </style>
</head>
<body>
  <main class="app">
    <section class="hero">
      <header>
        <h1>Grafik miesięczny</h1>
        <p>Ustaw miesiąc, dodawaj godziny pracy albo oznaczaj wolne. Podsumowanie godzin i udział zmian 16:00–20:00 liczy się automatycznie.</p>
      </header>

      <section class="toolbar" aria-label="Ustawienia grafiku">
        <div>
          <label for="monthSelect">Miesiąc</label>
          <select id="monthSelect"></select>
        </div>

        <div>
          <label for="yearInput">Rok</label>
          <input id="yearInput" type="number" min="2000" max="2100" />
        </div>

        <div class="wide btn-row">
          <button id="todayBtn" type="button" class="btn secondary">Bieżący</button>
          <button id="saveMonthBtn" type="button" class="btn">Zapisz miesiąc</button>
        </div>

        <div class="wide btn-row">
          <button id="printBtn" type="button" class="btn secondary">Drukuj / PDF</button>
          <button id="clearMonthBtn" type="button" class="btn danger">Wyczyść miesiąc</button>
        </div>
      </section>
    </section>

    <section class="summary" aria-label="Podsumowanie miesiąca">
      <div class="summary-card">
        <div class="label-text">Suma godzin</div>
        <div class="value" id="totalHours">0 h</div>
        <small>Wszystkie zmiany w miesiącu.</small>
      </div>

      <div class="summary-card">
        <div class="label-text">Godziny 16–20</div>
        <div class="value" id="lateHours">0 h</div>
        <small>Tylko części zmian w 16:00–20:00.</small>
      </div>

      <div class="summary-card">
        <div class="label-text">% zmian 16–20</div>
        <div class="value" id="latePercent">0%</div>
        <small>Godziny 16–20 / suma godzin.</small>
      </div>

      <div class="summary-card">
        <div class="label-text">Dni wolne</div>
        <div class="value" id="freeDays">0</div>
        <small>Dni oznaczone jako wolne.</small>
      </div>
    </section>

    <section class="calendar-shell">
      <section class="weekdays" aria-hidden="true">
        <div class="weekday">Pon</div>
        <div class="weekday">Wt</div>
        <div class="weekday">Śr</div>
        <div class="weekday">Czw</div>
        <div class="weekday">Pt</div>
        <div class="weekday">Sob</div>
        <div class="weekday">Nd</div>
      </section>

      <section id="calendar" class="calendar"></section>
    </section>

    <section class="note">
      <strong>Jak działa strona?</strong>
      Wybierz miesiąc i rok, ustaw godziny zmiany z roletek co 30 minut, a potem kliknij <strong>„Zmiana”</strong>. Dzień możesz też oznaczyć jako <strong>„Wolne”</strong>. Na kafelku zobaczysz czas pracy, przerwę i netto, a na podsumowaniu miesiąca u góry strona policzy łączną liczbę godzin oraz procentowy udział godzin 16:00–20:00. Grafik zapisuje się automatycznie w tej przeglądarce, możesz go też zapisać do pliku albo wydrukować jako PDF.
      <span class="signature">MM - 2026</span>
    </section>
  </main>

  <script>
    const monthNames = [
      "Styczeń", "Luty", "Marzec", "Kwiecień", "Maj", "Czerwiec",
      "Lipiec", "Sierpień", "Wrzesień", "Październik", "Listopad", "Grudzień"
    ];

    const dayNames = ["Nd", "Pon", "Wt", "Śr", "Czw", "Pt", "Sob"];

    const calendar = document.getElementById("calendar");
    const monthSelect = document.getElementById("monthSelect");
    const yearInput = document.getElementById("yearInput");
    const totalHoursEl = document.getElementById("totalHours");
    const lateHoursEl = document.getElementById("lateHours");
    const latePercentEl = document.getElementById("latePercent");
    const freeDaysEl = document.getElementById("freeDays");

    let schedule = loadSchedule();
    let lastStartTime = localStorage.getItem("grafik-last-start") || "08:00";
    let lastEndTime = localStorage.getItem("grafik-last-end") || "16:00";

    function storageKey() {
      return "grafik-miesieczny-v2";
    }

    function loadSchedule() {
      try {
        return JSON.parse(localStorage.getItem(storageKey())) || {};
      } catch {
        return {};
      }
    }

    function saveSchedule() {
      localStorage.setItem(storageKey(), JSON.stringify(schedule));
    }

    function monthKey(year, month) {
      return `${year}-${String(month + 1).padStart(2, "0")}`;
    }

    function dayKey(year, month, day) {
      return `${monthKey(year, month)}-${String(day).padStart(2, "0")}`;
    }

    function ensureDay(key) {
      if (!schedule[key]) {
        schedule[key] = { free: false, shifts: [] };
      }
      return schedule[key];
    }

    function timeToMinutes(time) {
      const [h, m] = time.split(":").map(Number);
      return h * 60 + m;
    }

    function minutesToHours(minutes) {
      return minutes / 60;
    }

    function formatHours(hours) {
      const rounded = Math.round(hours * 100) / 100;
      return Number.isInteger(rounded)
        ? `${rounded} h`
        : `${rounded.toFixed(2).replace(".", ",")} h`;
    }

    function formatBreak(minutes) {
      if (minutes < 60) {
        return `${minutes} min`;
      }

      const hours = Math.floor(minutes / 60);
      const rest = minutes % 60;
      return rest === 0 ? `${hours} h` : `${hours} h ${rest} min`;
    }

    function breakMinutesForShift(start, end) {
      const duration = shiftDurationMinutes(start, end);
      const fullHours = Math.floor(duration / 60);

      // Zasady:
      // - 5 minut za każdą pełną godzinę zmiany,
      // - od 6 godzin dodatkowe 15 minut,
      // - od 9 godzin kolejne dodatkowe 15 minut.
      let minutes = fullHours * 5;

      if (duration >= 6 * 60) {
        minutes += 15;
      }

      if (duration >= 9 * 60) {
        minutes += 15;
      }

      return minutes;
    }

    function netMinutesForShift(start, end) {
      return Math.max(0, shiftDurationMinutes(start, end) - breakMinutesForShift(start, end));
    }

    const timeValues = Array.from({ length: 48 }, (_, index) => {
      const hour = Math.floor(index / 2);
      const minute = index % 2 === 0 ? "00" : "30";
      return `${String(hour).padStart(2, "0")}:${minute}`;
    });

    const timeOptionsCache = new Map();

    function buildTimeOptions(selectedValue) {
      if (timeOptionsCache.has(selectedValue)) {
        return timeOptionsCache.get(selectedValue);
      }

      const options = timeValues
        .map((value) => `<option value="${value}"${value === selectedValue ? " selected" : ""}>${value}</option>`)
        .join("");

      timeOptionsCache.set(selectedValue, options);
      return options;
    }

    function shiftDurationMinutes(start, end) {
      let startMin = timeToMinutes(start);
      let endMin = timeToMinutes(end);

      if (endMin <= startMin) {
        endMin += 24 * 60;
      }

      return endMin - startMin;
    }

    function overlapMinutes(start, end, rangeStart = 16 * 60, rangeEnd = 20 * 60) {
      let startMin = timeToMinutes(start);
      let endMin = timeToMinutes(end);

      if (endMin <= startMin) {
        endMin += 24 * 60;
      }

      let total = 0;

      for (const offset of [0, 24 * 60]) {
        const overlapStart = Math.max(startMin, rangeStart + offset);
        const overlapEnd = Math.min(endMin, rangeEnd + offset);
        total += Math.max(0, overlapEnd - overlapStart);
      }

      return total;
    }

    function getMonthEntries(year, month) {
      const prefix = monthKey(year, month);
      return Object.entries(schedule)
        .filter(([key]) => key.startsWith(prefix))
        .sort(([a], [b]) => a.localeCompare(b));
    }

    function calculateMonthStats(year, month) {
      let totalMinutes = 0;
      let lateMinutes = 0;
      let freeDays = 0;

      getMonthEntries(year, month).forEach(([, data]) => {
        if (data.free) {
          freeDays++;
        }

        data.shifts.forEach((shift) => {
          totalMinutes += shiftDurationMinutes(shift.start, shift.end);
          lateMinutes += overlapMinutes(shift.start, shift.end);
        });
      });

      return {
        totalMinutes,
        lateMinutes,
        freeDays,
        totalHours: minutesToHours(totalMinutes),
        lateHours: minutesToHours(lateMinutes),
        latePercent: totalMinutes > 0 ? (lateMinutes / totalMinutes) * 100 : 0
      };
    }

    function populateMonths() {
      monthNames.forEach((name, index) => {
        const option = document.createElement("option");
        option.value = index;
        option.textContent = name;
        monthSelect.appendChild(option);
      });
    }

    function setCurrentMonth() {
      const now = new Date();
      monthSelect.value = now.getMonth();
      yearInput.value = now.getFullYear();
      renderCalendar();
    }

    function renderCalendar() {
      const year = Number(yearInput.value);
      const month = Number(monthSelect.value);

      calendar.innerHTML = "";

      const firstDay = new Date(year, month, 1);
      const daysInMonth = new Date(year, month + 1, 0).getDate();
      const startOffset = (firstDay.getDay() + 6) % 7;

      for (let i = 0; i < startOffset; i++) {
        const empty = document.createElement("article");
        empty.className = "day empty";
        empty.setAttribute("aria-hidden", "true");
        calendar.appendChild(empty);
      }

      for (let day = 1; day <= daysInMonth; day++) {
        calendar.appendChild(createDayCard(year, month, day));
      }

      updateSummary();
    }

    function createDayCard(year, month, day) {
      const key = dayKey(year, month, day);
      const data = ensureDay(key);
      const date = new Date(year, month, day);
      const isWeekend = date.getDay() === 0 || date.getDay() === 6;

      const card = document.createElement("article");
      card.className = `day${data.free ? " free" : ""}${isWeekend ? " weekend" : ""}`;
      card.dataset.key = key;

      const head = document.createElement("div");
      head.className = "day-head";
      head.innerHTML = `
        <div class="date-block">
          <div class="date">${day}</div>
          <div class="day-name">${dayNames[date.getDay()]}</div>
        </div>
        <span class="badge ${data.free ? "free-badge" : ""}">${data.free ? "Wolne" : "Praca"}</span>
      `;

      const inputs = document.createElement("div");
      inputs.className = "inputs";
      inputs.innerHTML = `
        <div>
          <label>Od</label>
          <select class="start time-select" aria-label="Godzina rozpoczęcia">
            ${buildTimeOptions(lastStartTime)}
          </select>
        </div>
        <div>
          <label>Do</label>
          <select class="end time-select" aria-label="Godzina zakończenia">
            ${buildTimeOptions(lastEndTime)}
          </select>
        </div>
      `;

      const templates = document.createElement("div");
      templates.className = "quick-templates";
      templates.innerHTML = `
        <button type="button" class="template-btn" data-action="template" data-key="${key}" data-start="09:00" data-end="15:00">9–15</button>
        <button type="button" class="template-btn" data-action="template" data-key="${key}" data-start="09:00" data-end="17:00">9–17</button>
        <button type="button" class="template-btn" data-action="template" data-key="${key}" data-start="14:00" data-end="20:00">14–20</button>
      `;

      const actions = document.createElement("div");
      actions.className = "actions";

      const addBtn = document.createElement("button");
      addBtn.type = "button";
      addBtn.className = "btn";
      addBtn.dataset.action = "add-shift";
      addBtn.dataset.key = key;
      addBtn.textContent = "Zmiana";

      const freeBtn = document.createElement("button");
      freeBtn.type = "button";
      freeBtn.className = data.free ? "btn danger" : "btn secondary";
      freeBtn.dataset.action = "toggle-free";
      freeBtn.dataset.key = key;
      freeBtn.textContent = data.free ? "Usuń wolne" : "Wolne";

      actions.append(addBtn, freeBtn);

      const copyRow = document.createElement("div");
      copyRow.className = "copy-row";
      copyRow.innerHTML = `
        <button type="button" class="btn secondary" data-action="copy-next" data-key="${key}">Kopiuj +1</button>
        <button type="button" class="btn secondary" data-action="copy-week" data-key="${key}">Kopiuj tydz.</button>
      `;

      const shifts = document.createElement("div");
      shifts.className = "shifts";

      if (data.shifts.length === 0 && !data.free) {
        const empty = document.createElement("div");
        empty.className = "empty-info";
        empty.textContent = "Brak godzin";
        shifts.appendChild(empty);
      }

      if (data.free) {
        const free = document.createElement("div");
        free.className = "empty-info";
        free.textContent = "Dzień wolny";
        shifts.appendChild(free);
      }

      if (data.shifts.length > 0) {
        const dayMinutes = data.shifts.reduce((sum, shift) => sum + shiftDurationMinutes(shift.start, shift.end), 0);
        const dayBreakMinutes = data.shifts.reduce((sum, shift) => sum + breakMinutesForShift(shift.start, shift.end), 0);
        const dayNetMinutes = Math.max(0, dayMinutes - dayBreakMinutes);

        const daySummary = document.createElement("div");
        daySummary.className = "day-break-info";
        daySummary.textContent = `Praca: ${formatHours(minutesToHours(dayMinutes))}\nPrzerwa: ${formatBreak(dayBreakMinutes)}\nNetto: ${formatHours(minutesToHours(dayNetMinutes))}`;
        shifts.appendChild(daySummary);
      }

      data.shifts.forEach((shift, index) => {
        const duration = minutesToHours(shiftDurationMinutes(shift.start, shift.end));
        const late = minutesToHours(overlapMinutes(shift.start, shift.end));

        const row = document.createElement("div");
        row.className = `shift${late > 0 ? " late" : ""}`;
        row.innerHTML = `
          <div>
            <strong>${shift.start}–${shift.end}</strong>
            <small>Razem: ${formatHours(duration)}\n16–20: ${formatHours(late)}</small>
          </div>
        `;

        const remove = document.createElement("button");
        remove.type = "button";
        remove.className = "remove";
        remove.title = "Usuń zmianę";
        remove.textContent = "×";
        remove.dataset.action = "remove-shift";
        remove.dataset.key = key;
        remove.dataset.index = index;

        row.appendChild(remove);
        shifts.appendChild(row);
      });

      card.append(head, inputs, actions, shifts);
      return card;
    }

    function updateSummary() {
      const year = Number(yearInput.value);
      const month = Number(monthSelect.value);
      const stats = calculateMonthStats(year, month);

      totalHoursEl.textContent = formatHours(stats.totalHours);
      lateHoursEl.textContent = formatHours(stats.lateHours);
      latePercentEl.textContent = `${Math.round(stats.latePercent * 100) / 100}%`.replace(".", ",");
      freeDaysEl.textContent = stats.freeDays;
    }

    function saveSelectedMonthToFile() {
      const year = Number(yearInput.value);
      const month = Number(monthSelect.value);
      const entries = getMonthEntries(year, month);
      const stats = calculateMonthStats(year, month);

      const exportData = {
        miesiac: monthNames[month],
        miesiacNumer: month + 1,
        rok: year,
        podsumowanie: {
          sumaGodzin: Math.round(stats.totalHours * 100) / 100,
          godziny16do20: Math.round(stats.lateHours * 100) / 100,
          procent16do20: Math.round(stats.latePercent * 100) / 100,
          dniWolne: stats.freeDays
        },
        dni: entries.map(([date, data]) => ({
          data: date,
          wolne: data.free,
          zmiany: data.shifts.map((shift) => ({
            od: shift.start,
            do: shift.end,
            godziny: Math.round(minutesToHours(shiftDurationMinutes(shift.start, shift.end)) * 100) / 100,
            godziny16do20: Math.round(minutesToHours(overlapMinutes(shift.start, shift.end)) * 100) / 100,
            przerwaMinuty: breakMinutesForShift(shift.start, shift.end),
            godzinyNetto: Math.round(minutesToHours(netMinutesForShift(shift.start, shift.end)) * 100) / 100
          }))
        }))
      };

      const blob = new Blob([JSON.stringify(exportData, null, 2)], {
        type: "application/json;charset=utf-8"
      });

      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = `grafik-${year}-${String(month + 1).padStart(2, "0")}.json`;
      document.body.appendChild(link);
      link.click();
      link.remove();
      URL.revokeObjectURL(url);
    }

    calendar.addEventListener("click", (event) => {
      const button = event.target.closest("button[data-action]");
      if (!button) return;

      const key = button.dataset.key;
      const action = button.dataset.action;
      const card = button.closest(".day");
      const dayData = ensureDay(key);

      if (action === "template") {
        dayData.free = false;
        dayData.shifts.push({
          start: button.dataset.start,
          end: button.dataset.end
        });
      }

      if (action === "add-shift") {
        const start = card.querySelector(".start").value;
        const end = card.querySelector(".end").value;

        if (!start || !end) {
          alert("Wybierz godzinę rozpoczęcia i zakończenia.");
          return;
        }

        dayData.free = false;
        dayData.shifts.push({ start, end });
        lastStartTime = start;
        lastEndTime = end;
        localStorage.setItem("grafik-last-start", start);
        localStorage.setItem("grafik-last-end", end);
      }

      if (action === "toggle-free") {
        dayData.free = !dayData.free;

        if (dayData.free) {
          dayData.shifts = [];
        }
      }

      if (action === "remove-shift") {
        const index = Number(button.dataset.index);
        dayData.shifts.splice(index, 1);
      }

      if (action === "copy-next") {
        copyDayShifts(key, 1);
      }

      if (action === "copy-week") {
        copyDayShifts(key, 7);
      }

      saveSchedule();
      renderCalendar();
    });

    document.getElementById("todayBtn").addEventListener("click", setCurrentMonth);
    document.getElementById("saveMonthBtn").addEventListener("click", saveSelectedMonthToFile);
    document.getElementById("printBtn").addEventListener("click", () => window.print());

    document.getElementById("clearMonthBtn").addEventListener("click", () => {
      const year = Number(yearInput.value);
      const month = Number(monthSelect.value);
      const prefix = monthKey(year, month);

      if (!confirm(`Na pewno wyczyścić cały grafik: ${monthNames[month]} ${year}?`)) {
        return;
      }

      Object.keys(schedule).forEach((key) => {
        if (key.startsWith(prefix)) {
          delete schedule[key];
        }
      });

      saveSchedule();
      renderCalendar();
    });

    monthSelect.addEventListener("change", renderCalendar);
    yearInput.addEventListener("change", renderCalendar);

    populateMonths();
    setCurrentMonth();
  </script>
</body>
</html>
