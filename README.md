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
    <p id="greeting" style="margin-top: 20px; font-size: 18px; color: green;"></p>
  </div>

  <script>
    const scriptURL = "https://script.google.com/macros/s/AKfycbwycawQwWLJ50sOvindVJMtjCj0ljtLgfhcNxAdk6fhJ-WO_GYVa-qJNC2-ZAqLUHaqEg/exec"; // ใส่ URL ของ Google Apps Script
    const form = document.getElementById("loginForm");
    const greeting = document.getElementById("greeting");

    form.addEventListener("submit", (e) => {
      e.preventDefault();

      // รับค่าจากฟอร์ม
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      // ส่งข้อมูลไปยัง Google Sheets
      const formData = new FormData();
      formData.append("username", username);
      formData.append("password", password);

      fetch(scriptURL, { method: "POST", body: formData })
        .then((response) => {
          if (response.ok) {
            // แสดงชื่อผู้ใช้
            greeting.textContent = `ยินดีต้อนรับ, ${username}!`;
          } else {
            alert("Error: " + response.statusText);
          }
        })
        .catch((error) => alert("Fetch Error: " + error.message));
    });
  </script>
</body>
</html>
