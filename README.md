<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Reminder Clock</title>
  <style>
    body {
      background: #1e1e2f;
      color: #fff;
      font-family: 'Segoe UI', sans-serif;
      padding: 30px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .clock {
      font-size: 2rem;
      margin-bottom: 30px;
    }

    .reminder-form {
      background: #2c2c3e;
      padding: 20px;
      border-radius: 10px;
      width: 100%;
      max-width: 500px;
      margin-bottom: 30px;
    }

    input, textarea, button {
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 1rem;
      border: none;
      border-radius: 5px;
    }

    button {
      background: #00ffaa;
      color: #000;
      cursor: pointer;
    }

    .reminders {
      width: 100%;
      max-width: 500px;
    }

    .reminder {
      background: #333;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 10px;
    }

    .reminder-time {
      font-weight: bold;
    }
  </style>
</head>
<body>

  <div class="clock" id="clock">Loading time...</div>

  <div class="reminder-form">
    <h2>Set a Reminder</h2>
    <input type="datetime-local" id="reminderTime">
    <textarea id="reminderMsg" placeholder="Reminder message..."></textarea>
    <button onclick="addReminder()">Add Reminder</button>
  </div>

  <div class="reminders" id="reminderList">
    <h3>Upcoming Reminders</h3>
  </div>

  <script>
    // Show live time
    function updateClock() {
      const now = new Date();
      const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      const day = days[now.getDay()];
      const time = now.toLocaleTimeString();
      const date = now.toLocaleDateString();

      document.getElementById("clock").textContent = `${day}, ${date} - ${time}`;
    }
    setInterval(updateClock, 1000);
    updateClock();

    // Reminder logic
    let reminders = [];

    function addReminder() {
      const timeInput = document.getElementById("reminderTime").value;
      const msgInput = document.getElementById("reminderMsg").value.trim();

      if (!timeInput || !msgInput) {
        alert("Please enter both date/time and message.");
        return;
      }

      const reminderTime = new Date(timeInput);
      reminders.push({ time: reminderTime, message: msgInput, shown: false });
      document.getElementById("reminderTime").value = '';
      document.getElementById("reminderMsg").value = '';

      renderReminders();
    }

    function renderReminders() {
      const list = document.getElementById("reminderList");
      list.innerHTML = '<h3>Upcoming Reminders</h3>';
      reminders.forEach((reminder, index) => {
        const div = document.createElement("div");
        div.className = "reminder";
        div.innerHTML = `
          <div class="reminder-time">${reminder.time.toLocaleString()}</div>
          <div>${reminder.message}</div>
        `;
        list.appendChild(div);
      });
    }

    function checkReminders() {
      const now = new Date();
      reminders.forEach(reminder => {
        if (!reminder.shown && Math.abs(reminder.time - now) < 1000) {
          alert("🔔 Reminder: " + reminder.message);
          reminder.shown = true;
        }
      });
    }

    setInterval(checkReminders, 1000);
  </script>

</body>
</html>
