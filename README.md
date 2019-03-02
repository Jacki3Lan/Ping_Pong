# Ping_Pong
Interactive table tennis game.
var canvas;
var canvasContext;

// Pong Ball
var ballX = 50;
var ballY = 50;
var ballSpeedX = 15;
var ballSpeedY = 15;


// Player paddles
var paddle1Y = 250;
var paddle2Y = 250;
const paddle_height = 100;
const paddle_thickness = 10;

//score
var player1Score = 0;
var player2Score = 0;
var gameOver = 4;

function calcMousePos(evt) {
	var rect = canvas.getBoundingClientRect();
	var root = document.documentElement;
	var mouseX = evt.clientX - rect.left - root.scrollLeft;
	var mouseY = evt.clientY - rect.top - root.scrollTop;
	return {
       x:mouseX,
       y:mouseY
	};
}
// Runs function after window has loaded
window.onload = function() {

	canvas = document.getElementById('gameCanvas');
	canvasContext = canvas.getContext('2d');
	var framesPerSecond = 30;
	// Animates drawings by calling functions in intervals
	setInterval(function() {
		moveEverything();
		drawEverything();
	}, 1000/framesPerSecond);

	canvas.addEventListener('mousemove', function(evt) {
 var mousePos = calcMousePos(evt);
paddle1Y = mousePos.y - paddle_height/2;

});
  }
 // ball collision result
  function ballReset() {
canvasContext.fillText(gameOver,375, 200);
ballSpeedX = 0;
ballSpeedY = 0;
ballX = canvas.width/2;
ballY = canvas.height/2;

}

// player 2 paddle AI
function computerMovement(){
	var paddle2YCenter = paddle2Y + (paddle_height/2);
	if (paddle2YCenter < ballY -35) {
		paddle2Y += 6;
	} else if (paddle2YCenter > ballY +35){
         paddle2Y -= 6;
	}
}
// Ball/Paddle logic and conditionals
function moveEverything() {
	computerMovement();
	ballX = ballX + ballSpeedX;
	ballY = ballY + ballSpeedY;

	if(ballX < 0) { 
		if (ballY > paddle1Y && ballY < paddle1Y + paddle_height + 3){
		ballSpeedX = -ballSpeedX;
	} else { ballReset();
		     player2Score++
      }
	}

	if(ballX > canvas.width){
	if (ballY > paddle2Y && ballY < paddle2Y + paddle_height + 3){
		ballSpeedX = -ballSpeedX;
	} else { ballReset();
		     player1Score++
      }
	}

	
	 if(ballY > canvas.height) {
    	
    	ballSpeedY = -ballSpeedY;
	}
	 if(ballY < 0){
	 	ballSpeedY = -ballSpeedY;
	  }
	}


function drawEverything() {
   
    // Background
	colorRect(0,0,canvas.width,canvas.height, 'black');
	// Player 1
	colorRect(0,paddle1Y,paddle_thickness,paddle_height, 'white');
	// Player 2
	colorRect(canvas.width-paddle_thickness,paddle2Y,paddle_thickness,paddle_height, 'white');
	// Pong Ball
	colorCircle(ballX, ballY, 10, 'white');

	canvasContext.fillText(player1Score, 100, 100);
	canvasContext.fillText(player2Score, canvas.width -100, 100);
}
// to create Circles
function colorCircle(centerX, centerY, radius, drawColor) {
	canvasContext.fillStyle = drawColor;
	canvasContext.beginPath();
	canvasContext.arc(centerX, centerY, radius, 0,Math.PI * 2, false);
	canvasContext.fill();
}
// to create Rectangles
function colorRect(leftX,topY, width,height, drawColor){
     canvasContext.fillStyle = drawColor;
     canvasContext.fillRect(leftX,topY, width,height);
}

