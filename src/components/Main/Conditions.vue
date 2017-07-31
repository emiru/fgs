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
        labo: this.$cookie.get('fsg.ratio.labo') - 0 || 4,
        ammo: this.$cookie.get('fsg.ratio.ammo') - 0 || 8,
        food: this.$cookie.get('fsg.ratio.food') - 0 || 1,
        part: this.$cookie.get('fsg.ratio.part') - 0 || 2,
      },
      forSleep: this.$cookie.get('fsg.forSleep') === 'true',
      period: this.$cookie.get('fsg.period') - 0 || 60,
      limitTime: this.$cookie.get('fsg.limitTime') - 0 || 480,
    };
  },
  methods: {
    plan() {
      this.$cookie.set('fsg.ratio.labo', this.ratio.labo);
      this.$cookie.set('fsg.ratio.ammo', this.ratio.ammo);
      this.$cookie.set('fsg.ratio.food', this.ratio.food);
      this.$cookie.set('fsg.ratio.part', this.ratio.part);
      this.$cookie.set('fsg.forSleep', this.forSleep);
      this.$cookie.set('fsg.period', this.period);
      this.$cookie.set('fsg.limitTime', this.limitTime);

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
  width: 320px;
  height: 281px;
  background-image: url("~@/assets/main.png");
  background-size: 320px;
  background-repeat: no-repeat;

  .inner-container {
    display: inline-block;
    margin-top: 83px;
    height: 129px;

    .empty:nth-child(1) {
      display: inline-block;
      width: 90px;
    }

    .ratio {
      display: inline-block;
      width: 48px;
      height: 100%;
      vertical-align: top;

      input[type="number"] {
        display: block;
        margin-top: 13.5px;
        padding: 0 0 0 0;
        width: 100%;
        height: 22px;
        border: 0px;
        text-align: right;
        font-size: 17px;
        font-weight: bold;
        background-color: rgba(0, 0, 0, 0);

        &:nth-child(1) {
          margin-top: 0px;
        }
      }
    }

    .empty {
      display: inline-block;
      width: 27px;
      height: 100%;
      vertical-align: top;
    }

    .time {
      width: 130px;
      height: 100%;
      display: inline-block;
      vertical-align: top;

      .toggle {
        display: block;
        padding-left: 42px;
        padding-top: 32px;
        height: 25px;

        img {
          width: 68px;

        }
      }

      .yes, .no {
        display: block;
        height: 70px;
        padding: 0 0 0 0;
        background-size: 100%;
        background-repeat: no-repeat;

        input[type="number"] {
          background-color: rgba(0, 0, 0, 0);
          border: 0px;

          width: 61px;
          height: 39px;
          text-align: right;
          font-size: 20px;
          font-weight: bold;
        }
      }

      .yes {
        background-image: url("~@/assets/yes_.png");

        input[type="number"] {
          margin-left: 3px;
          margin-top: 26px;
        }
      }

      .no {
        background-image: url("~@/assets/no_.png");

        input[type="number"] {
          margin-left: 64px;
          margin-top: 26px;
        }
      }
    }
  }

  button {
    margin-top: 24px;
    display: block;
    width: 320px;
    height: 45px;
    background-image: url("~@/assets/planning.png");
    background-size: 319px;
    background-repeat: no-repeat;
    background-color: rgba(0, 0, 0, 0);
    border: 0px;
  }
}
</style>
