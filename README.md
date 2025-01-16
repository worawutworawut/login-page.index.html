<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Login Page</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      text-align: center;
      padding: 50px;
    }
    .login-container {
      background: #fff;
      padding: 20px;
      margin: auto;
      max-width: 300px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      border-radius: 10px;
    }
    input[type="text"], input[type="password"] {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    button {
      background: #007bff;
      color: white;
      padding: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
  </style>
</head>
<body>
  <div class="login-container">
    <h1>Login</h1>
    <form id="loginForm">
      <input type="text" id="username" placeholder="ชื่อผู้ใช้งาน" required>
      <input type="password" id="password" placeholder="รหัสนักศึกษา" required>
      <button type="submit">Login</button>
    </form>
    <button id="logoutButton" style="display: none;">Logout</button>
    <p id="greeting" style="margin-top: 20px; font-size: 18px; color: green;"></p>
  </div>

  <script>
    const scriptURL = "http://192.168.0.241/login"; // URL ของ Arduino (ESP32)
    const logoutURL = "http://192.168.0.241/logout"; // URL สำหรับออกจากระบบ
    const form = document.getElementById("loginForm");
    const logoutButton = document.getElementById("logoutButton");
    const greeting = document.getElementById("greeting");

    // ฟังก์ชั่นเมื่อกดล็อกอิน
    form.addEventListener("submit", (e) => {
      e.preventDefault();

      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      // ส่งข้อมูลไปยัง Arduino (ESP32) เพื่อทำการล็อกอิน
      fetch(`${scriptURL}?username=${username}&password=${password}`)
        .then((response) => response.text())
        .then((data) => {
          // แสดงข้อความยินดีต้อนรับ
          greeting.textContent = `ยินดีต้อนรับ, ${username}!`;
          form.style.display = "none";  // ซ่อนฟอร์มเมื่อล็อกอินแล้ว
          logoutButton.style.display = "inline";  // แสดงปุ่ม Logout
        })
        .catch((error) => alert("Fetch Error: " + error.message));
    });

    // ฟังก์ชั่นเมื่อกดออกจากระบบ
    logoutButton.addEventListener("click", () => {
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      // ส่งคำขอออกจากระบบไปยัง Arduino (ESP32)
      fetch(`${logoutURL}?username=${username}&password=${password}`)
        .then((response) => response.text())
        .then((data) => {
          // แสดงข้อความหลังออกจากระบบ
          greeting.textContent = "Logged out successfully!";
          form.style.display = "block";  // แสดงฟอร์มอีกครั้ง
          logoutButton.style.display = "none";  // ซ่อนปุ่ม Logout
        })
        .catch((error) => alert("Fetch Error: " + error.message));
    });
  </script>
</body>
</html>
