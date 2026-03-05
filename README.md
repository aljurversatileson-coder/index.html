# index.html
<!DOCTYPE html>
<html>
<head>
<title>Mini MOBA Prototype</title>
<style>
  body { margin:0; overflow:hidden; background:#222; }
  canvas { background:#333; display:block; margin:auto; }
</style>
</head>
<body>

<canvas id="gameCanvas" width="800" height="500"></canvas>

<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

let keys = {};
document.addEventListener("keydown", e => keys[e.key] = true);
document.addEventListener("keyup", e => keys[e.key] = false);

let hero = { x:100, y:200, size:30, speed:4, hp:100 };
let enemy = { x:600, y:200, size:30, hp:100 };
let bullets = [];

function update(){
  // Movement (WASD)
  if(keys["w"]) hero.y -= hero.speed;
  if(keys["s"]) hero.y += hero.speed;
  if(keys["a"]) hero.x -= hero.speed;
  if(keys["d"]) hero.x += hero.speed;

  // Shoot (Space)
  if(keys[" "]){
    bullets.push({x:hero.x+15, y:hero.y+15, speed:6});
    keys[" "] = false;
  }

  // Move bullets
  bullets.forEach(b => b.x += b.speed);

  // Collision
  bullets.forEach((b,i)=>{
    if(b.x < enemy.x + enemy.size &&
       b.x > enemy.x &&
       b.y < enemy.y + enemy.size &&
       b.y > enemy.y){
         enemy.hp -= 10;
         bullets.splice(i,1);
       }
  });
}

function draw(){
  ctx.clearRect(0,0,canvas.width,canvas.height);

  // Hero
  ctx.fillStyle = "blue";
  ctx.fillRect(hero.x, hero.y, hero.size, hero.size);

  // Enemy
  ctx.fillStyle = "red";
  ctx.fillRect(enemy.x, enemy.y, enemy.size, enemy.size);

  // Bullets
  ctx.fillStyle = "yellow";
  bullets.forEach(b=> ctx.fillRect(b.x,b.y,5,5));

  // HP Text
  ctx.fillStyle="white";
  ctx.fillText("Hero HP: "+hero.hp,10,20);
  ctx.fillText("Enemy HP: "+enemy.hp,10,40);

  if(enemy.hp <= 0){
    ctx.fillText("YOU WIN!",350,250);
  }
}

function gameLoop(){
  update();
  draw();
  requestAnimationFrame(gameLoop);
}

gameLoop();
</script>

</body>
</html>
