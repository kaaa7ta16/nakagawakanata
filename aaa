<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>SMASH KANJI BATTLE</title>
    <style>
        body {
            margin: 0;
            background: #050505;
            color: white;
            overflow: hidden;
            font-family: serif;
        }
        /* 背景の巨大な漢字 */
        #bg-kanji {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 60vw;
            color: rgba(255, 50, 0, 0.1);
            z-index: -1;
            user-select: none;
        }
        canvas {
            display: block;
            margin: 0 auto;
            border-bottom: 5px solid #ff4d00;
        }
        .ui {
            position: absolute;
            top: 20px;
            width: 100%;
            text-align: center;
            font-size: 2rem;
            text-shadow: 0 0 10px #ff4d00;
        }
    </style>
</head>
<body>

<div id="bg-kanji">闘</div>
<div class="ui">蓄積ダメージ: <span id="damage">0</span>%</div>
<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const damageDisplay = document.getElementById('damage');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// ゲーム設定
const gravity = 0.8;
const friction = 0.9;

class Fighter {
    constructor(x, y, color) {
        this.x = x;
        this.y = y;
        this.width = 50;
        this.height = 50;
        this.color = color;
        this.vx = 0;
        this.vy = 0;
        this.damage = 0;
        this.isJumping = false;
    }

    draw() {
        ctx.fillStyle = this.color;
        ctx.shadowBlur = 15;
        ctx.shadowColor = this.color;
        ctx.fillRect(this.x, this.y, this.width, this.height);
        ctx.shadowBlur = 0;
    }

    update() {
        // 重力
        this.vy += gravity;
        this.x += this.vx;
        this.y += this.vy;
        this.vx *= friction;

        // 床との判定
        if (this.y + this.height > canvas.height - 50) {
            this.y = canvas.height - 50 - this.height;
            this.vy = 0;
            this.isJumping = false;
        }

        // 画面外（バースト）判定
        if (this.x < -100 || this.x > canvas.width + 100 || this.y < -500) {
            this.reset();
        }
    }

    reset() {
        this.x = canvas.width / 2;
        this.y = 100;
        this.vx = 0;
        this.vy = 0;
        this.damage = 0;
        damageDisplay.innerText = this.damage;
    }

    attack(target) {
        let dist = Math.hypot(this.x - target.x, this.y - target.y);
        if (dist < 100) {
            target.damage += 15;
            damageDisplay.innerText = target.damage;
            
            // 吹っ飛ばし計算：ダメージが大きいほど飛ぶ！
            let knockback = target.damage / 5;
            target.vx = (target.x > this.x ? 1 : -1) * (10 + knockback);
            target.vy = -10 - knockback;
        }
    }
}

const player = new Fighter(200, 100, '#38bdf8'); // 青（自分）
const enemy = new Fighter(canvas.width - 250, 100, '#ff4d00'); // 赤（相手）

const keys = {};

window.addEventListener('keydown', e => keys[e.code] = true);
window.addEventListener('keyup', e => keys[e.code] = false);

function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // 操作
    if (keys['KeyA']) player.vx -= 1.5;
    if (keys['KeyD']) player.vx += 1.5;
    if (keys['KeyW'] && !player.isJumping) {
        player.vy = -20;
        player.isJumping = true;
    }
    if (keys['Space']) {
        player.attack(enemy);
    }

    // 更新
    player.update();
    enemy.update();

    // 描画
    player.draw();
    enemy.draw();

    // 足場
    ctx.fillStyle = "#333";
    ctx.fillRect(0, canvas.height - 50, canvas.width, 50);

    requestAnimationFrame(gameLoop);
}

gameLoop();
</script>
</body>
</html>
