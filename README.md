
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>REKSAFLIX</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#101010;color:white;font-family:Arial,sans-serif}
header{padding:18px 5%;display:flex;justify-content:space-between;align-items:center;background:#080808}
.logo{color:#e50914;font-size:25px;font-weight:bold}
nav a{color:white;text-decoration:none;margin-left:14px;font-size:13px}
.hero{padding:75px 6%;background:linear-gradient(90deg,#080808,#351014,#101010);min-height:280px}
.hero h1{font-size:38px;margin:0 0 12px}
.hero p{max-width:450px;color:#ddd;line-height:1.6}
button{border:0;padding:11px 18px;border-radius:5px;cursor:pointer;font-weight:bold}
.primary{background:#e50914;color:white}
.secondary{background:#444;color:white}
main{padding:20px 5%}
.search{width:100%;padding:13px;background:#252525;border:1px solid #555;border-radius:6px;color:white;margin-bottom:20px}
h2{font-size:19px}
.categories{display:flex;gap:9px;overflow-x:auto;margin-bottom:20px}
.categories button{background:#333;color:white;white-space:nowrap}
.categories button.active{background:#e50914}
.movies{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px}
.movie{background:#202020;border-radius:8px;overflow:hidden}
.poster{height:145px;display:flex;align-items:center;justify-content:center;font-size:42px;background:linear-gradient(135deg,#4c1017,#151515)}
.info{padding:11px}
.info h3{font-size:15px;margin:0 0 6px}
.info p{font-size:12px;color:#aaa}
.info button{font-size:12px;padding:8px;margin:3px 2px 0 0}
footer{text-align:center;color:#888;padding:30px;font-size:12px}
#dialog{display:none;position:fixed;inset:0;background:#000e;align-items:center;justify-content:center;padding:20px}
.panel{background:#222;padding:22px;border-radius:10px;width:100%;max-width:400px}
.panel button{margin-top:10px}
@media(min-width:650px){.movies{grid-template-columns:repeat(4,1fr)}.poster{height:190px}}
</style>
</head>
<body>
<header>
<div class="logo">REKSAFLIX</div>
<nav><a href="#">Beranda</a><a href="#koleksi">Film</a><a href="#favorit">Favorit</a></nav>
</header>

<section class="hero">
<h1>Hiburan Tanpa Batas.</h1>
<p>Temukan film dan tontonan pilihan dalam satu tempat. Nikmati koleksi video gratis dan legal.</p>
<button class="primary" onclick="showInfo('Selamat datang di REKSAFLIX!')">Jelajahi Sekarang</button>
</section>

<main id="koleksi">
<input class="search" id="search" placeholder="Cari judul film..." oninput="render()">
<div class="categories" id="categories">
<button class="active" onclick="filter('Semua',this)">Semua</button>
<button onclick="filter('Aksi',this)">Aksi</button>
<button onclick="filter('Drama',this)">Drama</button>
<button onclick="filter('Animasi',this)">Animasi</button>
<button onclick="filter('Komedi',this)">Komedi</button>
</div>

<h2>Film Pilihan</h2>
<div class="movies" id="movies"></div>
<h2 id="favorit">Daftar Favorit</h2>
<div class="movies" id="favorites"></div>
</main>

<footer>REKSAFLIX © 2026 · Tonton konten legal dan gratis</footer>

<div id="dialog">
<div class="panel">
<h2 id="dialogTitle"></h2>
<p id="dialogText"></p>
<button class="primary" onclick="closeDialog()">Tutup</button>
</div>
</div>

<script>
const films=[
{id:1,title:"Petualangan Dunia",genre:"Aksi",year:2025,icon:"🌍"},
{id:2,title:"Kisah Persahabatan",genre:"Drama",year:2024,icon:"🤝"},
{id:3,title:"Dunia Imajinasi",genre:"Animasi",year:2025,icon:"🚀"},
{id:4,title:"Hari Penuh Tawa",genre:"Komedi",year:2023,icon:"😂"},
{id:5,title:"Misi Rahasia",genre:"Aksi",year:2024,icon:"🕵️"},
{id:6,title:"Cerita Kehidupan",genre:"Drama",year:2025,icon:"🎬"},
{id:7,title:"Galaksi Ceria",genre:"Animasi",year:2024,icon:"🪐"},
{id:8,title:"Liburan Seru",genre:"Komedi",year:2023,icon:"🏝️"}
];
let selected="Semua";
let saved=JSON.parse(localStorage.getItem("saved")||"[]");

function card(f){
return `<article class="movie">
<div class="poster">${f.icon}</div>
<div class="info">
<h3>${f.title}</h3>
<p>${f.genre} · ${f.year}</p>
<button class="primary" onclick="watch(${f.id})">▶ Tonton</button>
<button class="secondary" onclick="favorite(${f.id})">♡ Favorit</button>
</div></article>`;
}
function render(){
let q=document.getElementById("search").value.toLowerCase();
let list=films.filter(f=>(selected==="Semua"||f.genre===selected)&&f.title.toLowerCase().includes(q));
document.getElementById("movies").innerHTML=list.map(card).join("")||"Film tidak ditemukan.";
document.getElementById("favorites").innerHTML=films.filter(f=>saved.includes(f.id)).map(card).join("")||"<p>Belum ada film favorit.</p>";
}
function filter(g,b){
selected=g;
document.querySelectorAll(".categories button").forEach(x=>x.classList.remove("active"));
b.classList.add("active");
render();
}
function favorite(id){
saved=saved.includes(id)?saved.filter(x=>x!==id):[...saved,id];
localStorage.setItem("saved",JSON.stringify(saved));
render();
}
function watch(id){
let f=films.find(x=>x.id===id);
showInfo("Film: "+f.title+" | Video belum ditambahkan. Nantinya kita hubungkan dengan video gratis yang legal.");
}
function showInfo(t){
document.getElementById("dialogTitle").innerText="REKSAFLIX";
document.getElementById("dialogText").innerText=t;
document.getElementById("dialog").style.display="flex";
}
function closeDialog(){document.getElementById("dialog").style.display="none"}
render();
</script>
</body>
</html>
