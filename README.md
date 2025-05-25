<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>HiTube</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
    }
    .sidebar {
      width: 250px;
      background-color: #f1f1f1;
      padding-top: 20px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      border-right: 2px solid #ccc;
      overflow-y: auto;
      height: 100vh;
      position: fixed;
    }
    .sidebar a {
      padding: 12px 20px;
      text-decoration: none;
      color: #000;
      font-size: 16px;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .sidebar a:hover {
      background-color: #ddd;
    }
    .sidebar img.icon {
      width: 20px;
      height: 20px;
    }
    .main-content {
      margin-left: 250px;
      padding: 20px;
      flex: 1;
    }
    .topbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 20px;
      background-color: #ffffff;
      border-bottom: 2px solid #ccc;
    }
    .logo {
      height: 40px;
    }
    .search-bar {
      flex: 1;
      margin: 0 20px;
    }
    .search-bar input {
      width: 100%;
      padding: 8px;
      font-size: 16px;
    }
    .avatar-wrapper {
      position: relative;
      cursor: pointer;
    }
    .avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
    }
    .checkmark {
      position: absolute;
      bottom: -5px;
      right: -5px;
      width: 16px;
      height: 16px;
      background-color: #4CAF50;
      color: white;
      font-size: 12px;
      text-align: center;
      border-radius: 50%;
      font-weight: bold;
    }
    .avatar-menu {
      display: none;
      position: absolute;
      top: 50px;
      right: 0;
      background: #fff;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 10px;
      z-index: 10;
      width: 200px;
    }
    .avatar-menu input,
    .avatar-menu button {
      width: 100%;
      margin-top: 10px;
    }
    .video-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    .video-card {
      background-color: #fff;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 10px;
    }
    .video-card video {
      width: 100%;
      border-radius: 5px;
    }
    .video-card h3 {
      font-size: 16px;
      margin-top: 10px;
    }
    .upload-btn {
      background-color: #ff0000;
      color: #fff;
      border: none;
      padding: 10px 16px;
      border-radius: 5px;
      cursor: pointer;
      margin-top: 10px;
    }
    #uploadForm,
    #adminPanel {
      display: none;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div class="sidebar">
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/25/25694.png" class="icon" />Главная</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/2589/2589175.png" class="icon" />Shorts</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/2989/2989832.png" class="icon" />Подписки</a>
    <hr />
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/2921/2921222.png" class="icon" />Библиотека</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/1828/1828970.png" class="icon" />История</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/847/847969.png" class="icon" />Ваши видео</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/2991/2991148.png" class="icon" />Смотреть позже</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/889/889140.png" class="icon" />Понравившиеся</a>
    <hr />
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/833/833314.png" class="icon" />Кино</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/126/126472.png" class="icon" />Тренды</a>
    <a href="#"><img src="https://cdn-icons-png.flaticon.com/512/3585/3585109.png" class="icon" />Обучение</a>
  </div>
  <div class="main-content">
    <div class="topbar">
      <img src="https://i.yapx.cc/ZG4QN.png" alt="HiTube Logo" class="logo" />
      <div class="search-bar">
        <input type="text" placeholder="Поиск видео..." />
      </div>
      <div class="avatar-wrapper" onclick="toggleMenu()">
        <img id="user-avatar" src="https://cdn-icons-png.flaticon.com/512/149/149071.png" alt="Avatar" class="avatar" />
        <div class="checkmark">✓</div>
        <div class="avatar-menu" id="avatar-menu">
          <label>Сменить фото:</label>
          <input type="file" id="avatar-input" accept="image/*" />
          <input type="text" id="username-input" placeholder="Новое имя" />
          <button onclick="updateProfile()">Сохранить</button>
        </div>
      </div>
    </div>

    <h1 id="username">Рекомендации для вас</h1>

    <button class="upload-btn" onclick="document.getElementById('uploadForm').style.display='block'">➕ Добавить видео</button>
    <button class="upload-btn" onclick="document.getElementById('adminPanel').style.display='block'">🛠 Панель разработчика</button>

    <div id="uploadForm">
      <input type="file" id="videoInput" accept="video/mp4" />
      <input type="text" id="videoTitle" placeholder="Введите название видео" />
      <input type="text" id="videoDesc" placeholder="Описание видео" />
      <input type="text" id="videoTags" placeholder="Хэштеги через запятую" />
      <button class="upload-btn" onclick="addVideo()">Загрузить</button>
    </div>

    <div id="adminPanel">
      <h3>Панель разработчика</h3>
      <input type="text" id="banUser" placeholder="Ник пользователя для бана" />
      <button onclick="alert('Аккаунт забанен: ' + document.getElementById('banUser').value)">Бан аккаунта</button>

      <input type="text" id="banVideo" placeholder="ID или имя видео для бана" />
      <button onclick="alert('Видео забанено: ' + document.getElementById('banVideo').value)">Бан видео</button>

      <input type="text" id="makeAdmin" placeholder="Ник для выдачи админки" />
      <button onclick="alert('Назначен администратором: ' + document.getElementById('makeAdmin').value)">Назначить администратором</button>

      <button onclick="alert('ПРАВИЛА СЕРВИСА:\n1. Никакого спама\n2. Уважайте других\n3. Не нарушайте авторские права')">Выдать правила</button>
    </div>

    <div class="video-grid" id="videoGrid">
      <!-- Видео карточки будут здесь -->
    </div>
  </div>

  <script>
    function toggleMenu() {
      const menu = document.getElementById("avatar-menu");
      menu.style.display = menu.style.display === "block" ? "none" : "block";
    }

    function updateProfile() {
      const avatarInput = document.getElementById("avatar-input");
      const usernameInput = document.getElementById("username-input");

      const name = usernameInput.value;
      if (name) {
        document.getElementById("username").textContent = name;
      }

      const file = avatarInput.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function (e) {
          document.getElementById("user-avatar").src = e.target.result;
        };
        reader.readAsDataURL(file);
      }

      toggleMenu();
    }

    function addVideo() {
      const fileInput = document.getElementById("videoInput");
      const titleInput = document.getElementById("videoTitle");
      const descInput = document.getElementById("videoDesc");
      const tagsInput = document.getElementById("videoTags");

      const file = fileInput.files[0];
      const title = titleInput.value || "Без названия";
      const desc = descInput.value || "Описание отсутствует";
      const tags = tagsInput.value || "";

      if (file && file.type === "video/mp4") {
        const reader = new FileReader();
        reader.onload = function (e) {
          const videoGrid = document.getElementById("videoGrid");
          const videoCard = document.createElement("div");
          videoCard.className = "video-card";
          videoCard.innerHTML = `
            <video controls src="${e.target.result}"></video>
            <h3>${title}</h3>
            <p>${desc}</p>
            <p><small>Теги: ${tags}</small></p>
          `;
          videoGrid.prepend(videoCard);
          fileInput.value = "";
          titleInput.value = "";
          descInput.value = "";
          tagsInput.value = "";
        };
        reader.readAsDataURL(file);
      }
    }
  </script>
</body>
</html>
