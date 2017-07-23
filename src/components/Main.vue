<template lang="pug">
  .main
    .conditions
      div Target Ratio
        div
          span labo
          input(type="number" v-model="ratio.labo")
        div
          span ammo
          input(type="number" v-model="ratio.ammo")
        div
          span food
          input(type="number" v-model="ratio.food")
        div
          span part
          input(type="number" v-model="ratio.part")
      div period
        input(type="number" v-model="period")
    div
      button(@click="plan") Planning
    div Result
      div(v-for="plan in plans")
        div(v-for="support in plan.supports")
          div {{ support }}
        div
          | Expectation Value: {{ plan.sum.value }},
          | labo: {{ plan.sum.labo }}
          | ammo: {{ plan.sum.ammo }}
          | food: {{ plan.sum.food }}
          | part: {{ plan.sum.part }}
</template>

<script>
import supports from '@/assets/logistic-support.json';

export default {
  data() {
    return {
      ratio: {
        labo: 1,
        ammo: 1,
        food: 1,
        part: 1,
      },
      period: 60,
      plans: [],
    };
  },
  computed: {
    supports() {
      return this.plans.supports;
    },
  },
  methods: {
    plan() {
      const supportsWithValue = supports.map((item) => {
        const time = Math.ceil(item.time / this.period) * this.period;
        return {
          area: item.area,
          time: item.time,
          labo: (item.labo / time) * 60,
          ammo: (item.ammo / time) * 60,
          food: (item.food / time) * 60,
          part: (item.part / time) * 60,
        };
      }).map((item) => {
        const expectationValue =
          (item.labo * this.ratio.labo)
          + (item.ammo * this.ratio.ammo)
          + (item.food * this.ratio.food)
          + (item.part * this.ratio.part);
        return {
          ...item,
          value: expectationValue,
        };
      });

      const tempPlans = [];
      const tempSupports = [];
      for (let i = 0; i < supportsWithValue.length; i += 1) {
        tempSupports.push(supportsWithValue[i]);
        for (let j = i + 1; j < supportsWithValue.length; j += 1) {
          tempSupports.push(supportsWithValue[j]);
          for (let k = j + 1; k < supportsWithValue.length; k += 1) {
            tempSupports.push(supportsWithValue[k]);
            for (let l = k + 1; l < supportsWithValue.length; l += 1) {
              tempSupports.push(supportsWithValue[l]);
              tempPlans.push({
                supports: tempSupports.slice(0),
                sum: tempSupports.reduce((pre, cur) => ({
                  labo: pre.labo + cur.labo,
                  ammo: pre.ammo + cur.ammo,
                  food: pre.food + cur.food,
                  part: pre.part + cur.part,
                  value: pre.value + cur.value,
                }), { labo: 0, ammo: 0, food: 0, part: 0, value: 0 }),
              });
              tempSupports.pop();
            }
            tempSupports.pop();
          }
          tempSupports.pop();
        }
        tempSupports.pop();
      }

      this.plans = tempPlans
        .sort((o1, o2) => (o2.sum.value - o1.sum.value))
        .splice(0, 5);
    },
  },
};
</script>

<style lang="less" scoped>
.main {
  width: 100%;
  .conditions {
    background-color: blue;
    div span {
      display: inline-block;
      width: 80px;
    }
  }
}
</style>
