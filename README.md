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
  background:linear-gradient(135deg,#fff0f6,#e6f7ff);
}

/* 頂部 */
h1{
  text-align:center;
  padding:18px;
  font-size:26px;
}

/* 工具列 */
.topbar{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:10px;
  padding:10px;
}

input, select, button{
  padding:10px 12px;
  border-radius:14px;
  border:none;
  box-shadow:0 4px 12px rgba(0,0,0,0.1);
  font-size:14px;
}

/* GRID */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(170px,1fr));
  gap:16px;
  padding:16px;
}

/* 卡片升級 */
.card{
  background:white;
  border-radius:18px;
  overflow:hidden;
  position:relative;
  cursor:pointer;
  box-shadow:0 10px 25px rgba(0,0,0,0.12);
  transition:0.25s;
}

.card:hover{
  transform:translateY(-6px) scale(1.02);
  box-shadow:0 15px 30px rgba(0,0,0,0.2);
}

.card img{
  width:100%;
  height:150px;
  object-fit:cover;
}

.card h3{
  font-size:14px;
  margin:8px;
  padding:0 6px;
}

/* ❤️ 收藏 */
.heart{
  position:absolute;
  top:8px;
  right:10px;
  font-size:18px;
  background:white;
  border-radius:50%;
  padding:4px;
}

/* badge */
.badge{
  background:linear-gradient(45deg,#ff4d6d,#ff85a2);
  color:white;
  padding:3px 10px;
  border-radius:999px;
  font-size:12px;
}

/* modal */
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
  width:280px;
}

/* 🌙 深色模式 */
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
  <option value="fav">❤️ 收藏</option>
</select>

<select id="sort">
  <option value="id">排序：ID</option>
  <option value="name">排序：名稱</option>
</select>

<button onclick="toggleDark()">🌙</button>

<div>❤️ <span class="badge" id="favCount">0</span></div>

</div>

<div class="grid" id="grid"></div>

<!-- 彈窗 -->
<div class="modal" id="modal">
  <div class="modal-box">
    <img id="mimg" style="width:100%;border-radius:10px;">
    <h3 id="mname"></h3>
    <p id="mdesc"></p>
    <button onclick="closeModal()">關閉</button>
  </div>
</div>

<script>

/*const data = [
{ id:1, name:"America Cares Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"白色／星條旗愛心圖案｜象徵關懷與團結。" },
{ id:2, name:"Perfect Panda", tag:"normal", img:"https://via.placeholder.com/300", desc:"黑白色／流星造型｜追求平衡與自信。" },
{ id:3, name:"Hopeful Heart Bear", tag:"valentine", img:"https://via.placeholder.com/300", desc:"粉色／彩虹愛心圖案｜代表希望與正向力量。" },
{ id:4, name:"Love-a-Lot Bear", tag:"valentine", img:"https://via.placeholder.com/300", desc:"粉紅色／雙愛心圖案｜象徵愛與關懷。" },
{ id:5, name:"Cheer Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"桃粉色／彩虹圖案｜樂觀開朗。" },

{ id:6, name:"Secret Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"守護秘密。" },
{ id:7, name:"Always There Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"陪伴支持。" },
{ id:8, name:"Friend Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"友情。" },
{ id:9, name:"Work of Heart Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"創意用心。" },
{ id:10, name:"Sparkle Heart Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"閃耀希望。" },

{ id:11, name:"Birthday Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"生日快樂。" },
{ id:12, name:"Funshine Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"快樂陽光。" },
{ id:13, name:"Laugh-a-Lot Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"歡笑。" },
{ id:14, name:"Do-Your-Best Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"努力。" },
{ id:15, name:"Good Luck Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"幸運。" },

{ id:16, name:"Wish Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"願望。" },
{ id:17, name:"Grams Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"慈愛。" },
{ id:18, name:"Grams Bear (New)", tag:"normal", img:"https://via.placeholder.com/300", desc:"新版。" },
{ id:19, name:"Bedtime Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"睡眠。" },
{ id:20, name:"Sea Friend Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"海洋。" },

{ id:21, name:"Heart Song Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"音樂。" },
{ id:22, name:"Grumpy Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"外冷內暖。" },
{ id:23, name:"Sweet Dreams Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"夢境。" },
{ id:24, name:"Share Bear Milkshake", tag:"normal", img:"https://via.placeholder.com/300", desc:"分享。" },
{ id:25, name:"Share Bear (New)", tag:"normal", img:"https://via.placeholder.com/300", desc:"分享。" },

{ id:26, name:"Take Care Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"照顧。" },
{ id:27, name:"Harmony Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"和平。" },
{ id:28, name:"Best Friend Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"好友。" },
{ id:29, name:"Forest Friend Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"自然。" },
{ id:30, name:"Tenderheart Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"關懷。" },

{ id:31, name:"True Heart Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"真誠。" },
{ id:32, name:"Togetherness Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"團結。" },
{ id:33, name:"Calming Heart Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"平靜。" },
{ id:34, name:"Care-a-Lot Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"關愛。" },
{ id:35, name:"Dream Bright Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"夢想。" },

{ id:36, name:"I Care Bear", tag:"normal", img:"https://via.placeholder.com/300", desc:"關心世界。" },

{ id:37, name:"All My Heart Bear", tag:"valentine", img:"https://via.placeholder.com/300", desc:"情人節。" },
{ id:38, name:"Sweet Messages Bear", tag:"valentine", img:"https://via.placeholder.com/300", desc:"訊息。" },

{ id:39, name:"Trick-or-Sweet Bear", tag:"halloween", img:"https://via.placeholder.com/300", desc:"萬聖節。" },
{ id:40, name:"Trick-or-Sweet Bear (2022–2023)", tag:"halloween", img:"https://via.placeholder.com/300", desc:"萬聖節。" },
{ id:41, name:"Spooky Sparkle Bear", tag:"halloween", img:"https://via.placeholder.com/300", desc:"閃耀。" },
{ id:42, name:"Cheer Bear Halloween", tag:"halloween", img:"https://via.placeholder.com/300", desc:"蝙蝠。" },
{ id:43, name:"Tenderheart Bear Halloween", tag:"halloween", img:"https://via.placeholder.com/300", desc:"骷髏。" },
{ id:44, name:"Good Luck Bear Halloween", tag:"halloween", img:"https://via.placeholder.com/300", desc:"科學怪人。" },

{ id:45, name:"Christmas Wishes Bear (2021)", tag:"christmas", img:"https://via.placeholder.com/300", desc:"聖誕。" },
{ id:46, name:"Christmas Wishes Bear (2022)", tag:"christmas", img:"https://via.placeholder.com/300", desc:"聖誕。" },
{ id:47, name:"Christmas Wishes Bear (2024)", tag:"christmas", img:"https://via.placeholder.com/300", desc:"馴鹿。" },
{ id:48, name:"Christmas Wishes Bear (2025)", tag:"christmas", img:"https://via.placeholder.com/300", desc:"聖誕樹。" },

{ id:49, name:"Grumpy Bear (Birthday)", tag:"normal", img:"https://via.placeholder.com/300", desc:"生日。" },
{ id:50, name:"Cheer Bear (Birthday)", tag:"normal", img:"https://via.placeholder.com/300", desc:"生日。" }
];  */
const data = window.data || [];

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

/* 篩選 */
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

/* 初始化 */
applyFilters();

</script>

</body>
</html>
