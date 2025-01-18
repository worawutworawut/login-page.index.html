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
      <input type="text" id="username" placeholder="Username" required>
      <input type="password" id="password" placeholder="Password" required>
      <button type="submit">Login</button>
    </form>
    <button id="logoutButton" style="display: none;">Logout</button>
    <p id="greeting" style="margin-top: 20px; font-size: 18px; color: green;"></p>
  </div>

  <script>
    const scriptURL = "http://192.168.0.241/login";  // เปลี่ยนเป็น IP ของ ESP32 ของคุณ
    const logoutURL = "http://192.168.0.241/logout";  // เปลี่ยนเป็น IP ของ ESP32 ของคุณ
    const form = document.getElementById("loginForm");
    const logoutButton = document.getElementById("logoutButton");
    const greeting = document.getElementById("greeting");

    form.addEventListener("submit", (e) => {
      e.preventDefault();

      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      fetch(`${scriptURL}?username=${username}&password=${password}`)
        .then((response) => response.text())
        .then((data) => {
          greeting.textContent = `Welcome, ${username}!`;
          form.style.display = "none";
          logoutButton.style.display = "inline";
        })
        .catch((error) => alert("Error: " + error.message));
    });

    logoutButton.addEventListener("click", () => {
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      fetch(`${logoutURL}?username=${username}&password=${password}`)
        .then((response) => response.text())
        .then((data) => {
          greeting.textContent = "Logged out successfully!";
          form.style.display = "block";
          logoutButton.style.display = "none";
        })
        .catch((error) => alert("Error: " + error.message));
    });
  </script>
</body>
</html>
