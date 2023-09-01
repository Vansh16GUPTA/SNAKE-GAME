# SNAKE-GAME
This is my snake game created using HTML, CSS &amp; JAVASCRIPT. 
<br>
Author-Vansh Gupta
<br>
This is html code for my project
<br>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div class="body">
        <div id="scorebox">Score:0</div>
        <div id="hiscorebox">Hi Score:0</div>
        <div id="board"></div>
    </div>
</body>
<script src="js/index.js"></script>
</html>
<br>
This is my CSS code for designing of code.
<br>
*{
    padding: 0;
    margin: 0;
}

.body{
    background: url("../bg.jpg");
    min-height: 100vh;
    background-size: 100vw 100vh;
    background-repeat: no-repeat;
    display: flex;
    justify-content: center;
    align-items: center;
}

#board{
    background: linear-gradient(rgb(140, 190, 140),rgb(194, 194, 127));
    width: 90vmin;
    height: 95vmin;
    border: 2px solid black;
    display: grid;
    grid-template-columns:repeat(18,1fr) ;
    grid-template-rows: repeat(18,1fr);
}

#scorebox{
    position: absolute;
    top: 9px;
    right: 160px;
    font-size: 39px;
    font-weight: bold;
    font-style: italic;
}

#hiscorebox{
    position: absolute;
    top:45px;
    right: 140px;
    font-size: 39px;
    font-weight: bold;
}

.head{
    background-color: red;
}
.snake{
    background-color: purple;
    border: .25vmin solid white;
    border-radius: 12px;
}

.food{
    background: linear-gradient(red,purple);
    border: .25vmin solid black;
    border-radius: 8px;
}
<br>
This is my main javascript code for my project.It contains all my logic behind the code.
<br>
// Game Functions and Variables
let inputDir={x:0,y:0};
const foodsound=new Audio('Eating.mp3');
const gameOverSound=new Audio('GameOver.mp3');
const moveSound=new Audio('Direction.mp3');
const musicSound=new Audio('background.mp3');
let speed=30;
let score=0;
let lastPaintTime=0;
let snakeArr=[
    {x:13,y:15}
];
food={x:6,y:7};


// Game Functions   
function main(ctime){
    window.requestAnimationFrame(main);
    // console.log(ctime);
    if((ctime-lastPaintTime)/1000 < 1/speed){
        return;
    }
    lastPaintTime=ctime;
    gameEngine();
}

function isCollide(sarr){
    // if you bump into yourself
    for (let i=1;i<snakeArr.length;i++) {
        if(snakeArr[i].x===snakeArr[0].x && snakeArr[i].y===snakeArr[0].y){
            return true;
        }
    }    
    // if you bump into wall
    if(snakeArr[0].x>=18 ||snakeArr[0].x<=0 && snakeArr[0].y>=18 ||snakeArr[0].y<=0 ){
        return true;
    }
}
function gameEngine(){
    // Part1: Updating the snake array and food;
        if(isCollide(snakeArr)){
            gameOverSound.play();
            musicSound.pause();
            inputDir={x:0,y:0};
            alert("Game over.Press any key to play again!");
            snakeArr=[{x:13,y:15}];
            musicSound.play();
            score=0;
        }

        //if you have eaten the food,increment  the score and regenerate the food
        if(snakeArr[0].y===food.y && snakeArr[0].x===food.x){
            foodsound.play();
            score+=1;
            if(score>hiscoreval){
                hiscoreval=score;
                localStorage.setItem("hiscore",JSON.stringify(hiscoreval));
                hiscorebox.innerHTML="Hiscore:" + hiscoreval;
            }
            scorebox.innerHTML="score: " + score;
            snakeArr.unshift({x:snakeArr[0].x+inputDir.x,y:snakeArr[0].y+inputDir.y});
            let a=2;
            let b=16;
            food={x:Math.round(a+(b-a)*Math.random()),y:Math.round(a+(b-a)*Math.random())}
        }


        // Moving the snake
        for (let i=snakeArr.length-2;i>=0;i--){
            snakeArr[i+1]={...snakeArr[i]};
        }
        
        snakeArr[0].x += inputDir.x;
        snakeArr[0].y += inputDir.y;

    // Part 2: Display  the snake and food;
    // Display  the snake
    board.innerHTML="";
    snakeArr.forEach((e,index)=>{
        snakeElement=document.createElement('div');
        snakeElement.style.gridRowStart=e.y;
        snakeElement.style.gridColumnStart=e.x;
        if(index==0){
            snakeElement.classList.add('head');
        }
        else{
            snakeElement.classList.add('snake');
        } 
        board.appendChild(snakeElement);
    });

    // Display the food
    foodElement=document.createElement('div');
        foodElement.style.gridRowStart=food.y;
        foodElement.style.gridColumnStart=food.x;
        foodElement.classList.add('food');
        board.appendChild(foodElement);
}


// Main Logic starts here

let hiscore=localStorage.getItem("hiscore");
if(hiscore===null){
    hiscoreval=0;
    localStorage.setItem("hiscore",JSON.stringify(hiscoreval));
}
else{
    hiscoreval=JSON.parse(hiscore);
    hiscorebox.innerHTML="Hiscore:" + hiscore;
}
window.requestAnimationFrame(main);
window.addEventListener('keydown', e =>{
    inputDir={x: 0,y: 1} //start the game 
    moveSound.play();
    switch(e.key){
        case "w":
            console.log("w");
            inputDir.x=0;
            inputDir.y=-1;
            break;

        case "s":
            console.log("s");
            inputDir.x=0;
            inputDir.y=1;
            break; 
                
        case "a":
            console.log("a")
            inputDir.x=-1;
            inputDir.y=0;
            break;    

        case "d":
            console.log("d")
            inputDir.x=1;
            inputDir.y=0;
            break;

        default:
            break;
    }
});


