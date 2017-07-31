<template lang="pug">
  .conditions
    .inner-container
      .empty
      .ratio
        input(type="number" v-model="ratio.labo")
        input(type="number" v-model="ratio.ammo")
        input(type="number" v-model="ratio.food")
        input(type="number" v-model="ratio.part")
      .empty
      .time
        .toggle
          img(src="~@/assets/yes.png" v-show="forSleep" @click="forSleep=false")
          img(src="~@/assets/no.png" v-show="!forSleep" @click="forSleep=true")
        .yes(v-show="forSleep")
          input(type="number" v-model="limitTime")
        .no(v-show="!forSleep")
          input(type="number" v-model="period")
    .bottom
      button(@click="plan")
</template>

<script>
export default {
  data() {
    return {
      ratio: {
        labo: 4,
        ammo: 8,
        food: 1,
        part: 2,
      },
      forSleep: false,
      period: 60,
      limitTime: 480,
    };
  },
  methods: {
    plan() {
      if (this.forSleep) {
        this.$emit('plan-for-sleep', this.ratio, this.limitTime);
      } else {
        this.$emit('plan-as-usual', this.ratio, this.period);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.conditions {
  width: 765px;
  height: 472px;
  padding-top: 201px;
  background-image: url("~@/assets/main.png");

  .inner-container {
  background-color: rgba(0, 0, 0, 0);
    height: 305px;

    .empty:nth-child(1) {
      display: inline-block;
      width: 217px;
      height: 100%;
      vertical-align: top;
    }

    .ratio {
      display: inline-block;
      width: 109px;
      height: 100%;
      background-color: rgba(0, 0, 0, 0);
      vertical-align: top;

      input[type="number"] {
        display: block;
        margin-top: 39px;
        width: 100%;
        height: 43px;
        border: 0px;
        background-color: rgba(0, 0, 0, 0);
        text-align: right;
        font-size: 40px;
        font-weight: bold;

        &:nth-child(1) {
          margin-top: 2px;
        }
      }
    }

    .empty {
      display: inline-block;
      width: 68px;
      height: 100%;
      vertical-align: top;
    }

    .time {
    background-color: rgba(0, 0, 0, 0);
      width: 314px;
      height: 100%;
      display: inline-block;
      vertical-align: top;

      .toggle {
        display: block;
        padding-left: 100px;
        padding-top: 80px;
      }

      .yes, .no {
        margin-top: 2px;
        display: block;
        height: 171px;
        padding: 0 0 0 0;

        input[type="number"] {
        background-color: rgba(0, 0, 0, 0);
          border: 0px;

          width: 145px;
          height: 87px;
          text-align: right;
          font-size: 50px;
          font-weight: bold;
        }
      }

      .yes {
        background-image: url("~@/assets/yes_.png");

        input[type="number"] {
          margin-left: 10px;
          margin-top: 70px;
        }
      }

      .no {
        background-image: url("~@/assets/no_.png");

        input[type="number"] {
          margin-left: 157px;
          margin-top: 70px;
        }
      }
    }
  }

  button {
    margin-top: 60px;
    display: block;
    width: 763px;
    height: 107px;
    background-image: url("~@/assets/planning.png");
    background-color: rgba(0, 0, 0, 0);
    border: 0px;
  }
}
</style>
