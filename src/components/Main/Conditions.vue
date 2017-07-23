<template lang="pug">
  .conditions
    .item
      .title 인력(비율)
      div 
        input(type="number" v-model="ratio.labo")
    .item
      .title 탄약(비율)
      div
        input(type="number" v-model="ratio.ammo")
    .item
      .title 식량(비율)
      div
        input(type="number" v-model="ratio.food")
    .item 
      .title 부품(비율)
      div
        input(type="number" v-model="ratio.part")
    .item
      .title 잘 거니?
      div
        label
          input(type="radio" :value="true" v-model="forSleep")
          | 응
        label
          input(type="radio" :value="false" v-model="forSleep")
          | 아니
    .item(v-show="forSleep")
      .title 수면 시간(분)
      div
        input(type="number" v-model="limitTime")
    .item(v-show="!forSleep")
      .title 확인 주기(분)
      div
        input(type="number" v-model="period")
    button(@click="plan") Planning
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

  .item {
    display: flex;
    height: 30px;

    .title {
      display: flex;
      align-items: center;
      justify-content: flex-end;
      width: 110px;
    }

    div:nth-child(2) {
      flex: 1;
      display: flex;
      align-items: center;
      padding: 5px 20px 5px 20px;

      input[type="number"] {
        width: 100%;
        height: 100%;
        font-size: 18px;
        border: 1px solid gray;
      }
      
      label {
        flex: 1;
      }
    }
  }

  button {
    width: 100%;
    height: 35px;
    margin-top: 10px;
    background-color: white;
    border: 1px solid gray;
    font-size: 20px;
  }
}
</style>
