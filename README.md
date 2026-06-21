# Carebears-
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<title>Care Bears Shop</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:linear-gradient(135deg,#ffe6f0,#e6f0ff);
  transition:0.3s;
}

h1{
  text-align:center;
  padding:12px;
}

/* 工具列 */
.topbar{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:8px;
  padding:10px;
}

input, select, button{
  padding:10px;
  border-radius:12px;
  border:none;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
  cursor:pointer;
}

/* 卡片區 */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(160px,1fr));
  gap:15px;
  padding:15px;
}

/* 卡片 */
.card{
  background:white;
  border-radius:18px;
  overflow:hidden;
  position:relative;
  cursor:pointer;
  box-shadow:0 6px 18px rgba(0,0,0,0.12);
  transition:0.25s;
}

.card:hover{
  transform:translateY(-6px) scale(1.02);
  box-shadow:0 10px 25px rgba(0,0,0,0.2);
}

.card img{
  width:100%;
  height:140px;
  object-fit:cover;
}

.card h3{
  font-size:14px;
  margin:8px;
}

/* 愛心 */
.heart{
  position:absolute;
  top:8px;
  right:10px;
  font-size:18px;
  cursor:pointer;
}

/* badge */
.badge{
  background:linear-gradient(45deg,#ff4d6d,#ff8fa3);
  color:white;
  padding:3px 8px;
  border-radius:999px;
  font-size:12px;
}

/* 彈窗 */
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,0.6);
  display:none;
  justify-content:center;
  align-items:center;
}

.modal-box{
  background:white;
  padding:18px;
  border-radius:18px;
  width:260px;
}

/* 深色模式 */
.dark{
  background:#0f0f14;
  color:white;
}

.dark .card{
  background:#1c1c26;
  color:white;
}

.dark input,
.dark select,
.dark button{
  background:#2a2a38;
  color:white;
}
</style>
</head>

<body>

<h1>🧸 Care Bears Shop</h1>

<div class="topbar">

<input id="search" placeholder="搜尋熊熊">

<select id="category">
  <option value="all">全部</option>
  <option value="normal">一般</option>
  <option value="halloween">萬聖節</option>
  <option value="christmas">聖誕節</option>
  <option value="valentine">情人節</option>
  <option value="fav">❤️ 收藏夾</option>
</select>

<select id="sort">
  <option value="id">排序ID</option>
  <option value="name">排序名稱</option>
</select>

<button onclick="toggleDark()">🌙</button>

<div>❤️ <span class="badge" id="favCount">0</span></div>

</div>

<div class="grid" id="grid"></div>

<div class="modal" id="modal">
  <div class="modal-box">
    <img id="mimg" style="width:100%;border-radius:10px;">
    <h3 id="mname"></h3>
    <p id="mdesc"></p>
    <button onclick="closeModal()">關閉</button>
  </div>
</div>

<script>

/* ===== 50隻資料（簡化版，可換你完整JSON） ===== */
const data = Array.from({length:50}).map((_,i)=>({
  id:i+1,
  name:"Bear "+(i+1),
  img:"https://via.placeholder.com/300",
  tag:i%4===0?"halloween":i%4===1?"christmas":i%4===2?"valentine":"normal",
  desc:"可愛熊熊 "+(i+1)
}));

/* 收藏 */
let fav = JSON.parse(localStorage.getItem("fav")||"[]");

/* 渲染 */
function render(list){
  const grid=document.getElementById("grid");
  grid.innerHTML="";

  list.forEach(b=>{

    const div=document.createElement("div");
    div.className="card";

    const isFav = fav.includes(b.id);

    div.innerHTML=`
      <div class="heart" onclick="toggleFav(event,${b.id})">
        ${isFav?"❤️":"🤍"}
      </div>
      <img src="${b.img}">
      <h3>${b.name}</h3>
    `;

    div.onclick=()=>openModal(b);
    grid.appendChild(div);
  });

  document.getElementById("favCount").innerText=fav.length;
}

/* 收藏 */
function toggleFav(e,id){
  e.stopPropagation();

  if(fav.includes(id)){
    fav = fav.filter(x=>x!==id);
  }else{
    fav.push(id);
  }

  localStorage.setItem("fav",JSON.stringify(fav));
  applyFilters();
}

/* modal */
function openModal(b){
  document.getElementById("modal").style.display="flex";
  document.getElementById("mimg").src=b.img;
  document.getElementById("mname").innerText=b.name;
  document.getElementById("mdesc").innerText=b.desc;
}

function closeModal(){
  document.getElementById("modal").style.display="none";
}

/* 深色 */
function toggleDark(){
  document.body.classList.toggle("dark");
}

/* 篩選＋排序核心 */
function applyFilters(){

  let list=[...data];

  const cat=document.getElementById("category").value;
  const search=document.getElementById("search").value.toLowerCase();
  const sort=document.getElementById("sort").value;

  if(cat==="fav"){
    list=list.filter(x=>fav.includes(x.id));
  }else if(cat!=="all"){
    list=list.filter(x=>x.tag===cat);
  }

  if(search){
    list=list.filter(x=>x.name.toLowerCase().includes(search));
  }

  if(sort==="name"){
    list.sort((a,b)=>a.name.localeCompare(b.name));
  }else{
    list.sort((a,b)=>a.id-b.id);
  }

  render(list);
}

/* 事件 */
document.getElementById("search").oninput=applyFilters;
document.getElementById("category").onchange=applyFilters;
document.getElementById("sort").onchange=applyFilters;

/* 初始 */
applyFilters();

</script>

</body>

</html>
