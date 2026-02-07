<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sunflower 🌻</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial,Helvetica,sans-serif}

body{
  background:linear-gradient(135deg,#a19a34,#1a002b,#070010);
  color:white;
  min-height:100vh;
}

/* ===== TELAS ===== */
.screen{
  min-height:100vh;
  display:flex;
  justify-content:center;
  align-items:center;
}

/* ===== LOGIN ===== */
.box{
  background:rgba(0,0,0,.7);
  padding:30px;
  width:330px;
  border-radius:22px;
  text-align:center;
  animation:fade .5s ease;
}

@keyframes fade{
  from{opacity:0;transform:scale(.9)}
  to{opacity:1;transform:scale(1)}
}

.sunflower{
  width:80px;
  height:80px;
  margin:0 auto 15px;
  border-radius:50%;
  background:repeating-conic-gradient(
    #ffb703 0 15deg,
    transparent 15deg 30deg
  );
  animation:spin 18s linear infinite;
}
@keyframes spin{to{transform:rotate(360deg)}}

input{
  width:100%;
  padding:12px;
  margin:8px 0;
  border-radius:12px;
  border:none;
  background:#1c1c1c;
  color:white;
}

button{
  width:100%;
  padding:12px;
  margin-top:10px;
  border:none;
  border-radius:14px;
  font-size:15px;
  cursor:pointer;
  background:linear-gradient(90deg,#ffcc00,#ff9800);
}

button:hover{transform:scale(1.04)}

.error{
  margin-top:10px;
  color:#ff5c5c;
}

/* ===== APP ===== */
#app{display:none}

header{
  position:fixed;
  top:0;
  width:100%;
  background:#000;
  padding:15px;
  display:flex;
  align-items:center;
  gap:15px;
  z-index:10;
}

.menu-btn{
  font-size:22px;
  cursor:pointer;
}

.menu{
  position:fixed;
  left:-260px;
  top:0;
  width:250px;
  height:100%;
  background:#111;
  padding:20px;
  transition:.3s;
  z-index:20;
}

.menu.active{left:0}

.menu h3{
  margin-bottom:20px;
}

.menu a{
  display:block;
  padding:14px;
  color:white;
  text-decoration:none;
  border-radius:10px;
}

.menu a:hover{
  background:#222;
}

main{
  padding:90px 20px 40px;
  max-width:900px;
  margin:auto;
}

.card{
  background:rgba(0,0,0,.6);
  padding:25px;
  border-radius:20px;
  margin-bottom:20px;
  animation:slide .4s ease;
}

@keyframes slide{
  from{opacity:0;transform:translateY(15px)}
  to{opacity:1;transform:translateY(0)}
}
</style>
</head>

<body>

<!-- LOGIN -->
<div id="login" class="screen">
  <div class="box">
    <div class="sunflower"></div>
    <h2>Sunflower 🌻</h2>

    <input id="email" placeholder="Email">
    <input id="password" type="password" placeholder="Senha">

    <button onclick="login()">Entrar</button>
    <button onclick="loginFacebook()">Entrar com Facebook</button>

    <p id="msg" class="error"></p>
  </div>
</div>

<!-- APP -->
<div id="app">

<header>
  <span class="menu-btn" onclick="toggleMenu()">☰</span>
  <strong>Sunflower 🌻</strong>
</header>

<nav class="menu" id="menu">
  <h3 id="userName"></h3>
  <a onclick="openPage('home')">🏠 Home</a>
  <a onclick="openPage('perfil')">🔐 Perfil</a>
  <a onclick="openPage('config')">⚙️ Configurações</a>
  <a onclick="logout()">🚪 Sair</a>
</nav>

<main id="content"></main>
</div>

<script>
/* ===== BACKEND FAKE ===== */
const users=[
  {email:"admin@sunflower.com",senha:"123456",nome:"Maycon"},
  {email:"user@sunflower.com",senha:"654321",nome:"Usuário"}
];

const content=document.getElementById("content");
const menu=document.getElementById("menu");
const msg=document.getElementById("msg");

/* ===== LOGIN ===== */
function login(){
  const e=document.getElementById("email").value;
  const p=document.getElementById("password").value;

  const user=users.find(u=>u.email===e && u.senha===p);
  if(!user){
    msg.innerText="Email ou senha inválidos";
    return;
  }

  localStorage.setItem("user",JSON.stringify(user));
  enter();
}

function loginFacebook(){
  const fbUser={nome:"Facebook User",email:"fb@sunflower.com"};
  localStorage.setItem("user",JSON.stringify(fbUser));
  enter();
}

/* ===== APP ===== */
function enter(){
  document.getElementById("login").style.display="none";
  document.getElementById("app").style.display="block";

  const u=JSON.parse(localStorage.getItem("user"));
  document.getElementById("userName").innerText=u.nome;

  openPage("home");
}

function toggleMenu(){
  menu.classList.toggle("active");
}

function openPage(page){
  menu.classList.remove("active");

  if(page==="home"){
    content.innerHTML=`
      <div class="card">
        <h2>Bem-vindo 🌻</h2>
        <p>Seu app está funcionando como um site real.</p>
      </div>
    `;
  }

  if(page==="perfil"){
    const u=JSON.parse(localStorage.getItem("user"));
    content.innerHTML=`
      <div class="card">
        <h2>Perfil</h2>
        <p><b>Nome:</b> ${u.nome}</p>
        <p><b>Email:</b> ${u.email}</p>
      </div>
    `;
  }

  if(page==="config"){
    content.innerHTML=`
      <div class="card">
        <h2>Configurações</h2>
        <p>Modo app ativo 📱</p>
      </div>
    `;
  }
}

function logout(){
  localStorage.clear();
  location.reload();
}

/* AUTO LOGIN */
if(localStorage.getItem("user")){
  enter();
}
</script>

</body>
</html>
