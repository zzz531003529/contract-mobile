<template>
  <div class="text-wrapper">
      <div class="title" v-if="title">
          {{title}}
      </div>
      <div class="content" ref="content">
          <p v-for="(item,index) in data" :key="index">
             <span>{{item.amount | formatCash}}</span> 
             <span>{{item.name}}</span>
             <u :style="{'left':left}"></u>
          </p>
          <slot name="chart"></slot>
      </div>
  </div>
</template>

<script>
import { pie } from "@/assets/js/pie.js";
// const pie1 = JSON.parse(JSON.stringify(pie));
// const pie2 = JSON.parse(JSON.stringify(pie));
// const pie3 = JSON.parse(JSON.stringify(pie));
// console.log(pie0)
export default {
  data() {
    return {
      isMounted: false,
      pie,
      // chart: [pie1, pie2, pie3],
      series: [
        [{ name: "其他", value: "" }, { name: "合同应收", value: "" }],
        [{ name: "其他", value: "" }, { name: "权责应收", value: "" }],
        [{ name: "其他", value: "" }, { name: "发票应收", value: "" }]
      ]
    };
  },
  props: {
    data: {
      type: Array
    },
    title: ""
  },

  computed: {
    left() {
      let arr = this.$refs;
      if (this.isMounted) {
        switch (this.$refs.content.children.length) {
          case 2:
            return "5.5rem";
          case 3:
            return "5rem";
        } 
      }
    }
  },

  mounted() {
      this.isMounted = true;
  },

  methods: {
    getData(){
        this.data.forEach((element, index) => {
          console.log(element.title);
          this.chart[index].title.text = element.title;
          this.series[index].forEach((el,i) =>{
            el.value = element.percent[i]
            console.log(element.percent[i])
          })
          console.log(this.series[index])
          this.chart[index].series[0].data = this.series[index]
        });
    }
  },

  watch: {
  },

  components: {}
};
</script>
<style lang='less' scoped>
@import url("../assets/styles/mixin.less");
.text-wrapper {
  margin: 0.45rem;
  padding: 0.7rem 0;
  background: #fff;
  .borderRadios();
  .title {
    height: 1.5rem;
    font-size: 0.7rem;
    font-weight: bold;
    // line-height: 1.5rem;
    padding-left: 0.5rem;
    &::before {
      display: inline-block;
      content: "";
      vertical-align: middle;
      .wh(0.3rem,0.3rem);
      border-radius: 50%;
      background: @theme-color;
    }
  }
  .content {
    margin: 0.45rem;
    .flex(@justify:space-around);
    p {
      .flex(@dir:column);
      position: relative;
      span:first-child {
        font-size: 0.8rem;
        font-weight: bold;
      }
      span:nth-child(2) {
        padding-top: 0.75rem;
        font-weight: 100;
        font-size: 0.6rem;
      }
      u {
        position: absolute;
        height: 100%;
        width: 1px;
        background: #ccc;
        // left: 140%;
      }
    }
    p:last-child {
      u {
        display: none;
      }
    }
    .echarts {
      width: 4rem;
      height: 4rem;
    }
  }
}
</style>