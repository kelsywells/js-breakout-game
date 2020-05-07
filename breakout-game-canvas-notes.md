var canvas = document.getElementById("myCanvas");
var ctx = canvas.getContext("2d");

// bring red square to canvas:

// beginPath method starts drawing a line. closePath method ends the line.
// rect() specifies a rectangle.
// rect() first 2 numbers 20, 40 = coordinates of the top left corner onto the canvas.
// rect() last 2 numbers 50, 50 = indicate height & width of the rectangle
ctx.beginPath();
ctx.rect(20, 40, 50, 50);
ctx.fillStyle = "#FF0000";
ctx.fill();
ctx.closePath();


// bring green circle to canvas:
// arc() specifies circle drawing & takes 5-6 parameters:
//// x & y coordinates for arc center (240, 160)
//// arc radius (20)
//// angles to start and finish drawing in radians (0, Math.PI)
//// direction of drawing(optional): false= clockwise, true(default)=counter-clockwise
ctx.beginPath();
ctx.arc(240, 160, 20, 0, Math.PI * 2, false);
ctx.fillStyle = "green";
ctx.fill();
ctx.closePath();


// strokeStyle= outline, no color fill:
ctx.beginPath();
ctx.rect(160, 10, 100, 40);
ctx.strokeStyle = "rgba(0, 0, 255, 0.5)";
ctx.stroke();
ctx.closePath();

// draw function will be called every 10ms forever because setInterval is infinite in nature
// once ball is moving, ctx code will ensure ball is now repainted on every frame

function draw() {
ctx.beginPath();
ctx.arc(50, 50, 10, 0, Math.PI * 2);
ctx.fillStyle = "#0095DD";
ctx.fill();
ctx.closePath();
}
setInterval(draw, 10);

// instead of specific position at x,y coords on arc, define starting point with var x, var y (see below)
var x= canvas.width/2;
var y=canvas.height-30;
// define x & y above draw() function, and replace (50, 50) arc coords with x, y

// add small value to x & y after every frame has been drawn to make the ball appear moving. Define values as dx & dy:
var dx = 2;
var dy = -2;

// update x & y with dx & dy variable on every frame x+=dx, y+=dy. Ball will move but leave a trail because a new circle
is added to every frame
function draw() {
ctx.beginPath();
ctx.arc(x, y, 10, 0, Math.PI * 2);
ctx.fillStyle = "#0095DD";
ctx.fill();
ctx.closePath();
x += dx;
y += dy;
}
setInterval(draw, 10);

// clearRect() will clear canvas content & takes 4 parameters: x,y coords at top left of rect, and x,y coords at bottom
// right of rect. This specified area will be cleared of any content previously painted there.
function draw() {
ctx.clearRect(0, 0, canvas.width, canvas.height);
ctx.beginPath();
ctx.arc(x, y, 10, 0, Math.PI * 2);
ctx.fillStyle = "#0095DD";
ctx.fill();
ctx.closePath();
x += dx;
y += dy;
}
setInterval(draw, 10);

// The ball will now move without a trail. Every 10 milliseconds the canvas is cleared, the ball will be drawn on a
// given position and the x and y values will be updated for the next frame.


//CLEAN UP CODE:
//replace existing draw() function with the following 2 functions:

function drawBall() {
ctx.beginPath();
ctx.arc(x, y, 10, 0, Math.PI * 2);
ctx.fillStyle = "#0095DD";
ctx.fill();
ctx.closePath();
}

function draw() {
ctx.clearRect(0, 0, canvas.width, canvas.height);
drawBall();
x += dx;
y += dy;
}

setInterval(draw, 10);

// Collision detection: if the ball touches the wall, we will change its direction. ballRadius will hold the radius of
the circle and be used for
// calculations. var ballRadius = 10;
//There are 4 walls to bounce off. Each must be programmed to bounce the ball a different direction.

// top wall:
// if the y value of the balls position (y+dy) is < 0, change the y-axis movement direction by making it equal to
    itself, reversed (dy=-dy):

    if(y + dy < 0) { 
        dy=-dy; 
     }

    // bottom wall: 
    // canvas height: y values are counted from top left (0) to bottom right (height size=320) so canvas.height refers to the bottom wall:
    
    if (y + dy> canvas.height) {
    dy = -dy;
    }

// MERGE the two statements with || : 

if (y + dy > canvas.height || y + dy < 0) {
    dy = -dy;
    }

// Do the same thing with x and width to accomplish the sides: 
if(x + dx > canvas.width || x + dx < 0) {
    dx = -dx;
}

//The ball should be bouncing off the walls, but sinking into them before bouncing. This is because the ball's radius is what's hitting the wall, not the 
//circumference. To fix this, subtract ballRadius from the width & height, and change 0 to ballRadius

//"When the distance between the center of the ball and the edge of the wall is exactly the same as the radius of the ball, it will change the movement direction. Subtracting the radius from one edge's width and adding it onto the other gives us the impression of the proper collision detection — the ball bounces off the walls as it should do."


//Draw the paddle:

//declare paddle variables:
var paddleHeight = 10;
var paddleWidth = 75;
var paddleX = (canvas.width-paddleWidth) / 2;

//write function to draw the paddle with the vars just like the ball: 
function drawPaddle() {
    ctx.beginPath();
    ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
    ctx.fillStyle = "#0095DD";
    ctx.fill();
    ctx.closePath();
}

//call drawPaddle() in draw() function 


//CONTROL THE PADDLE: 
// define rightPressed and leftPressed variables as false (they are not pressed on load): var rightPressed = false; var leftPressed = false;
//set up event listeners to listen for key press: 
    //document.addEventListener("keydown", keyDownHandler, false);
    //document.addEventListener("keyup", keyUpHandler, false);

//set keyUpHandler and keyDownHandler: 
        function keyDownHandler(e) {
            if (e.key == "Right" || e.key == "ArrowRight") {
                rightPressed = true;
            }
            else if (e.key == "Left" || e.key == "ArrowLeft") {
                leftPressed = true;
            }
        }

        function keyUpHandler(e) {
            if (e.key == "Right" || e.key == "ArrowRight") {
                rightPressed = false;
            }
            else if (e.key == "Left" || e.key == "ArrowLeft") {
                leftPressed = false;
            }
        }

// paddle LOGIC: 
// In draw() function: if right is pressed, move 5 spaces right. If left is pressed, move 5 spaces left:
//         if(rightPressed) {paddleX += 5;} else if(leftPressed) {paddleX -= 5;}

// paddle will fly off the screen when the buttons are held. Change code to stay within the boundaries: 
        // if (rightPressed) paddle += 5; if (paddleX + paddleWidth > canvas.width) { paddleX= canvas.width - paddleWidth}; } 
        // else if (leftPressed) {paddleX -= 5; if (paddleX < 0) { paddleX = 0; }

//Instead of letting the ball bounce off all 4 edges, end the game when the ball hits the bottom, by adding an alert and removing the original bounce statement:

            if (y + dy < ballRadius) {
                dy = -dy;
            } else if (y + dy > canvas.height - ballRadius) {
                alert("YOU HAVE DIED");
                document.location.reload();
                clearInterval(interval); // Needed for Chrome to end game
            }

//Let the paddle hit the ball back by updating the same statement again: 

        if(y + dy < ballRadius) {
            dy = -dy;
        } else if(y + dy > canvas.height-ballRadius) {
            if(x > paddleX && x < paddleX + paddleWidth) {
                dy = -dy;
            }
            else {
                alert("YOU HAVE DIED");
                document.location.reload();
                clearInterval(interval);
            }
        }
    