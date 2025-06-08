<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Fórum CNFVB</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #111;
      color: #fff;
    }

    header {
      background: linear-gradient(to right, #b30000, #ffcc00);
      padding: 1rem;
      text-align: center;
      position: relative;
    }

    header h1 {
      color: #fff;
      margin: 0;
      font-size: 2rem;
    }

    #avatar {
      position: absolute;
      top: 10px;
      right: 10px;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #fff;
      display: none;
    }

    nav {
      background-color: #222;
      display: flex;
      justify-content: center;
      gap: 2rem;
      padding: 0.5rem 0;
      border-bottom: 2px solid #ffcc00;
    }

    nav a {
      color: #ffcc00;
      text-decoration: none;
      font-weight: bold;
    }

    .forum-container {
      max-width: 900px;
      margin: 2rem auto;
      padding: 1rem;
      background-color: #1a1a1a;
      border-radius: 10px;
    }

    .forum-section {
      margin-bottom: 2rem;
    }

    .forum-section h2 {
      border-bottom: 2px solid #ffcc00;
      padding-bottom: 0.5rem;
      color: #ffcc00;
    }

    .topic {
      background-color: #2a2a2a;
      margin: 0.5rem 0;
      padding: 0.75rem;
      border-radius: 8px;
    }

    .topic-title {
      cursor: pointer;
      font-weight: bold;
    }

    .comment-box, .comments {
      margin-top: 0.5rem;
      display: none;
    }

    .comment-box textarea {
      width: 100%;
      height: 50px;
      background: #333;
      color: white;
      border: none;
      border-radius: 5px;
      margin-top: 5px;
    }

    .comment-box button {
      margin-top: 5px;
      background-color: #b30000;
      border: none;
      color: white;
      padding: 0.5rem;
      border-radius: 6px;
      cursor: pointer;
    }

    .new-topic input, .new-topic textarea, #searchBar {
      width: 100%;
      padding: 0.5rem;
      margin: 0.5rem 0;
      border: none;
      border-radius: 4px;
      background-color: #333;
      color: white;
    }

    .new-topic button {
      background-color: #b30000;
      color: white;
      padding: 0.5rem 1rem;
      border: none;
      border-radius: 6px;
      font-weight: bold;
      cursor: pointer;
    }

    footer {
      text-align: center;
      padding: 1rem;
      background-color: #111;
      color: #999;
      font-size: 0.9rem;
    }

    .login-box {
      max-width: 400px;
      margin: 4rem auto;
      padding: 2rem;
      background-color: #1a1a1a;
      border-radius: 10px;
      text-align: center;
    }

    .login-box input {
      width: 90%;
      margin: 0.5rem 0;
      padding: 0.5rem;
      border: none;
      border-radius: 5px;
      background: #333;
      color: white;
    }

    .login-box button {
      margin-top: 1rem;
      background-color: #b30000;
      color: white;
      padding: 0.5rem 1rem;
      border: none;
      border-radius: 6px;
      font-weight: bold;
      cursor: pointer;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <!-- Login -->
  <div class="login-box" id="loginBox">
    <h2>Login CNFVB</h2>
    <input type="text" id="user" placeholder="Usuário">
    <input type="password" id="pass" placeholder="Senha">
    <input type="file" id="avatarUpload" accept="image/*">
    <button onclick="login()">Entrar</button>
    <p style="color:#ffcc00; font-size: 0.9rem;">Usuário: <b>admin</b> | Senha: <b>1234</b></p>
  </div>

  <!-- Fórum -->
  <div id="forum" class="hidden">
    <header>
      <h1>Fórum CNFVB</h1>
      <img id="avatar" alt="Avatar">
    </header>

    <nav>
      <a href="#sub15">Sub-15</a>
      <a href="#sub17">Sub-17</a>
      <a href="#geral">Geral</a>
    </nav>

    <div class="forum-container">
      <input type="text" id="searchBar" placeholder="🔍 Buscar tópicos...">

      <!-- Sub-15 -->
      <div class="forum-section" id="sub15">
        <h2>Sub-15</h2>
        <div class="topics" data-categoria="sub15"></div>
        <div class="new-topic">
          <input type="text" id="titleSub15" placeholder="Título">
          <textarea id="msgSub15" placeholder="Mensagem..."></textarea>
          <button onclick="addTopic('sub15')">Publicar</button>
        </div>
      </div>

      <!-- Sub-17 -->
      <div class="forum-section" id="sub17">
        <h2>Sub-17</h2>
        <div class="topics" data-categoria="sub17"></div>
        <div class="new-topic">
          <input type="text" id="titleSub17" placeholder="Título">
          <textarea id="msgSub17" placeholder="Mensagem..."></textarea>
          <button onclick="addTopic('sub17')">Publicar</button>
        </div>
      </div>

      <!-- Geral -->
      <div class="forum-section" id="geral">
        <h2>Geral</h2>
        <div class="topics" data-categoria="geral"></div>
        <div class="new-topic">
          <input type="text" id="titleGeral" placeholder="Título">
          <textarea id="msgGeral" placeholder="Mensagem..."></textarea>
          <button onclick="addTopic('geral')">Publicar</button>
        </div>
      </div>
    </div>

    <footer>
      &copy; 2025 CNFVB - Confederação Nevense de Futebol de Base.
    </footer>
  </div>

  <script>
    const avatarEl = document.getElementById('avatar');
    const forum = document.getElementById('forum');
    const loginBox = document.getElementById('loginBox');

    function login() {
      const user = document.getElementById('user').value;
      const pass = document.getElementById('pass').value;
      const file = document.getElementById('avatarUpload').files[0];

      if (user === 'admin' && pass === '1234') {
        loginBox.classList.add('hidden');
        forum.classList.remove('hidden');

        if (file) {
          const reader = new FileReader();
          reader.onload = () => {
            avatarEl.src = reader.result;
            avatarEl.style.display = 'block';
          };
          reader.readAsDataURL(file);
        }

        loadTopics();
      } else {
        alert('Usuário ou senha incorretos!');
      }
    }

    function addTopic(category) {
      const title = document.getElementById(`title${capitalize(category)}`).value;
      const msg = document.getElementById(`msg${capitalize(category)}`).value;
      if (!title || !msg) return alert("Preencha todos os campos!");

      const topic = {
        title, msg, comments: []
      };

      const stored = JSON.parse(localStorage.getItem(category) || '[]');
      stored.push(topic);
      localStorage.setItem(category, JSON.stringify(stored));
      renderTopics(category);
    }

    function renderTopics(category) {
      const container = document.querySelector(`.topics[data-categoria="${category}"]`);
      container.innerHTML = '';
      const data = JSON.parse(localStorage.getItem(category) || '[]');

      data.forEach((topic, index) => {
        const div = document.createElement('div');
        div.className = 'topic';
        div.innerHTML = `
          <div class="topic-title" onclick="toggleComments(this)">📌 ${topic.title}</div>
          <div class="comments">
            <p>${topic.msg}</p>
            <div class="comment-list" id="${category}-comments-${index}">
              ${(topic.comments || []).map(c => `<p>- ${c}</p>`).join('')}
            </div>
            <div class="comment-box">
              <textarea placeholder="Comente..." onkeydown="if(event.key==='Enter'){event.preventDefault();addComment('${category}', ${index}, this)}"></textarea>
              <button onclick="addComment('${category}', ${index}, this.previousElementSibling)">Comentar</button>
            </div>
          </div>`;
        container.appendChild(div);
      });
    }

    function toggleComments(el) {
      const comments = el.nextElementSibling;
      comments.style.display = comments.style.display === 'block' ? 'none' : 'block';
    }

    function addComment(category, index, textarea) {
      const comment = textarea.value.trim();
      if (!comment) return;
      const data = JSON.parse(localStorage.getItem(category));
      data[index].comments.push(comment);
      localStorage.setItem(category, JSON.stringify(data));
      renderTopics(category);
    }

    function loadTopics() {
      ['sub15', 'sub17', 'geral'].forEach(renderTopics);
    }

    function capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }

    document.getElementById('searchBar')?.addEventListener('input', function () {
      const query = this.value.toLowerCase();
      document.querySelectorAll('.topic').forEach(t => {
        t.style.display = t.textContent.toLowerCase().includes(query) ? 'block' : 'none';
      });
    });
  </script>
</body>
</html>
