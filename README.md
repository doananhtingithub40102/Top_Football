# Top_Football

Top Football - Game bóng đá sử dụng canvas để tạo game

## Các ngôn ngữ, công nghệ
HTML, CSS, Javascript

## Hướng dẫn cách chơi
Sút bóng vào lưới để ghi thật nhiều bàn thắng.

> **Hướng bóng**

Nhấn ``D`` để sút, đồng thời giữ phím mũi tên ``<-`` để sút bóng về trái và phím ``->`` để sút bóng về phải, nếu không giữ 1 trong 2 phím mũi tên trên bóng sẽ đi thẳng

> **Lực bóng**

Giữ ``D`` càng lâu bóng sẽ bay càng cao. Nếu giữ ``D`` quá lâu (>= 0.45s), bóng sẽ bay lên trời

> **Chiến thắng**

Sút bóng vào lưới mà thủ môn không thể bắt được sẽ tính 1 điểm thắng

Trò chơi sẽ chơi mãi mãi đến khi nào bạn muốn thoát ra!

## Một số hàm/phương thức cơ bản để tạo game

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
  myBall = new component(15, 15, "../img/ball.png", 765, 190, "image")
  myScore = new component("20px", "Consolas", "black", 150, 40, "text")
  centerCircle  = new component(150, 150, "white", screen.width / 2, screen.height - 200, "circle")
  centerLine = new component(screen.width - 70, screen.height - 200, "white", 70, screen.height - 200, "line")
  
  myBall.update()
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

Tùy vào đối số ``type`` khác nhau mà các thành phần được vẽ bởi các phương thức (trong canvas) khác nhau cho ra các hình vẽ đồ họa khác nhau.

### Cập nhật game

```javascript
var myGameArea = {
  canvas : document.createElement("canvas"),
  start : function() {
    this.canvas.width = screen.width;
    this.canvas.height = screen.height;
    this.context = this.canvas.getContext("2d");
    document.body.insertBefore(this.canvas, document.body.childNodes[0]);
    this.interval = setInterval(updateGameArea, 20);
  },
  clear : function() {
    this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
  }
}

function updateGameArea() {
  myGameArea.clear();
  myBall.update();
}
```
cứ 20ms (50 lần mỗi giây) sẽ gọi hàm ``updateGameArea()``, làm sạch khu vực game hiện tại và vẽ lại các thành phần

### Di chuyển các thành phần

```javascript
function updateGameArea() {
  myGameArea.clear();
  myBall.x += 1;
  myBall.update();
}
```
cứ 20ms ``myBall `` sẽ dịch chuyển sang phải 1px

Di chuyển các thành phần bằng bàn phím
```javascript
let myGameArea = {
  canvas: document.createElement("canvas"),
  start: function () {
    this.canvas.width = screen.width
    this.canvas.height = screen.height
    this.context = this.canvas.getContext("2d")
    document.body.insertBefore(this.canvas, document.body.childNodes[0])
    this.interval = setInterval(updateGameArea, 20)
    window.addEventListener("keydown", function (e) {
      myGameArea.keys = (myGameArea.keys || [])
      myGameArea.keys[e.keyCode] = true
    })
    window.addEventListener("keyup", function (e) {
      myGameArea.keys[e.keyCode] = false
    })
  }
}

function component(width, height, color, x, y, type) {
  this.speedX = 0
  this.speedY = 0
  this.x = x
  this.y = y
  ...
  this.newPos = function () {
    this.x += this.speedX
    this.y += this.speedY
  }
}

function updateGameArea() {
  myGameArea.clear()
  myBall.speedX = 0
  myBall.speedY = 0
  if (myGameArea.keys && myGameArea.keys[37]) {myBall.speedX = -1; }
  if (myGameArea.keys && myGameArea.keys[39]) {myBall.speedX = 1; }
  if (myGameArea.keys && myGameArea.keys[38]) {myBall.speedY = -1; }
  if (myGameArea.keys && myGameArea.keys[40]) {myBall.speedY = 1; }
  myBall.newPos();
  myBall.update();
}
```
khi đang nhấn giữ phím bất kỳ thì giá trị của nó là true và ngược lại.

Các thành phần sẽ di chuyển dựa theo phím đang nhấn

## Demo
[ Top Football ](https://doananhtingithub40102.github.io/Top_Football/html/penalty.html)
