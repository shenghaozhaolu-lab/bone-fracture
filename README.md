# bone-fracture
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bone Fracture Detection System</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Segoe UI,sans-serif;
}

body{
    background:#f5f7fa;
}

header{
    background:#0d6efd;
    color:white;
    padding:20px;
    text-align:center;
    font-size:28px;
    font-weight:bold;
}

.container{
    max-width:1200px;
    margin:30px auto;
    padding:20px;
}

.upload-area{
    background:white;
    border-radius:15px;
    padding:30px;
    text-align:center;
    box-shadow:0 2px 10px rgba(0,0,0,.1);
}

.upload-btn{
    margin-top:15px;
}

input[type=file]{
    padding:10px;
}

.grid{
    margin-top:30px;
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:20px;
}

.card{
    background:white;
    border-radius:15px;
    padding:20px;
    box-shadow:0 2px 10px rgba(0,0,0,.1);
}

.card h2{
    margin-bottom:15px;
}

.preview{
    width:100%;
    height:500px;
    object-fit:contain;
    border:1px solid #ddd;
}

.result{
    text-align:center;
}

.status{
    font-size:32px;
    font-weight:bold;
    margin-top:40px;
}

.fracture{
    color:#dc3545;
}

.normal{
    color:#198754;
}

.score{
    font-size:22px;
    margin-top:20px;
}

.btn{
    margin-top:25px;
    padding:12px 25px;
    border:none;
    background:#0d6efd;
    color:white;
    border-radius:8px;
    cursor:pointer;
}

.btn:hover{
    background:#0b5ed7;
}

.progress{
    width:100%;
    height:25px;
    background:#eee;
    border-radius:30px;
    overflow:hidden;
    margin-top:20px;
}

.bar{
    height:100%;
    width:0%;
    background:#dc3545;
    transition:1s;
}

.footer{
    text-align:center;
    margin-top:40px;
    color:#888;
}

@media(max-width:768px){
.grid{
grid-template-columns:1fr;
}
}
</style>
</head>

<body>

<header>
Bone Fracture Detection System
</header>

<div class="container">

<div class="upload-area">
<h2>Upload X-Ray Image</h2>
<input type="file" id="fileInput">
</div>

<div class="grid">

<div class="card">

<h2>X-Ray Image</h2>

<img id="preview"
class="preview"
src="fracture.jpg">

</div>

<div class="card result">

<h2>Analysis Result</h2>

<div id="status" class="status">
Waiting...
</div>

<div class="score">
Confidence:
<span id="confidence">0%</span>
</div>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<button class="btn" onclick="analyze()">
Analyze Image
</button>

</div>

</div>

<div class="footer">
AI Bone Fracture Detection Demo
</div>

</div>

<script>

const fileInput =
document.getElementById('fileInput');

const preview =
document.getElementById('preview');

fileInput.addEventListener('change', e => {

const file = e.target.files[0];

if(file){
preview.src =
URL.createObjectURL(file);
}

});

function analyze(){

let probability = 97.8;

document.getElementById('status')
.innerHTML = 'FRACTURE DETECTED';

document.getElementById('status')
.className = 'status fracture';

document.getElementById('confidence')
.innerHTML = probability + '%';

document.getElementById('bar')
.style.width = probability + '%';

}

</script>

</body>
</html>
