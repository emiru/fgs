<template lang="pug">
  .results
    .result(v-for="plan in plans")
      img(src="~@/assets/4_.png")
      .result-part(v-for="support in plan.supports")
        div {{ support.area }}
        div {{ Math.round(support.labo) }}
        div {{ Math.round(support.ammo) }}
        div {{ Math.round(support.food) }}
        div {{ Math.round(support.part) }}
      img.arrow(src="~@/assets/-.png")
      img(src="~@/assets/4_.png")
      .result-part
        div
        div {{ Math.round(plan.sum.labo) }}
        div {{ Math.round(plan.sum.ammo) }}
        div {{ Math.round(plan.sum.food) }}
        div {{ Math.round(plan.sum.part) }}
</template>

<script>
export default {
  props: ['plans'],
};
</script>

<style lang="less" scoped>
.results {
  width: 320px;
  height: 273px;
  padding-top: 3px;
  background-image: url("~@/assets/page.png");
  background-size: 322.5px;
  background-position-x: -3.5px;
  background-position-y: -1px;
  background-repeat: no-repeat;

  .result {
    width: 290px;
    height: 84px;
    margin-top: 4.5px;
    margin-left: 10px;

    img {
      display: inline-block;
      vertical-align: top;
      width: 17px;
      padding-top: 22px;

      &.arrow {
        width: 14px;
        height: 15px;
        padding-top: 43px;
        margin-left: 13px;
        margin-right: 9px;
      }
    }

    .result-part {
      display: inline-block;
      width: 40px;
      margin-left: 2px;

      div {
        display: flex;
        margin-top: 2px;
        font-size: 12px;
        font-weight: bold;
        height: 13.5px;
        justify-content: flex-end;
        align-items: center;

        &:nth-child(1) {
          margin-top: 1px;
          height: 18px;
          font-size: 14px;
        }
      }
    }

    &:nth-child(1) {
      margin-top: 0px;
    }
  }
}
</style>
