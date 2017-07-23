<template lang="pug">
  div
    .result(v-for="plan in plans")
      div(v-for="support in plan.supports")
        div
          span {{ support.area }}
          | (
          span 인력: {{ Math.round(support.labo) }}
          | ,
          span 탄약: {{ Math.round(support.ammo) }}
          | ,
          span 식량: {{ Math.round(support.food) }}
          | ,
          span 부품: {{ Math.round(support.part) }}
          | )
      .bold
        | 인력: {{ Math.round(plan.sum.labo) }}
        | 탄약: {{ Math.round(plan.sum.ammo) }}
        | 식량: {{ Math.round(plan.sum.food) }}
        | 부품: {{ Math.round(plan.sum.part) }}
      .bold 총 기대값: {{ Math.round(plan.sum.value) }}
</template>

<script>
export default {
  props: ['plans'],
};
</script>

<style lang="less" scoped>
div .result {
  border: 1px solid gray;

  div.bold {
    font-weight: bold;
  }
}
</style>

