<template>
  <div>
    <h3>Canavar Avı Oyunu</h3>
    <br />
    <hr />
    <div class="header">
      <div class="sen"><b>SEN</b></div>
      <div class="canavar"><b>CANAVAR</b></div>
    </div>
    <div class="progressBar">
      <div class="greyBar">
        <div class="senBar" :style="{ width: playerHeal + '%' }">
          %{{ playerHeal }}
        </div>
      </div>
      <div class="greyBar">
        <div class="canavarBar" :style="{ width: monsterHeal + '%' }">
          %{{ monsterHeal }}
        </div>
      </div>
    </div>
    <div class="buttonBar" v-if="!gameIsOn">
      <div
        class="button"
        :style="{ backgroundColor: greenColor }"
        @click="startGame"
      >
        Yeni Oyun
      </div>
    </div>
    <div class="buttonBar" v-if="gameIsOn">
      <div
        class="button"
        :style="{ backgroundColor: redColor }"
        @click="attack"
      >
        Saldırı
      </div>
      <div
        class="button"
        :style="{ backgroundColor: orangeColor }"
        @click="specialAttack"
      >
        Özel Saldırı
      </div>
      <div
        class="button"
        :style="{ backgroundColor: greenColor }"
        @click="healUp"
      >
        İlk Yardım
      </div>
      <div class="button" @click="giveUp">Pes Et!</div>
    </div>
    <ul class="health" v-if="gameIsOn">
      <li
        v-for="log in logs"
        :key="log"
        :class="{
          playerDamage: log.turn == 'P',
          monsterDamage: log.turn == 'M',
        }"
      >
        {{ log.text }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHeal: 100,
      monsterHeal: 100,
      gameIsOn: false,
      orangeColor: "orange",
      greenColor: "lightgreen",
      redColor: "red",
      logs: [],
    };
  },
  methods: {
    startGame: function () {
      this.gameIsOn = true;
    },
    attack: function () {
      var point = Math.ceil(Math.random() * 10);
      this.monsterHeal = this.monsterHeal - point;
      this.addToLog({ turn: "P", text: "Oyuncu atağı ( " + point + ")" });
      this.monsterAttack();
    },
    specialAttack: function () {
      var point = Math.ceil(Math.random() * 25);
      this.monsterHeal = this.monsterHeal - point;
      this.addToLog({ turn: "P", text: "Özel oyuncu atağı ( " + point + ")" });

      this.monsterAttack();
    },
    healUp: function () {
      var point = Math.ceil(Math.random() * 20);
      this.playerHeal = this.playerHeal + point;
      this.addToLog({
        turn: "P",
        text: "Oyuncuya ilk yardım seti verildi ( " + point + ")",
      });
      this.monsterAttack();
    },
    giveUp: function () {
      this.playerHeal = 0;
      this.gameIsOn = false;
      this.addToLog({ turn: "P", text: "Oyuncu pes etti..." });
    },
    monsterAttack: function () {
      var point = Math.ceil(Math.random() * 12);
      this.playerHeal = this.playerHeal - point;
      this.addToLog({ turn: "M", text: "Canavar atağı ( " + point + ")" });
    },
    addToLog: function (log) {
      this.logs.push(log);
    },
  },
  watch: {
    playerHeal: function (value) {
      if (value <= 0) {
        this.playerHeal = 0;

        if (confirm("Oyunu kaybettin. Tekrar oynamak ister misin ?")) {
          this.playerHeal = 100;
          this.monsterHeal = 100;
          this.logs = [];
        }
      } else if (value >= 100) {
        this.playerHeal = 100;
      }
    },
    monsterHeal: function (value) {
      if (value <= 0) {
        this.monsterHeal = 0;
        if (confirm("Oyunu kazandın. Tekrar oynamak ister misin ?")) {
          this.playerHeal = 100;
          this.monsterHeal = 100;
          this.logs = [];
        }
      }
    },
  },
};
</script>
<style>
.playerDamage {
  margin-top: 3px;
  background-color: rgb(162, 162, 220);
  width: 95%;
  height: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.monsterDamage {
  margin-top: 3px;
  width: 95%;
  background-color: rgb(232, 153, 153);
  height: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.health {
  width: 95%;
  border: 1px solid #cccccc;
  box-shadow: 2px 1px #cccccc;
  margin-top: 15px;
}
.greyBar {
  width: 35%;
  height: 30px;
  background-color: lightgray;
}

.senBar {
  height: 30px;
  background-color: green;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}
.canavarBar {
  height: 30px;
  background-color: green;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
}
.progressBar {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin-top: 10px;
}

.lightgreen {
  background-color: lightgreen;
}
.red {
  background-color: red;
}
.orange {
  background-color: orange;
}
.buttonBar {
  width: 100%;
  height: 60px;
  border: 1px solid #cccccc;
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  box-shadow: 2px 1px #cccccc;
}
.button {
  height: 30px;
  width: 20%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: larger;
  border: 1px solid #cccccc;
  border-radius: 5px;
  box-shadow: 2px 1px #cccccc;
}

.header {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>
