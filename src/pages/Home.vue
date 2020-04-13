<template>
  <div class="home">
    <!-- Top Bar/Logo -->
    <div class="container-fluid">
      <div class="row">
        <div class="col-12 mr-auto">
          <img class="logo" alt="Vue logo" src="../assets/logo.png" />
          <p class="logo-text">
            ue
            <span class="logo-text-upper">S</span>lap
            <span class="logo-text-upper">G</span>ame
          </p>
        </div>
      </div>
    </div>
    <!-- End Top Bar/Logo -->

    <div class="container bg-image mt-2">
      <div class="row">
        <div class="col-3 ml-auto dsp-box text-center">
          <img class="img-player img-fluid mb-1" src="../assets/knight.png" alt />
          <div class="progress mx-auto">
            <div
              class="progress-bar bg-success player-health"
              id="player-health-green"
              role="progressbar"
            ></div>
            <div class="progress-bar bg-danger" id="player-health-red" role="progressbar"></div>
          </div>
          <div class="mt-1">
            <button
              @click="playerAttack(attackIndex)"
              class="btn btn-danger btn-sm m-1"
              v-for="(attack, attackIndex) in player.attacks"
              :disabled="player.cooldown == true || monster.health === 0"
            >{{ attack.name }}</button>
          </div>
        </div>
        <div class="col-3 mr-auto dsp-box text-center">
          <img class="img-monster img-fluid mb-1" src="../assets/goblin.png" alt />
          <div class="progress mx-auto">
            <div
              class="progress-bar bg-success monster-health"
              id="monster-health-green"
              role="progressbar"
            ></div>
            <div class="progress-bar bg-danger" id="monster-health-red" role="progressbar"></div>
          </div>
          <div class="mt-1">
            <button
              class="btn btn-danger btn-sm m-1"
              v-for="(attack, attackIndex) in monster.attacks"
              disabled
            >{{ attack.name }}</button>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-12 text-center mt-3">
          <h5>{{message}}</h5>
          <button class="btn btn-primary" @click="reset">Reset</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  data() {
    return {
      player: {
        health: 100,
        cooldown: false,
        attacks: [
          {
            name: "Slash",
            dmg: 5
          },
          {
            name: "Stab",
            dmg: 10
          },
          {
            name: "Special",
            dmg: 20
          }
        ]
      },
      monster: {
        health: 100,
        attacks: [
          {
            name: "Scratch",
            dmg: 5
          },
          {
            name: "Bite",
            dmg: 10
          },
          {
            name: "Special",
            dmg: 20
          }
        ]
      },
      message: "Select an attack!"
    };
  },
  methods: {
    playerAttack(attackIndex) {
      this.message = "Player used " + this.player.attacks[attackIndex].name;
      this.monster.health -= this.player.attacks[attackIndex].dmg;
      this.healthBar();
      this.player.cooldown = true;
      if (this.monster.health <= 0) {
        this.monster.health = 0;
        this.message = "Monster has been defeated!";
      } else {
        setTimeout(() => {
          this.monsterAttack();
        }, 1000);
      }
      setTimeout(() => {
        this.player.cooldown = false;
      }, 2000);
    },
    monsterAttack() {
      let keys = this.monster.attacks;
      let monsterChoiceIndex = Math.floor(Math.random() * keys.length);
      let monsterChoice = this.monster.attacks[monsterChoiceIndex];
      this.message = "Monster used " + monsterChoice.name;
      this.player.health -= monsterChoice.dmg;
      if (this.player.health <= 0) {
        this.player.health = 0;
        this.message = "Player has been defeated!";
      }
      this.healthBar();
    },
    healthBar() {
      document
        .getElementById("monster-health-green")
        .setAttribute("style", `width:${this.monster.health}%`);
      document
        .getElementById("monster-health-red")
        .setAttribute("style", `width:${100 - this.monster.health}%`);
      document
        .getElementById("player-health-green")
        .setAttribute("style", `width:${this.player.health}%`);
      document
        .getElementById("player-health-red")
        .setAttribute("style", `width:${100 - this.player.health}%`);
    },
    reset() {
      this.player.health = 100;
      this.player.cooldown = false;
      this.monster.health = 100;
      this.healthBar();
      this.message = "Select an attack!";
    }
  },
  components: {}
};
</script>
<style>
.bg-image {
  background-image: url("https://opengameart.org/sites/default/files/battleback2.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  height: 80vh;
}
.dsp-box {
  margin-top: 20%;
}
img.logo {
  height: 2.5rem;
}
img.img-player {
  height: 10rem;
  width: 10rem;
  border-radius: 25px;
  background-color: rgba(255, 255, 255, 0.5);
}
img.img-monster {
  height: 10rem;
  width: 10rem;
  border-radius: 25px;
  background-color: rgba(255, 255, 255, 0.5);
}
.progress {
  width: 10rem;
}
.player-health {
  width: 100%;
}
.monster-health {
  width: 100%;
}
.logo-text {
  display: inline;
  margin-left: -8px;
  color: #2c3e50;
}
.logo-text-upper {
  color: #42b983;
  font-weight: bold;
}
</style>
