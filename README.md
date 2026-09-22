<!DOCTYPE html>
<html lang="az">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEON RUSH - Racing Simulator</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

html,body{
    width:100%;
    height:100%;
    overflow:hidden;
    background:#05070b;
    font-family:Arial,sans-serif;
    color:white;
}

canvas{
    display:block;
}

#hud{
    position:fixed;
    inset:0;
    pointer-events:none;
}

.top{
    position:absolute;
    top:20px;
    left:20px;
    right:20px;
    display:flex;
    justify-content:space-between;
    align-items:flex-start;
}

.panel{
    background:rgba(4,8,15,.84);
    border:1px solid rgba(0,220,255,.35);
    border-radius:14px;
    padding:13px 17px;
    backdrop-filter:blur(10px);
    box-shadow:0 0 25px rgba(0,220,255,.12);
}

.logo{
    color:#00eaff;
    font-size:23px;
    font-weight:900;
    letter-spacing:4px;
    text-shadow:0 0 15px #00eaff;
}

.sub{
    color:#71808a;
    font-size:9px;
    letter-spacing:3px;
    margin-top:4px;
}

.stats{
    display:flex;
    gap:10px;
}

.stat{
    min-width:90px;
    text-align:center;
}

.stat small{
    display:block;
    color:#697781;
    font-size:9px;
    letter-spacing:2px;
}

.stat strong{
    display:block;
    margin-top:5px;
    font-size:18px;
}

#speed{
    color:#00eaff;
}

#position{
    color:#ffd43b;
}

#lap{
    color:#ff4da6;
}

.bottom{
    position:absolute;
    bottom:20px;
    left:20px;
    right:20px;
    display:flex;
    justify-content:space-between;
    align-items:flex-end;
}

.nitroBox{
    width:260px;
}

.nitroTitle{
    display:flex;
    justify-content:space-between;
    color:#89959e;
    font-size:10px;
    margin-bottom:7px;
}

.bar{
    height:12px;
    background:#10161d;
    border-radius:20px;
    overflow:hidden;
}

#nitroBar{
    width:100%;
    height:100%;
    background:linear-gradient(90deg,#0077ff,#00eaff);
    box-shadow:0 0 15px #00eaff;
}

.controls{
    font-size:10px;
    color:#77838c;
}

.controls b{
    color:white;
}

#startScreen,
#finishScreen{
    position:fixed;
    inset:0;
    z-index:20;
    display:flex;
    justify-content:center;
    align-items:center;
    background:
        radial-gradient(circle,rgba(0,200,255,.16),transparent 40%),
        #03050a;
}

#finishScreen{
    display:none;
}

.menu{
    width:min(650px,90%);
    text-align:center;
    padding:50px 35px;
    background:rgba(6,10,17,.95);
    border:1px solid rgba(0,220,255,.35);
    border-radius:25px;
    box-shadow:0 0 80px rgba(0,220,255,.15);
}

.menu h1{
    color:#00eaff;
    font-size:clamp(45px,8vw,85px);
    letter-spacing:8px;
    text-shadow:0 0 35px #00eaff;
}

.tag{
    margin-top:8px;
    color:#71808a;
    letter-spacing:5px;
    font-size:11px;
}

button{
    margin-top:35px;
    padding:16px 45px;
    background:rgba(0,220,255,.08);
    color:#00eaff;
    border:1px solid #00eaff;
    border-radius:10px;
    font-weight:bold;
    letter-spacing:3px;
    cursor:pointer;
}

button:hover{
    background:#00eaff;
    color:#001016;
    box-shadow:0 0 30px #00eaff;
}

.help{
    margin-top:25px;
    color:#68757e;
    font-size:11px;
    line-height:2;
}

.result{
    margin-top:25px;
    color:#9ca8b0;
    line-height:2;
}

.result strong{
    color:#00eaff;
    font-size:25px;
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">

    <div class="top">

        <div class="panel">
            <div class="logo">NEON RUSH</div>
            <div class="sub">EXTREME CITY RACING</div>
        </div>

        <div class="stats">

            <div class="panel stat">
                <small>SPEED</small>
                <strong id="speed">0 KM/H</strong>
            </div>

            <div class="panel stat">
                <small>POSITION</small>
                <strong id="position">1 / 6</strong>
            </div>

            <div class="panel stat">
                <small>LAP</small>
                <strong id="lap">1 / 3</strong>
            </div>

            <div class="panel stat">
                <small>TIME</small>
                <strong id="time">00:00</strong>
            </div>

        </div>
    </div>

    <div class="bottom">

        <div class="panel nitroBox">

            <div class="nitroTitle">
                <span>NITRO</span>
                <span id="nitroText">100%</span>
            </div>

            <div class="bar">
                <div id="nitroBar"></div>
            </div>

        </div>

        <div class="panel controls">
            <b>W / ↑</b> Qaz
            &nbsp;&nbsp;
            <b>S / ↓</b> Əyləc
            &nbsp;&nbsp;
            <b>A / D</b> Sükan
            &nbsp;&nbsp;
            <b>SPACE</b> Nitro
            &nbsp;&nbsp;
            <b>R</b> Restart
        </div>

    </div>

</div>


<div id="startScreen">

    <div class="menu">

        <h1>NEON RUSH</h1>

        <div class="tag">
            EXTREME RACING
        </div>

        <button id="startBtn">
            START RACE
        </button>

        <div class="help">
            W / ↑ — Qaz<br>
            S / ↓ — Əyləc<br>
            A / D — Sükan<br>
            SPACE — Nitro<br>
            R — Restart
        </div>

    </div>

</div>


<div id="finishScreen">

    <div class="menu">

        <h1 id="finishTitle">FINISH</h1>

        <div class="result" id="resultText"></div>

        <button id="restartBtn">
            RACE AGAIN
        </button>

    </div>

</div>


<script>

const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

let W,H;

function resize(){

    W=canvas.width=window.innerWidth;
    H=canvas.height=window.innerHeight;

}

window.addEventListener("resize",resize);
resize();


/* =====================================
   KEYBOARD
===================================== */

const keys={};

window.addEventListener("keydown",e=>{

    keys[e.code]=true;

    if(
        ["ArrowUp","ArrowDown","ArrowLeft",
        "ArrowRight","Space"].includes(e.code)
    ){
        e.preventDefault();
    }

    if(e.code==="KeyR" && gameRunning){

        resetRace();

    }

});

window.addEventListener("keyup",e=>{

    keys[e.code]=false;

});


/* =====================================
   TRACK
===================================== */

const track=[];

function buildTrack(){

    const points=[

        {x:0,y:0},
        {x:800,y:0},
        {x:1450,y:300},
        {x:1600,y:900},
        {x:1350,y:1500},
        {x:600,y:1750},
        {x:-350,y:1650},
        {x:-1050,y:1050},
        {x:-1250,y:300},
        {x:-950,y:-400},
        {x:-300,y:-700},
        {x:500,y:-650}

    ];

    for(let i=0;i<points.length;i++){

        const a=points[i];
        const b=points[(i+1)%points.length];

        const distance=Math.hypot(
            b.x-a.x,
            b.y-a.y
        );

        const steps=Math.ceil(distance/50);

        for(let j=0;j<steps;j++){

            const t=j/steps;

            track.push({

                x:a.x+(b.x-a.x)*t,
                y:a.y+(b.y-a.y)*t

            });

        }

    }

}

buildTrack();


/* =====================================
   TRACK HELPERS
===================================== */

function point(i){

    i=Math.floor(i);

    i=((i%track.length)+track.length)%track.length;

    return track[i];

}


function nearest(x,y){

    let index=0;
    let distance=Infinity;

    for(let i=0;i<track.length;i++){

        const p=track[i];

        const dx=x-p.x;
        const dy=y-p.y;

        const d=dx*dx+dy*dy;

        if(d<distance){

            distance=d;
            index=i;

        }

    }

    return {

        index:index,
        distance:Math.sqrt(distance)

    };

}


/* =====================================
   GAME VARIABLES
===================================== */

let gameRunning=false;
let raceFinished=false;
let raceTime=0;

const TOTAL_LAPS=3;


/* =====================================
   PLAYER
===================================== */

const player={

    x:0,
    y:0,

    angle:0,

    speed:0,

    maxSpeed:9,

    nitro:100,

    lap:1,

    progress:0,

    previousProgress:0,

    hasGoneFarEnough:false

};


/* =====================================
   OPPONENTS
===================================== */

const opponentColors=[
    "#ff315f",
    "#ffd000",
    "#9d5cff",
    "#00ff9d",
    "#ff7900"
];

const opponents=[];


/*
   IMPORTANT:
   AI SPEED IS NOW TIME BASED.

   60 FPS -> same speed
   144 FPS -> same speed
   240 FPS -> same speed
*/

function createOpponents(){

    opponents.length=0;

    for(let i=0;i<5;i++){

        const startIndex=
            track.length-35-i*24;

        const p=point(startIndex);
        const n=point(startIndex+3);

        opponents.push({

            progress:startIndex,

            lap:1,

            /*
              Normal AI speed.
              Player maximum normal speed = 9.
              AI = 6.3 - 7.1.
            */

            speed:
                6.2+
                Math.random()*0.7,

            x:p.x,
            y:p.y,

            angle:Math.atan2(
                n.y-p.y,
                n.x-p.x
            ),

            color:opponentColors[i],

            finished:false

        });

    }

}


/* =====================================
   RESET
===================================== */

function resetRace(){

    const start=point(0);
    const next=point(4);

    player.x=start.x;
    player.y=start.y+45;

    player.angle=Math.atan2(
        next.y-start.y,
        next.x-start.x
    );

    player.speed=0;

    player.nitro=100;

    player.lap=1;

    player.progress=0;

    player.previousProgress=0;

    player.hasGoneFarEnough=false;

    raceTime=0;

    raceFinished=false;

    createOpponents();

    gameRunning=true;

    document.getElementById(
        "finishScreen"
    ).style.display="none";

}


/* =====================================
   PLAYER UPDATE
===================================== */

function updatePlayer(dt){

    const up=
        keys["KeyW"]||
        keys["ArrowUp"];

    const down=
        keys["KeyS"]||
        keys["ArrowDown"];

    const left=
        keys["KeyA"]||
        keys["ArrowLeft"];

    const right=
        keys["KeyD"]||
        keys["ArrowRight"];

    const nitro=
        keys["Space"] &&
        player.nitro>0 &&
        player.speed>2;


    /* ACCELERATION */

    if(up){

        player.speed+=
            8.5*dt;

    }else{

        player.speed-=
            2.8*dt;

    }


    /* BRAKE */

    if(down){

        player.speed-=
            12*dt;

    }


    /* NITRO */

    if(nitro){

        player.speed+=
            13*dt;

        player.nitro-=
            28*dt;

    }else{

        player.nitro+=
            2*dt;

    }


    player.nitro=Math.max(
        0,
        Math.min(100,player.nitro)
    );


    const maxSpeed=
        player.maxSpeed+
        (nitro?4:0);


    player.speed=Math.max(
        0,
        Math.min(maxSpeed,player.speed)
    );


    /* STEERING */

    let steer=0;

    if(left)steer=-1;
    if(right)steer=1;

    player.angle+=
        steer*
        2.5*
        dt*
        (0.35+player.speed/10);


    /* MOVE */

    player.x+=
        Math.cos(player.angle)*
        player.speed*
        60*
        dt;

    player.y+=
        Math.sin(player.angle)*
        player.speed*
        60*
        dt;


    /* TRACK */

    const n=nearest(
        player.x,
        player.y
    );

    player.progress=n.index;


    /* OUTSIDE ROAD */

    if(n.distance>120){

        const target=point(n.index);

        const dx=target.x-player.x;
        const dy=target.y-player.y;

        const d=Math.max(
            1,
            Math.hypot(dx,dy)
        );

        player.x+=
            dx/d*
            180*
            dt;

        player.y+=
            dy/d*
            180*
            dt;

        player.speed*=0.97;

    }


    /* =====================================
       LAP DETECTION
    ===================================== */

    const previous=player.previousProgress;
    const current=player.progress;


    /*
      First we make sure the player has travelled
      through at least 30% of the track.
    */

    if(current>track.length*0.30){

        player.hasGoneFarEnough=true;

    }


    /*
      A lap is completed only when the player
      has travelled far enough AND crosses
      from the end of the track to the beginning.
    */

    if(
        player.hasGoneFarEnough &&
        previous>track.length*0.90 &&
        current<track.length*0.10
    ){

        player.lap++;

        player.hasGoneFarEnough=false;

        if(player.lap>TOTAL_LAPS){

            finishRace();

            return;

        }

    }


    player.previousProgress=current;

}


/* =====================================
   AI UPDATE
===================================== */

function updateAI(dt){

    opponents.forEach(car=>{

        if(car.finished)return;


        /*
          SPEED IS MULTIPLIED BY dt.
          Therefore it is independent of FPS.
        */

        car.progress+=
            car.speed*
            60*
            dt;


        if(car.progress>=track.length){

            car.progress-=track.length;

            car.lap++;

            if(car.lap>TOTAL_LAPS){

                car.finished=true;

            }

        }


        const p=point(
            Math.floor(car.progress)
        );

        const n=point(
            Math.floor(car.progress+3)
        );


        car.x=p.x;
        car.y=p.y;


        car.angle=Math.atan2(
            n.y-p.y,
            n.x-p.x
        );

    });

}


/* =====================================
   COLLISION
===================================== */

function collisions(){

    opponents.forEach(car=>{

        const d=Math.hypot(
            player.x-car.x,
            player.y-car.y
        );

        if(d<40){

            player.speed*=0.75;

            const dx=player.x-car.x;
            const dy=player.y-car.y;

            const length=Math.max(
                1,
                Math.hypot(dx,dy)
            );

            player.x+=
                dx/length*8;

            player.y+=
                dy/length*8;

        }

    });

}


/* =====================================
   POSITION
===================================== */

function getPosition(){

    const racers=[{

        progress:
            (player.lap-1)*
            track.length+
            player.progress,

        isPlayer:true

    }];


    opponents.forEach(car=>{

        racers.push({

            progress:
                (car.lap-1)*
                track.length+
                car.progress,

            isPlayer:false

        });

    });


    racers.sort(
        (a,b)=>b.progress-a.progress
    );


    return racers.findIndex(
        r=>r.isPlayer
    )+1;

}


/* =====================================
   CAMERA
===================================== */

const camera={
    x:0,
    y:0,
    zoom:1
};


function updateCamera(){

    camera.x+=
        (player.x-camera.x)*0.08;

    camera.y+=
        (player.y-camera.y)*0.08;

    camera.zoom=
        Math.max(
            0.72,
            1-player.speed*0.018
        );

}


/* =====================================
   DRAW WORLD
===================================== */

function world(){

    ctx.translate(
        W/2-camera.x*camera.zoom,
        H/2-camera.y*camera.zoom
    );

    ctx.scale(
        camera.zoom,
        camera.zoom
    );

}


/* =====================================
   BACKGROUND
===================================== */

function drawBackground(){

    ctx.fillStyle="#07100c";

    ctx.fillRect(
        0,
        0,
        W,
        H
    );


    ctx.save();

    world();

    const grid=160;

    const startX=
        Math.floor(
            (camera.x-W/camera.zoom)/grid
        )*grid;

    const endX=
        camera.x+
        W/camera.zoom;

    const startY=
        Math.floor(
            (camera.y-H/camera.zoom)/grid
        )*grid;

    const endY=
        camera.y+
        H/camera.zoom;


    ctx.strokeStyle=
        "rgba(0,255,180,.035)";

    for(
        let x=startX;
        x<endX;
        x+=grid
    ){

        ctx.beginPath();
        ctx.moveTo(x,startY);
        ctx.lineTo(x,endY);
        ctx.stroke();

    }


    for(
        let y=startY;
        y<endY;
        y+=grid
    ){

        ctx.beginPath();
        ctx.moveTo(startX,y);
        ctx.lineTo(endX,y);
        ctx.stroke();

    }


    ctx.restore();

}


/* =====================================
   TRACK DRAW
===================================== */

function drawTrack(){

    ctx.save();

    world();


    ctx.beginPath();

    track.forEach((p,i)=>{

        if(i===0)
            ctx.moveTo(p.x,p.y);
        else
            ctx.lineTo(p.x,p.y);

    });

    ctx.closePath();


    ctx.lineWidth=190;
    ctx.strokeStyle="rgba(0,0,0,.4)";
    ctx.lineCap="round";
    ctx.lineJoin="round";
    ctx.stroke();


    ctx.lineWidth=175;
    ctx.strokeStyle="#1b2420";
    ctx.stroke();


    ctx.lineWidth=150;
    ctx.strokeStyle="#20252b";
    ctx.stroke();


    ctx.lineWidth=3;
    ctx.strokeStyle="rgba(255,255,255,.15)";
    ctx.setLineDash([25,25]);
    ctx.stroke();
    ctx.setLineDash([]);


    ctx.restore();

}


/* =====================================
   START LINE
===================================== */

function drawStartLine(){

    ctx.save();

    world();

    const p=point(0);
    const n=point(5);

    const angle=Math.atan2(
        n.y-p.y,
        n.x-p.x
    );

    ctx.translate(
        p.x,
        p.y
    );

    ctx.rotate(angle);


    for(let i=-3;i<3;i++){

        ctx.fillStyle=
            i%2===0
            ? "#fff"
            : "#111";

        ctx.fillRect(
            -8,
            i*25,
            20,
            25
        );

    }

    ctx.restore();

}


/* =====================================
   CAR DRAW
===================================== */

function drawCar(
    x,
    y,
    angle,
    color,
    isPlayer
){

    ctx.save();

    ctx.translate(x,y);
    ctx.rotate(angle);


    ctx.fillStyle=
        "rgba(0,0,0,.5)";

    ctx.fillRect(
        -23,
        -11,
        46,
        25
    );


    if(isPlayer){

        ctx.shadowBlur=25;
        ctx.shadowColor="#00eaff";

    }


    ctx.fillStyle=color;

    ctx.beginPath();

    ctx.roundRect(
        -23,
        -12,
        46,
        24,
        7
    );

    ctx.fill();


    ctx.shadowBlur=0;


    /* WINDOWS */

    ctx.fillStyle="#071018";

    ctx.beginPath();

    ctx.roundRect(
        -5,
        -8,
        17,
        16,
        4
    );

    ctx.fill();


    /* HEADLIGHTS */

    ctx.fillStyle="#eaffff";

    ctx.fillRect(
        16,
        -8,
        4,
        5
    );

    ctx.fillRect(
        16,
        3,
        4,
        5
    );


    /* TAIL LIGHTS */

    ctx.fillStyle="#ff203d";

    ctx.fillRect(
        -21,
        -8,
        4,
        5
    );

    ctx.fillRect(
        -21,
        3,
        4,
        5
    );


    /* NITRO */

    if(
        isPlayer &&
        keys["Space"] &&
        player.nitro>0
    ){

        ctx.fillStyle="#00eaff";

        ctx.beginPath();

        ctx.moveTo(-24,-6);
        ctx.lineTo(-50,0);
        ctx.lineTo(-24,6);

        ctx.fill();

    }


    ctx.restore();

}


/* =====================================
   DRAW CARS
===================================== */

function drawCars(){

    ctx.save();

    world();


    opponents.forEach(car=>{

        drawCar(
            car.x,
            car.y,
            car.angle,
            car.color,
            false
        );

    });


    drawCar(
        player.x,
        player.y,
        player.angle,
        "#00eaff",
        true
    );


    ctx.restore();

}


/* =====================================
   MINIMAP
===================================== */

function drawMinimap(){

    const size=170;

    const x=W-size-20;
    const y=105;


    ctx.save();

    ctx.fillStyle=
        "rgba(3,7,12,.88)";

    ctx.strokeStyle=
        "rgba(0,220,255,.4)";

    ctx.lineWidth=1;


    ctx.beginPath();

    ctx.roundRect(
        x,
        y,
        size,
        size,
        12
    );

    ctx.fill();
    ctx.stroke();


    let minX=Infinity;
    let maxX=-Infinity;

    let minY=Infinity;
    let maxY=-Infinity;


    track.forEach(p=>{

        minX=Math.min(minX,p.x);
        maxX=Math.max(maxX,p.x);

        minY=Math.min(minY,p.y);
        maxY=Math.max(maxY,p.y);

    });


    function map(p){

        return{

            x:x+15+
                (p.x-minX)/
                (maxX-minX)*
                (size-30),

            y:y+15+
                (p.y-minY)/
                (maxY-minY)*
                (size-30)

        };

    }


    ctx.beginPath();

    track.forEach((p,i)=>{

        const q=map(p);

        if(i===0)
            ctx.moveTo(q.x,q.y);
        else
            ctx.lineTo(q.x,q.y);

    });

    ctx.closePath();


    ctx.lineWidth=11;
    ctx.strokeStyle="#343a40";
    ctx.stroke();


    ctx.lineWidth=2;
    ctx.strokeStyle="#00eaff";
    ctx.stroke();


    opponents.forEach(car=>{

        const q=map(car);

        ctx.fillStyle=car.color;

        ctx.beginPath();

        ctx.arc(
            q.x,
            q.y,
            3,
            0,
            Math.PI*2
        );

        ctx.fill();

    });


    const q=map(player);

    ctx.fillStyle="#fff";

    ctx.beginPath();

    ctx.arc(
        q.x,
        q.y,
        4,
        0,
        Math.PI*2
    );

    ctx.fill();


    ctx.restore();

}


/* =====================================
   HUD
===================================== */

function formatTime(t){

    const m=Math.floor(t/60);
    const s=Math.floor(t%60);

    return(
        String(m).padStart(2,"0")+
        ":"+
        String(s).padStart(2,"0")
    );

}


function updateHUD(){

    document.getElementById("speed")
        .textContent=
        Math.round(
            player.speed*22
        )+
        " KM/H";


    document.getElementById("position")
        .textContent=
        getPosition()+
        " / 6";


    document.getElementById("lap")
        .textContent=
        Math.min(
            player.lap,
            TOTAL_LAPS
        )+
        " / "+
        TOTAL_LAPS;


    document.getElementById("time")
        .textContent=
        formatTime(raceTime);


    document.getElementById("nitroBar")
        .style.width=
        player.nitro+"%";


    document.getElementById("nitroText")
        .textContent=
        Math.round(player.nitro)+
        "%";

}


/* =====================================
   FINISH
===================================== */

function finishRace(){

    if(raceFinished)return;

    raceFinished=true;
    gameRunning=false;

    const pos=getPosition();


    document.getElementById(
        "finishTitle"
    ).textContent=
        pos===1
        ? "YOU WIN!"
        : "FINISH!";


    document.getElementById(
        "resultText"
    ).innerHTML=
        "Yekun mövqe<br>"+
        "<strong>"+
        pos+
        " / 6"+
        "</strong><br><br>"+
        "Yarış vaxtı<br>"+
        "<strong>"+
        formatTime(raceTime)+
        "</strong>";


    document.getElementById(
        "finishScreen"
    ).style.display="flex";

}


/* =====================================
   GAME LOOP
===================================== */

let last=performance.now();

function loop(now){

    const dt=Math.min(
        0.05,
        (now-last)/1000
    );

    last=now;


    if(gameRunning){

        raceTime+=dt;

        updatePlayer(dt);

        updateAI(dt);

        collisions();

        updateCamera();

        updateHUD();

    }


    ctx.clearRect(
        0,
        0,
        W,
        H
    );


    drawBackground();

    drawTrack();

    drawStartLine();

    drawCars();

    drawMinimap();


    requestAnimationFrame(loop);

}


/* =====================================
   BUTTONS
===================================== */

document.getElementById("startBtn")
.addEventListener("click",()=>{

    document.getElementById(
        "startScreen"
    ).style.display="none";

    resetRace();

});


document.getElementById("restartBtn")
.addEventListener("click",()=>{

    resetRace();

});


/* =====================================
   INITIAL
===================================== */

const first=point(0);

player.x=first.x;
player.y=first.y+45;

camera.x=player.x;
camera.y=player.y;

createOpponents();

updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>
