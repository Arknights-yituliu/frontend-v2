<template>
  <div id="pack" style="display: flex">
    <div id="pack_left">
      <div v-for="(pack2, index) in packsPPRData" :key="index" class="pack_unit">
        <div class="pack_img" :style="getPackPic(pack2.packName)">
          <div class="pack_img_text1">{{ pack2.packShowName }} ￥{{ pack2.packPrice }}</div>
        </div>

        <div class="pack_info">
          <div class="pack_info_text">
            共{{ pack2.packDraw }}抽 <br /><t1>￥{{ getEfficiency(pack2.packRmbPerDraw, 1) }}/抽</t1>
          </div>
          <div class="pack_chart">
            <div class="pack_chart_unit" v-show="pack2.packPPRDraw >= 1.57">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack2.packPPRDraw * 100, 0.75)">
                {{ getEfficiency(pack2.packPPRDraw * 100, 0) }}%
              </div>
            </div>
            <div class="pack_chart_unit">
              <div class="pack_chart_unit_text">大月卡</div>
              <div class="pack_chart_unit_ppr" :style="getWidth(157, 0.75)">157%</div>
            </div>
            <div class="pack_chart_unit" v-show="pack2.packPPRDraw < 1.57 && pack2.packPPRDraw >= 1">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack2.packPPRDraw * 100, 0.75)">
                {{ getEfficiency(pack2.packPPRDraw * 100, 0) }}%
              </div>
            </div>
            <div class="pack_chart_unit">
              <div class="pack_chart_unit_text">648源石</div>
              <div class="pack_chart_unit_ppr" :style="getWidth(100, 0.75)">100%</div>
            </div>
            <div class="pack_chart_unit" v-show="pack2.packPPRDraw < 1">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack2.packPPRDraw * 100, 0.75)">
                {{ getEfficiency(pack2.packPPRDraw * 100, 0) }}%
              </div>
            </div>
          </div>
        </div>

        <div class="pack_type">仅计抽卡</div>

        <div class="pack_corner corner_new" v-show="pack2.packType == 'limit'">New!</div>
        <div class="pack_corner corner_monthly" v-show="pack2.packType == 'monthly'">每月</div>
        <div class="pack_corner corner_monthly" v-show="pack2.packType == 'weekly'">每周</div>
        <div class="pack_corner corner_once" v-show="pack2.packType == 'once'">一次</div>
        <div class="pack_corner corner_once" v-show="pack2.packType == 'year'">双倍</div>
      </div>
    </div>
    <div id="pack_right">
      <div v-for="(pack3, index) in packsPPRData" :key="index" class="pack_unit">
        <div class="pack_img" :style="getPackPic(pack3.packName)">
          <div class="pack_img_text1">{{ pack3.packShowName }} ￥{{ pack3.packPrice }}</div>
        </div>

        <div class="pack_info">
          <div class="pack_info_text">
            {{ getEfficiency(pack3.packOriginium, 1) }}源石 <br /><t1
              >￥{{ getEfficiency(pack3.packRmbPerOriginium, 1) }}/石</t1
            >
          </div>
          <div class="pack_chart">
            <div class="pack_chart_unit" v-show="pack3.packPPROriginium >= 1.57">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack3.packPPROriginium * 100, 0.75)">
                {{ getEfficiency(pack3.packPPROriginium * 100, 0) }}%
              </div>
            </div>
            <div class="pack_chart_unit">
              <div class="pack_chart_unit_text">大月卡</div>
              <div class="pack_chart_unit_ppr" :style="getWidth(157, 0.75)">157%</div>
            </div>
            <div class="pack_chart_unit" v-show="pack3.packPPROriginium < 1.57 && pack3.packPPROriginium >= 1">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack3.packPPROriginium * 100, 0.75)">
                {{ getEfficiency(pack3.packPPROriginium * 100, 0) }}%
              </div>
            </div>
            <div class="pack_chart_unit">
              <div class="pack_chart_unit_text">648源石</div>
              <div class="pack_chart_unit_ppr" :style="getWidth(100, 0.75)">100%</div>
            </div>
            <div class="pack_chart_unit" v-show="pack3.packPPROriginium < 1">
              <div class="pack_chart_unit_text">本礼包</div>
              <div class="pack_chart_unit_ppr bg_red" :style="getWidth(pack3.packPPROriginium * 100, 0.75)">
                {{ getEfficiency(pack3.packPPROriginium * 100, 0) }}%
              </div>
            </div>
          </div>
        </div>

        <div class="pack_type">材料折合源石</div>
        <div class="pack_corner corner_new" v-show="pack3.packType == 'limit'">New!</div>
        <div class="pack_corner corner_monthly" v-show="pack3.packType == 'monthly'">每月</div>
        <div class="pack_corner corner_monthly" v-show="pack3.packType == 'weekly'">每周</div>
        <div class="pack_corner corner_once" v-show="pack3.packType == 'once'">一次</div>
        <div class="pack_corner corner_once" v-show="pack3.packType == 'year'">双倍</div>
      </div>
    </div>
  </div>
</template>

<style>
#pack {
  background-color: rgb(43, 72, 101);
  padding: 20px 0px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.pack_unit {
  margin: 20px;
  width: 522px;
  height: 120px;
  overflow: hidden;
}
.pack_img {
  width: 160px;
  height: 120px;
  position: absolute;
  /* display: inline-block; */
  /* z-index: 20; */
  /* vertical-align: top; */
  border-radius: 8px;
  box-shadow: 2px 2px 4px rgb(0 0 0 / 50%);
}
.pack_img_text1 {
  position: relative;
  top: 93px;
  width: 160px;
  height: 24px;
  background-color: rgba(0, 0, 0, 0.6902);
  font-size: 14px;
  font-weight: 600;
  color: white;
  text-align: center;
  padding-top: 3px;
  border-radius: 0px 0px 8px 8px;
}
.pack_info {
  display: inline-block;
  z-index: 10;
  height: 108px;
  width: 360px;
  background-color: #000000cc;
  margin: 6px 0px 6px 156px;
  border-radius: 4px;
  box-shadow: 2px 2px 4px rgb(0 0 0 / 50%);
}

.pack_info_text {
  width: 96px;
  padding: 18px 0px 18px 4px;
  font-size: 20px;
  text-align: center;
  font-weight: 600;
  color: #6ed7ff;
  display: inline-block;
  line-height: 36px;
  vertical-align: top;
}

t1 {
  color: #c59447;
}

.pack_chart {
  display: inline-block;
  margin-top: 18px;
  width: 240px;
  /* padding: 10px 0px; */
  border-left: 1px solid #d0d0d0;
  white-space: nowrap;
  overflow: hidden;
}

.pack_chart_unit_text {
  display: inline-block;
  padding: 0px 2px;
  width: 66px;
  /* background-color: burlywood; */
  border-radius: 6px;
  /* margin: 0px 0px 0px 0px; */
  vertical-align: middle;
  text-align: center;
  font-size: 16px;
  color: wheat;
  height: 24px;
}
.pack_chart_unit_ppr {
  display: inline-block;
  background-color: rgb(56, 112, 161);
  color: white;
  font-size: 12px;
  border-radius: 16px;
  padding: 0px 8px;
}

.pack_type {
  display: inline-block;
  color: gray;
  position: relative;
  /* text-align: right; */
  float: right;
  right: 12px;
  bottom: 28px;
  font-size: 14px;
}

.pack_corner {
  transform: rotate(-35deg);
  width: 96px;
  top: -112px;
  left: -28px;
  position: relative;
  color: white;
  text-align: center;
  font-size: 14px;
  box-shadow: 2px 2px 4px rgb(0 0 0 / 50%);
}
.corner_new {
  background: brown;
}
.corner_monthly {
  background: indigo;
}
.corner_once {
  background: #bf7c00;
}

.bg_red {
  background-color: rgb(250, 83, 83);
}
</style>

<script>
import packsPPR from "static/json/pack_packsPPR.json";

export default {
  data() {
    return {
      packsPPRData: packsPPR,
    };
  },
  methods: {
    getWidth(num, scale) {
      return "width:" + num * scale + "px";
    },
    getEfficiency(num, acc) {
      acc = typeof acc !== "undefined" ? acc : 2;
      return parseFloat(num).toFixed(acc);
    },
    getPackImgUrl(img) {
      return "/img/packs/" + img + ".png";
    },
    getPackPic(img) {
      if (img === "空的小钱夹")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c3f8da22f177a05be666d7b5688beda7.JPG) 12% 20%  / 500% no-repeat,#444444";
      if (img === "大帝的手提箱")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c3f8da22f177a05be666d7b5688beda7.JPG) 40% 20%  / 500% no-repeat,#444444";
      if (img === "资深干员特训礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c3f8da22f177a05be666d7b5688beda7.JPG) 70% 20%  / 500% no-repeat,#444444";
      if (img === "调用凭证组合包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c3f8da22f177a05be666d7b5688beda7.JPG) 99% 20%  / 500% no-repeat,#444444";
      if (img === "特训意向礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/1a4d8717d513321e2bc13c64f1f90b45.JPG) 16% 20%  / 500% no-repeat,#444444";
      if (img === "剧场之友组合包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/1a4d8717d513321e2bc13c64f1f90b45.JPG) 57% 9% / 400% no-repeat,#444444";
      if (img === "高级特训意向礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/1a4d8717d513321e2bc13c64f1f90b45.JPG) 97% 20%  / 500% no-repeat,#444444";
      if (img === "辟路芯片礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c9f1738ab94962bff14edc1dc92c098e.JPG) 24% 20%  / 400% no-repeat,#444444";
      if (img === "斩荆芯片礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221021/c9f1738ab94962bff14edc1dc92c098e.JPG) 85% 20%  / 400% no-repeat,#444444";
      if (img === "枯焰芯片礼包")
        return "background:url(https://ak.hycdn.cn/announce/images/20221208/e2f417b45fcc108caf01521d7d0d09fe.jpg) 75% 50%  / 250% no-repeat,#444444";
      return "background:url(/img/packs/" + img + ".png) 00% 110% / cover no-repeat,#444444;";
    },
  },
};
</script>
