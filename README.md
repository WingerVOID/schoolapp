# schoolapp
# School portal with login, timetable, lessons, meals, and dashboards.
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Smart School Login</title>
<style>
body {
  font-family: Inter, system-ui;
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #0d1117, #161b22);
  color: #e6eef8;
  transition: opacity 0.6s;
}
.container {
  background: #161b22;
  padding: 40px;
  border-radius: 16px;
  width: 350px;
  box-shadow: 0 0 20px #0006;
  text-align: center;
  animation: fadeIn 1s ease;
}
@keyframes fadeIn {
  from {opacity: 0; transform: translateY(-20px);}
  to {opacity:1; transform: translateY(0);}
}
input, select {
  width: 100%;
  padding: 12px;
  margin: 10px 0;
  border-radius: 8px;
  border: 1px solid #30363d;
  background: #0d1117;
  color: #e6eef8;
}
button {
  width: 100%;
  padding: 12px;
  border: none;
  border-radius: 8px;
  background: #3a8bfd;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-top: 10px;
  transition: background 0.3s;
}
button:hover {background: #2f6fd1;}
.link {color:#58a6ff;cursor:pointer;text-decoration:underline;margin-top:10px;display:block;}
small {color:#a6b3c4;}
.msg {color:#ff6b6b;margin-top:5px;min-height:18px;}
</style>
</head>
<body>
<div class="container" id="loginBox">
  <h2>Smart School</h2>
  <select id="roleSelect">
    <option value="student">Student</option>
    <option value="teacher">Teacher</option>
    <option value="parent">Parent</option>
    <option value="admin">Admin</option>
  </select>
  <input type="text" id="username" placeholder="Username">
  <input type="password" id="password" placeholder="Password">
  <label><input type="checkbox" id="remember"> Remember Me</label>
  <button onclick="login()">Login</button>
  <span class="link" onclick="window.location.href='forgot_password.html'">Forgot Password?</span>
  <div class="msg" id="msg"></div>
</div>
<script>
// Sample accounts (demo purposes)
const STUDENTS = {"7A":Array.from({length:24},(_,i)=>({user:`7a_student${i+1}`, pass:"7a123"}))};
const TEACHERS = [{user:"math_teacher", pass:"math123", subject:"math"}];
const PARENTS = [{user:"parent1", pass:"parent123", child:"7a_student1"}];
const ADMINS = [{user:"admin", pass:"admin123"}];

// Load remembered login
window.onload = ()=>{
  const savedUser = localStorage.getItem("savedUser");
  const savedPass = localStorage.getItem("savedPass");
  const savedRole = localStorage.getItem("savedRole");
  if(savedUser && savedPass && savedRole){
    username.value = savedUser;
    password.value = savedPass;
    roleSelect.value = savedRole;
    document.getElementById("remember").checked = true;
  }
};

function fadeOut(callback){
  document.body.style.opacity = 0;
  setTimeout(callback,600);
}

function login(){
  const role = roleSelect.value;
  const u = username.value.trim().toLowerCase();
  const p = password.value.trim();
  const msg = document.getElementById("msg");
  msg.innerText = "";

  if(document.getElementById("remember").checked){
    localStorage.setItem("savedUser", u);
    localStorage.setItem("savedPass", p);
    localStorage.setItem("savedRole", role);
  } else {
    localStorage.removeItem("savedUser");
    localStorage.removeItem("savedPass");
    localStorage.removeItem("savedRole");
  }

  if(role==="student"){
    for(const grade in STUDENTS){
      for(const s of STUDENTS[grade]){
        if(s.user===u && s.pass===p){
          fadeOut(()=>{window.location.href=`student_dashboard.html?student=${u}&grade=${grade}`});
          return;
        }
      }
    }
    msg.innerText = "❌ Incorrect student login";
  } else if(role==="teacher"){
    for(const t of TEACHERS){
      if(t.user===u && t.pass===p){
        fadeOut(()=>{window.location.href=`teacher_dashboard.html?teacher=${u}&subject=${t.subject}`});
        return;
      }
    }
    msg.innerText = "❌ Incorrect teacher login";
  } else if(role==="parent"){
    for(const pObj of PARENTS){
      if(pObj.user===u && pObj.pass===p){
        fadeOut(()=>{window.location.href=`parent_dashboard.html?parent=${u}&child=${pObj.child}`});
        return;
      }
    }
    msg.innerText = "❌ Incorrect parent login";
  } else if(role==="admin"){
    for(const a of ADMINS){
      if(a.user===u && a.pass===p){
        fadeOut(()=>{window.location.href=`admin_dashboard.html?admin=${u}`});
        return;
      }
    }
    msg.innerText = "❌ Incorrect admin login";
  }
}
</script>
</body>
</html>
