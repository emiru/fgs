<template lang="pug">
  .main
    cond(@plan-as-usual="planAsUsual" @plan-for-sleep="planForSleep")
    transition(name="slide")
      results(:plans="plans" v-show="plans.length > 0")
</template>

<script>
import logsticSupports from '@/assets/logistic-support.json';
import Conditions from './Conditions';
import Results from './Results';

const makeSupportsCombination = (supports) => {
  const tempPlans = [];
  const tempSupports = [];
  for (let i = 0; i < supports.length; i += 1) {
    tempSupports.push(supports[i]);
    for (let j = i + 1; j < supports.length; j += 1) {
      tempSupports.push(supports[j]);
      for (let k = j + 1; k < supports.length; k += 1) {
        tempSupports.push(supports[k]);
        for (let l = k + 1; l < supports.length; l += 1) {
          tempSupports.push(supports[l]);
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
  return tempPlans;
};

export default {
  components: {
    cond: Conditions,
    results: Results,
  },
  data() {
    return {
      plans: [],
    };
  },
  computed: {
    supports() {
      return this.plans.supports;
    },
  },
  methods: {
    planForSleep(ratio, limitTime) {
      const availableSupports = logsticSupports.filter((
        item => (item.time <= limitTime)
      )).map((item) => {
        const expectationValue =
          (item.labo * ratio.labo)
          + (item.ammo * ratio.ammo)
          + (item.food * ratio.food)
          + (item.part * ratio.part);
        return {
          ...item,
          value: expectationValue,
        };
      });
      this.plans = makeSupportsCombination(availableSupports)
        .sort((o1, o2) => (o2.sum.value - o1.sum.value))
        .slice(0, 3);
    },
    planAsUsual(ratio, period) {
      const supportsWithValue = logsticSupports.map((item) => {
        const time = Math.ceil(item.time / period) * period;
        return {
          area: item.area,
          labo: (item.labo / time) * 60,
          ammo: (item.ammo / time) * 60,
          food: (item.food / time) * 60,
          part: (item.part / time) * 60,
        };
      }).map((item) => {
        const expectationValue =
          (item.labo * ratio.labo)
          + (item.ammo * ratio.ammo)
          + (item.food * ratio.food)
          + (item.part * ratio.part);
        return {
          ...item,
          value: expectationValue,
        };
      });
      this.plans = makeSupportsCombination(supportsWithValue)
        .sort((o1, o2) => (o2.sum.value - o1.sum.value))
        .slice(0, 3);
    },
  },
};
</script>

<style lang="less">
.slide-enter-active {
  transition: all .3s ease;
}
.slide-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-enter, .slide-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateY(-10px);
  opacity: 0;
}
</style>
