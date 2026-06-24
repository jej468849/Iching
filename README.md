# Iching
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>매화역수 작괘기</title>

<style>

body{
    font-family: "Malgun Gothic", sans-serif;
    background:#f5f5f5;
    margin:0;
    padding:20px;
}

.container{
    max-width:500px;
    margin:auto;
    background:white;
    padding:25px;
    border-radius:15px;
    box-shadow:0 3px 15px rgba(0,0,0,0.1);
}

h1{
    text-align:center;
    margin-bottom:20px;
}

label{
    display:block;
    margin-top:15px;
    margin-bottom:5px;
    font-weight:bold;
}

input{
    width:100%;
    padding:12px;
    font-size:18px;
    box-sizing:border-box;
}

button{
    width:100%;
    padding:15px;
    margin-top:15px;
    font-size:18px;
    border:none;
    border-radius:10px;
    cursor:pointer;
}

.calcBtn{
    background:#d97706;
    color:white;
}

.nowBtn{
    background:#666;
    color:white;
}

.result{
    margin-top:25px;
    padding:20px;
    background:#fafafa;
    border-radius:10px;
    line-height:1.8;
}

.big{
    font-size:22px;
    font-weight:bold;
}

</style>
</head>
<body>

<div class="container">

<h1>매화역수 작괘기</h1>

<label>날짜</label>
<input type="date" id="date">

<label>시간</label>
<input type="time" id="time">

<button class="nowBtn" onclick="setNow()">
현재시간 불러오기
</button>

<button class="calcBtn" onclick="calculate()">
작괘하기
</button>

<div class="result" id="result">

날짜와 시간을 입력하세요.

</div>

</div>

<script>

const trigramNames = {
    1:"건 ☰",
    2:"태 ☱",
    3:"리 ☲",
    4:"진 ☳",
    5:"손 ☴",
    6:"감 ☵",
    7:"간 ☶",
    8:"곤 ☷"
};

function setNow(){

    const now = new Date();

    const year = now.getFullYear();

    const month =
        String(now.getMonth()+1).padStart(2,"0");

    const day =
        String(now.getDate()).padStart(2,"0");

    const hour =
        String(now.getHours()).padStart(2,"0");

    const minute =
        String(now.getMinutes()).padStart(2,"0");

    document.getElementById("date").value =
        `${year}-${month}-${day}`;

    document.getElementById("time").value =
        `${hour}:${minute}`;
}

function getHourNumber(hour){

    if(hour>=23 || hour<1) return 1;
    if(hour<3) return 2;
    if(hour<5) return 3;
    if(hour<7) return 4;
    if(hour<9) return 5;
    if(hour<11) return 6;
    if(hour<13) return 7;
    if(hour<15) return 8;
    if(hour<17) return 9;
    if(hour<19) return 10;
    if(hour<21) return 11;

    return 12;
}

function getYearBranch(year,month,day){

    let adjustedYear = year;

    // 입춘 전이면 전년도 적용
    if(month < 2 || (month === 2 && day < 4)){
        adjustedYear--;
    }

    return ((adjustedYear - 4) % 12) + 1;
}

function calculate(){

    const dateValue =
        document.getElementById("date").value;

    const timeValue =
        document.getElementById("time").value;

    if(!dateValue || !timeValue){

        alert("날짜와 시간을 입력하세요.");

        return;
    }

    const date = new Date(dateValue);

    const yearNum = getYearBranch(
        date.getFullYear(),
        date.getMonth()+1,
        date.getDate()
    );

    const monthNum =
        date.getMonth()+1;

    const dayNum =
        date.getDate();

    const hour =
        parseInt(timeValue.split(":")[0]);

    const hourNum =
        getHourNumber(hour);

    let upper =
        (yearNum + monthNum + dayNum) % 8;

    if(upper===0) upper=8;

    let lower =
        (yearNum + monthNum + dayNum + hourNum) % 8;

    if(lower===0) lower=8;

    let moving =
        (yearNum + monthNum + dayNum + hourNum) % 6;

    if(moving===0) moving=6;

    document.getElementById("result").innerHTML = `

    <div class="big">
    상괘 : ${trigramNames[upper]}
    </div>

    <div class="big">
    하괘 : ${trigramNames[lower]}
    </div>

    <div class="big">
    동효 : ${moving}효
    </div>

    <hr>

    연수 : ${yearNum}<br>
    월수 : ${monthNum}<br>
    일수 : ${dayNum}<br>
    시수 : ${hourNum}

    `;
}

setNow();

</script>

</body>
</html>
