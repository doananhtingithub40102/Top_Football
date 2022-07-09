# Top_Football

Top Football - Game bóng đá sử dụng canvas để tạo game

## Các ngôn ngữ, công nghệ
HTML, CSS, Javascript

## Các hàm/phương thức được sử dụng để tạo game

### Tạo 1 khu vực game

```javascript
function startGame() {
  myGameArea.start();
}

var myGameArea = {
  canvas : document.createElement("canvas"),
  start : function() {
    this.canvas.width = screen.width;
    this.canvas.height = screen.height;
    this.context = this.canvas.getContext("2d");
    document.body.insertBefore(this.canvas, document.body.childNodes[0]);
  }
}
```
hàm ``startGame()`` gọi phương thức ``start()`` của đối tượng ``myGameArea`` khi trang được tải

### Thêm các thành phần

```javascript
let myStadiumLeft, myStadiumRight, myGoal, myBall, myFootballer, myGoalKeeper, mySoundGoals, mySoundBravo, myScore, myKick, myStreak, centerCircle, centerLine

function startGame() {
  myGameArea.start();
  myGoal = new component(260, 110, "../img/goal.png", 640, 0, "image")
  myBall = new component(15, 15, "../img/ball.png", 765, 190, "image")
  myFootballer = new component(40, 95, "../img/cr7_penalty.png", 710, 180, "image")
  myGoalKeeper = new component(40, 75, "../img/degea.png", 750, 35, "image")
  myScore = new component("20px", "Consolas", "black", 150, 40, "text")
  centerCircle  = new component(150, 150, "white", screen.width / 2, screen.height - 200, "circle")
  centerLine = new component(screen.width - 70, screen.height - 200, "white", 70, screen.height - 200, "line")
  
  myGoal.update()
}

function component(width, height, color, x, y, type) {
  this.width = width
  this.height = height
  this.speedX = 0
  this.speedY = 0
  this.x = x
  this.y = y
  if (type == "image") {
    this.image = new Image()
    this.image.src = color
  }
  this.update = function () {
    ctx = myGameArea.context
    if (type == "image") {
      ctx.drawImage(this.image, this.x, this.y, this.width, this.height)
    }
    else if (type == "text") {
      ctx.font = this.width + " " + this.height
      ctx.fillStyle = color
      ctx.fillText(this.text, this.x, this.y)
    }
    else if (type == "rect") {
      ctx.fillStyle = color
      ctx.fillRect(this.x, this.y, this.width, this.height)
    }
    else if (type == "circle") {
      ctx.beginPath()
      ctx.arc(this.x, this.y, this.width || this.height, 0, 2 * Math.PI)
      ctx.strokeStyle = color
      ctx.stroke()
    }
    else if (type == "line") {
      ctx.beginPath()
      ctx.moveTo(this.x, this.y)
      ctx.lineTo(this.width, this.height)
      ctx.strokeStyle = color
      ctx.stroke()
    }
  }
}
```
sử dụng phương thức ``update()`` trong hàm ``component`` cho phép thêm các thành phần vào khu vực game.
Tùy vào đối số ``type`` khác nhau mà các thành phần được vẽ bởi các phương thức (trong canvas) khác nhau cho ra các hình vẽ đồ họa khác nhau

## Demo
[ Top Football ](https://doananhtingithub40102.github.io/Top_Football/html/penalty.html)