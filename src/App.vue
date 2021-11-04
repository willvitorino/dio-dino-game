<template>
  <div class="background">
    <div class="dino"></div>
  </div>
</template>

<script>
export default {
  name: 'App',

  data: () => ({
    isJumping: false,
    isGameOver: false,
    position: 0
  }),

  created() {
    document.addEventListener('keyup', this.handleKeyUp)
  },

  mounted() {
    this.dino = document.querySelector('.dino')
    this.background = document.querySelector('.background')
    this.createCactus();
  },

  methods: {
    handleKeyUp(event) {
      if (event.keyCode === 32 || event.code === 'ArrowUp' ) {
        if (!this.isJumping) {
          this.jump();
        }
      }
    },

    jump() {
      this.isJumping = true;

      let upInterval = setInterval(() => {
        if (this.position >= 150) {
          // Descendo
          clearInterval(upInterval);

          let downInterval = setInterval(() => {
            if (this.position <= 0) {
              clearInterval(downInterval);
              this.isJumping = false;
            } else {
              this.position -= 20;
              this.dino.style.bottom = this.position + 'px';
            }
          }, 20);
        } else {
          // Subindo
          this.position += 20;
          this.dino.style.bottom = this.position + 'px';
        }
      }, 20);
    },

    createCactus() {
      const cactus = document.createElement('div');
      let cactusPosition = 1000;
      let randomTime = Math.random() * 6000;

      if (this.isGameOver) return;

      cactus.classList.add('cactus');
      this.background.appendChild(cactus);
      cactus.style.left = cactusPosition + 'px';

      let leftTimer = setInterval(() => {
        if (cactusPosition < -60) {
          // Saiu da tela
          clearInterval(leftTimer);
          this.background.removeChild(cactus);
        } else if (cactusPosition > 0 && cactusPosition < 60 && this.position < 60) {
          // Game over
          clearInterval(leftTimer);
          this.isGameOver = true;
          document.body.innerHTML = '<h1 class="game-over">Fim de jogo</h1>';
        } else {
          cactusPosition -= 10;
          cactus.style.left = cactusPosition + 'px';
        }
      }, 20);

      setTimeout(this.createCactus, randomTime);
    }
  }
}
</script>

<style>
body, #app {
  background: #fafafa;
}

.dino {
  position: absolute;
  bottom: 0;
  background-image: url(../dino.png);
  width: 60px;
  height: 60px;
}

.cactus {
  position: absolute;
  width: 60px;
  height: 60px;
  bottom: 0;
  background-image: url(../cactus.png);
}

.game-over {
  text-align: center;
  color: #666;
  margin: 50px 0;
  font-family: arial;
}

@keyframes slideright {
  from {
      background-position: 70000%;
  }
  to {
      background-position: 0%;
  }
}

.background {
  position: absolute;
  bottom: 0px;
  background-image: url('../background.png');
  background-repeat: repeat-x;
  animation: slideright 600s infinite linear;
  -webkit-animation: slideright 600s infinite linear;
  width: 100%;
  height: 200px;
}

</style>
