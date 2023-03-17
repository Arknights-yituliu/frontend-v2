<template>
  <div id="riic">
    <div id="riic_controlPanel">
      <div class="riic_building">
        <div class="riic_building_title">控制面板</div>
        <div class="riic_building_parameter">
          <div class="parameter_text">作业名称</div>
          <el-input
            class="parameter_inputbox"
            size="small"
            placeholder="究极资本家v1.0"
            v-model="scheduleJson.title"
          ></el-input>
        </div>
        <div class="riic_building_parameter">
          <div class="parameter_text">描述(可选)</div>
          <el-input
            class="parameter_inputbox"
            size="small"
            placeholder="适合全干员，压榨每一个工具人！"
            v-model="scheduleJson.description"
          ></el-input>
        </div>
        <div class="riic_building_parameter">
          <div class="parameter_text">作者(可选)</div>
          <el-input
            class="parameter_inputbox"
            size="small"
            placeholder="yituliu"
            v-model="scheduleJson.author"
          ></el-input>
        </div>
        <div class="riic_building_parameter">
          <div class="parameter_text">基建模式</div>
          <el-radio-group size="small" v-model="scheduleJson.buildingType">
            <el-radio-button label="243"></el-radio-button>
            <el-radio-button label="153"></el-radio-button>
            <el-radio-button label="333"></el-radio-button>
            <el-radio-button label="252"></el-radio-button>
          </el-radio-group>
        </div>
        <!-- {{ scheduleJson.plans[0].rooms.control }} -->
      </div>
      <div class="riic_building">
        <div class="riic_building_title">排班方案</div>
        <div class="riic_building_parameter">
          <div class="parameter_text">换班次数</div>
          <el-radio-group size="small" v-model="scheduleJson.planTimes">
            <el-radio-button label="2班"></el-radio-button>
            <el-radio-button label="3班"></el-radio-button>
            <!-- <el-radio-button label="4班" ></el-radio-button> -->
          </el-radio-group>
        </div>
        <div class="riic_building_parameter" v-for="(planItem, index) in plans" :key="index">
          <div class="parameter_text" style="width: 108px">名称/起止时间</div>
          <el-input
            size="small"
            class="parameter_inputbox"
            placeholder="主力组A"
            style="width: 90px"
            v-model="planItem.name"
          ></el-input>
          <!-- <el-input
            size="small"
            class="parameter_inputbox"
            placeholder="20:00"
            style="width: 80px"
            v-model="period_plan0[0]"
          ></el-input>
          <el-input
            size="small"
            class="parameter_inputbox"
            placeholder="23:59"
            style="width: 80px"
            v-model="period_plan0[1]"
          ></el-input> -->
        </div>

        <div class="riic_building_parameter">
          *跨天需写成 22:00 —— 06:00 (MAA执行周期为24小时)
          <!-- (示例，写的时候别带&emsp;[&emsp;]！) -->
        </div>
      </div>

      <div class="riic_building">
        <div class="riic_building_title">方案导入/导出</div>
        <div class="riic_building_parameter">
          <el-button size="medium" type="primary" round style="width: 126px"> 生成排班方案 </el-button>
          <div
            id="export_cover"
            style="position: absolute; width: 235px; height: 40px; margin: -38px 18px 18px 137px; background: #ffffff80"
          ></div>
          <a>
            <el-button size="medium" type="primary" round style="width: 108px; margin-left: 12px">
              导出到本地
            </el-button>
          </a>
          <!-- <el-button size="medium" type="primary" round style="width:108px;padding-left:10px;" @click="MaaURLCopy()">
                导出到MAA
          </el-button> -->
        </div>
        <!-- <div class="riic_building_parameter">
          <el-button size="medium" type="primary" round style="width:126px;">
            通过文件导入
          </el-button> *(后续可用)
        </div> -->
        <div class="riic_building_parameter">
          <el-button size="medium" type="primary" round style="width: 126px"> 通过id导入 </el-button>
          <el-input
            size="small"
            class="parameter_inputbox"
            placeholder="id"
            style="margin-left: 12px; width: 150px"
          ></el-input>
        </div>
        <div class="riic_building_parameter">
          *导出json文件的文件名即为id <br />
          或打开排班文件最末尾中寻找id
        </div>
        <div class="riic_building_parameter">
          <a href="https://www.wjx.cn/vm/mRNtSzd.aspx#">
            <el-button size="medium" type="warning" round style="width: 186px"> 排班生成器 问题反馈 </el-button>
          </a>
        </div>

        <!-- 如果需要注释 从这里开始 -->
        <!--        <div class="riic_building_parameter">-->
        <!--          <div class="parameter_text">预设模板</div>-->
        <!--          <el-radio-group size="small">-->
        <!--            <el-radio-button label="243"></el-radio-button>-->
        <!--            <el-radio-button label="153"></el-radio-button>-->
        <!--            <el-radio-button label="333"></el-radio-button>-->
        <!--            <el-radio-button label="252" disabled></el-radio-button>-->
        <!--          </el-radio-group>-->
        <!--        </div>-->
        <!-- 到这里结束 -->
      </div>
    </div>
    <el-divider></el-divider>
    <div id="riic_workerSets" v-for="(planItem, index) in plans" :key="index">
      <div class="riic_workerSet">
        <div class="riic_building building_uni">
          <div class="riic_building_title">班次基本信息</div>
          <div class="riic_building_parameter">
            <div class="parameter_text">班次名称</div>
            <el-input
              size="small"
              class="parameter_inputbox"
              placeholder="例如：主力组A"
              v-model="planItem.name"
            ></el-input>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text">描述</div>
            <el-input
              size="small"
              class="parameter_inputbox"
              placeholder="可选"
              v-model="planItem.description"
            ></el-input>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text">无人机</div>
            <el-switch active-color="#13ce66" inactive-color="#c0c4cc" v-model="planItem.drones.enable"></el-switch>
            <el-radio-group size="small" v-model="planItem.drones.room">
              <el-radio-button label="贸易站" value="manufacture"></el-radio-button>
              <el-radio-button label="制造站" value="trading"></el-radio-button>
            </el-radio-group>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text" style="font-size: 15px">目标房间编号</div>
            <el-radio-group size="small" v-model="planItem.drones.index">
              <el-radio-button label="1"></el-radio-button>
              <el-radio-button label="2"></el-radio-button>
              <el-radio-button label="3"></el-radio-button>
              <el-radio-button label="4"></el-radio-button>
              <el-radio-button label="5"></el-radio-button>
            </el-radio-group>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text" style="font-size: 15px">使用顺序</div>
            <el-radio-group size="small" v-model="planItem.drones.order">
              <el-radio-button label="换班前" value="pre"></el-radio-button>
              <el-radio-button label="换班后" value="post"></el-radio-button>
            </el-radio-group>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text">菲亚梅塔</div>
            <el-switch active-color="#13ce66" inactive-color="#c0c4cc" v-model="planItem.Fiammetta.enable"></el-switch>
            <el-input
              size="small"
              style="width: 128px"
              class="parameter_inputbox"
              placeholder="例如：巫恋"
              v-model="planItem.Fiammetta.target"
            ></el-input>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text" style="font-size: 15px">使用顺序</div>
            <el-radio-group size="small" v-model="planItem.Fiammetta.order">
              <el-radio-button label="换班前" value="pre"></el-radio-button>
              <el-radio-button label="换班后" value="post"></el-radio-button>
            </el-radio-group>
          </div>
        </div>
        <div
          class="riic_building building_cortrolCenter"
          v-for="(control, roomIndex) in planItem.rooms.control"
          :key="roomIndex"
        >
          <!-- <div class="riic_building_title">
            控制中枢
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="control.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
            <el-select   v-for="(control, index) in planItem.control" :key="index"
              v-model="control[index]"
              filterable
              placeholder="请选择"
              size="small"
            >
              <el-option
                v-for="item in char_CONTROL"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select> -->
          <!-- <div v-for="(operator, index) in control.operators"
              :key="index">
              {{operator}}
            </div> 
            <el-input
              class="operator_inputbox"
              size="small"
              v-for="(operator, operator_index) in control.operators"
              :key="operator_index"
              v-model="control.operators[operator_index]"
            >
            </el-input>
          </div>
        </div> -->

          <div class="riic_building building_trade" v-for="(trading, index) in planItem.trading" :key="index">
            <div class="riic_building_title">
              贸易站1
              <el-checkbox
                style="margin: 4px 0px 0px 0px; background-color: #ffffff80; vertical-align: top"
                size="mini"
                label="顺序入驻"
                border
                v-model="trading.sort"
              ></el-checkbox>
              <el-checkbox
                style="margin: 4px 0px 0px 0px; background-color: #ffffff80; vertical-align: top"
                size="mini"
                label="补满空位"
                border
                v-model="trading.autofill"
              ></el-checkbox>
              <el-checkbox
                style="margin: 4px 0px 0px 0px; background-color: #ffffff80; vertical-align: top"
                size="mini"
                label="跳过"
                border
                v-model="planItem.trading.skip"
              ></el-checkbox>
            </div>
            <div class="riic_building_operatorArray">
              <!-- <el-select v-for="(trading, index) in planItem.trading" :key="index"
              v-model="trading[index]"
              filterable
              placeholder="请选择"
              size="small"
            >
              <el-option
                v-for="item in char_TRADING"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select> -->
              <el-input
                size="small"
                v-for="(operators, index) in trading.operators"
                :key="index"
                v-model="operators[index]"
              >
              </el-input>
            </div>
            <div class="riic_building_parameter">
              <div class="parameter_text">产物</div>
              <el-radio-group size="small" v-model="trading.product">
                <el-radio-button label="龙门币"></el-radio-button>
                <el-radio-button label="合成玉"></el-radio-button>
              </el-radio-group>
            </div>
          </div>

          <!-- <div
          class="riic_building building_factory"
          v-for="(manufacture, index) in planItem.manufacture"
          :key="index"
        >
          <div class="riic_building_title">
            制造站1
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="顺序入驻"
              border
              v-model="manufacture.sort"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="补满空位"
              border
              v-model="manufacture.autofill"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="manufacture.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
             <el-select
              v-model="manufacture_plan0_0[0]"
              filterable
              placeholder="请选择"
              size="small"
            >
              <el-option
                v-for="item in char_MANUFACTURE"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select> 
            <el-input
              size="small"
              v-for="(operators, index) in manufacture.operators"
              :key="index"
              v-model="operators[index]"
            >
            </el-input>
          </div>
          <div class="riic_building_parameter">
            <div class="parameter_text">产物</div>
            <el-radio-group size="small" v-model="manufacture.product">
              <el-radio-button label="作战记录"></el-radio-button>
              <el-radio-button label="赤金"></el-radio-button>
              <el-radio-button label="源石碎片"></el-radio-button>
            </el-radio-group>
          </div>
        </div>-->

          <!-- <div
          class="riic_building building_powerPlant"
          v-for="(power, index) in planItem.power"
          :key="index"
        >
          <div class="riic_building_title">
            发电站1
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="补满空位"
              border
              v-model="power.autofill"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="power.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
             <el-select
              v-model="power_plan0_0[0]"
              filterable
              placeholder="请选择"
              size="small"
            >
              <el-option
                v-for="item in char_POWER"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select> 
            <el-input
              size="small"
              v-for="(operators, index) in power.operators"
              :key="index"
              v-model="operators[index]"
            >
            </el-input>
          </div>
        </div>-->

          <!-- <div
          class="riic_building building_meetingRoom"
          v-for="(meeting, index) in planItem.meeting"
          :key="index"
        >
          <div class="riic_building_title">
            会客室
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="补满空位"
              border
              v-model="meeting.autofill"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="meeting.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
            <el-input
              v-for="(operators, index) in meeting.operators"
              :key="index"
              class="operator_inputbox"
              size="small"
              v-model="operators[index]"
            ></el-input>
          </div>
        </div>
        <div
          class="riic_building building_hr"
          v-for="(hire, index) in planItem.hire"
          :key="index"
        >
          <div class="riic_building_title">
            办公室
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="补满空位"
              border
              v-model="hire.autofill"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="hire.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
            <el-input
              v-for="(operators, index) in hire.operators"
              :key="index"
              class="operator_inputbox"
              size="small"
              v-model="hire[index]"
            ></el-input>
          </div>
        </div>
        <div
          class="riic_building building_dormitory"
          v-for="(dormitory, index) in planItem.dormitory"
          :key="index"
        >
          <div class="riic_building_title">
            宿舍1
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="顺序入驻"
              border
              v-model="dormitory.sort"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="补满空位"
              border
              v-model="dormitory.autofill"
            ></el-checkbox>
            <el-checkbox
              style="
                margin: 4px 0px 0px 0px;
                background-color: #ffffff80;
                vertical-align: top;
              "
              size="mini"
              label="跳过"
              border
              v-model="dormitory.skip"
            ></el-checkbox>
          </div>
          <div class="riic_building_operatorArray">
            <el-input
              v-for="(operators, index) in dormitory.operators"
              :key="index"
              class="operator_inputbox"
              size="small"
              placeholder="1"
              v-model="operators[index]"
            ></el-input>
          </div>-->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import buildingApi from "@/api/building";
import cookie from "js-cookie";
import toolApi from "@/api/tool";
import TRADINGJson from "static/json/build/TRADING.json";
import MANUFACTUREJson from "static/json/build/MANUFACTURE.json";
import CONTROLJson from "static/json/build/CONTROL.json";
import POWERJson from "static/json/build/POWER.json";
import scheduleJson_formwork from "static/json/build/test.json";

export default {
  head: {
    title: "一图流-基建排班生成器",
    meta: [
      { charset: "utf-8" },
      {
        name: "viewport",
        content: "width=device-width, initial-scale=0.68, maximum-scale=0.68, user-scalable=no",
      },
      {
        hid: "description",
        name: "description",
        content: "一图流-基建排班生成器",
      },
    ],
    link: [{ rel: "icon", type: "image/x-icon", href: "/favicon.ico" }],
  },
  data() {
    return {
      scheduleJson: {
        author: "基建资本家",
        description: "测试标题",
        title: "基建资本家",
        buildingType: "243",
        planTimes: "3班",
        plans: [],
      },
      trading: [
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
      ],
      manufacture: [
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
      ],
      power: [
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
        ],
      ],
      dormitory: [
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
        [
          ["1", "2", "3"],
          ["4", "5", "6"],
          ["7", "8", "9"],
          ["7", "8", "9"],
          ["7", "8", "9"],
        ],
      ],
      meeting: [[], [], []],
      Fiammetta: [
        {
          enable: true,
          target: "巫恋",
          order: "pre",
        },
        {
          enable: true,
          target: "巫恋",
          order: "pre",
        },
        {
          enable: true,
          target: "巫恋",
          order: "pre",
        },
      ],
      drones: [
        {
          enable: true,
          index: 2,
          room: "manufacture",
          order: "pre",
        },
        {
          enable: true,
          index: 2,
          room: "manufacture",
          order: "pre",
        },
        {
          enable: true,
          index: 2,
          room: "manufacture",
          order: "pre",
        },
      ],
      period: [],
      plans: [],
    };
  },
  created() {
    this.initScheduleJson();
  },
  methods: {
    openNotification() {
      this.$notify({
        title: "2023-03-13更新",
        dangerouslyUseHTMLString: true,
        message:
          "<strong> 干员追加：<br>增加了落叶逐火活动干员 <br>BUG相关：  <br>由于存储服务重写的问题<br>如果遇到导入为空的情况可加Q群539600566</strong>",
        duration: 12000,
      });
    },
    initScheduleJson() {
      console.log(this.scheduleJson);
      for (var i = 0; i < 3; i++) {
        this.plans[i] = {
          rooms: {
            control: [],
            trading: [],
            manufacture: [],
            hire: [],
            power: [],
            meeting: [],
            dormitory: [],
          },
          Fiammetta: {
            enable: true,
            target: "但书",
            order: "pre",
          },
          drones: {
            enable: true,
            index: 2,
            room: "manufacture",
            order: "post",
          },
          period: [["0" + i + ":00", "00:00"]],
          name: "测试" + i,
          description: "测试描述" + i,
        };
        for (var t = 0; t < 2; t++) {
          var trading = {
            product: "LMD",
            operators: [i + "-t" + t, i + "-t" + t, i + "-t" + t],
            sort: true,
            autofill: false,
            skip: false,
          };
          this.plans[i].rooms.trading[t] = trading;
        }
        for (var m = 0; m < 4; m++) {
          var manufacture = {
            product: "LMD",
            operators: [i + "-m-" + m, i + "-m-" + m, i + "-m-" + m],
            sort: true,
            autofill: false,
            skip: false,
          };
          this.plans[i].rooms.manufacture[m] = manufacture;
        }
        for (var p = 0; p < 3; p++) {
          var power = {
            operators: [i + "-p-" + p, i + "-p-" + p, i + "-p-" + p],
            autofill: false,
            skip: false,
          };
          this.plans[i].rooms.power.push(power);
        }
        for (var d = 0; d < 5; d++) {
          var dormitory = {
            operators: [i + "-d-" + d, i + "-d-" + d, i + "-d-" + d, i + "-d-" + d, i + "-d-" + d],
            sort: true,
            autofill: false,
            skip: false,
          };
          this.plans[i].rooms.dormitory[d] = dormitory;
        }

        this.plans[i].rooms.control.push({
          operators: ["1", "2", "3", "4", "5"],
          skip: false,
        });

        this.plans[i].rooms.hire[0] = {
          operators: ["1"],
          autofill: false,
          skip: false,
        };

        this.plans[i].rooms.meeting[0] = {
          operators: ["2", "1"],
          autofill: false,
          skip: false,
        };
      }

      console.log(this.plans);
    },

    setScheduleJson(room) {},
  },
};
</script>

<style>
.el-checkbox--mini {
  border-radius: 16px !important;
}

#riic {
  background: rgb(255, 255, 255);
  margin: auto;
  max-width: 1500px;
}

#riic_controlPanel {
  /* margin-top: 16px; */
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

#riic_workerSets {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.riic_workerSet:nth-child(1) {
  border: 4px dotted #ff000080;
}

.riic_workerSet:nth-child(2) {
  border: 4px dotted #00ff0080;
}

.riic_workerSet:nth-child(3) {
  border: 4px dotted #0000ff80;
}

.riic_workerSet {
  /* width: 450px; */
  border-radius: 8px;
  font-size: 18px;
  /* box-shadow: 0px 2px 6px rgb(160 160 160 / 69%); */
  margin: 0px 12px 12px 12px;
  /* margin: auto; */
}

.riic_building {
  margin: 16px;
  margin-top: 36px;
  padding: 16px 0px 8px 0px;
  border: 4px solid rgba(128, 128, 128, 0.50196);
  border-radius: 16px;
  width: 410px;
}

.riic_building_title {
  position: absolute;
  background: #e0e0e0;
  margin: -36px -12px;
  padding: 0px 8px;
  font-size: 24px;
  border-left: 4px solid #80808080;
  border-right: 4px solid #80808080;
  border-radius: 4px;
  height: 36px;
  /* width: 400px; */
}

.building_trade .riic_building_title {
  border-left: 4px solid #000d8180;
  border-right: 4px solid #000d8180;
  background-color: #eeeeff;
}

.building_factory .riic_building_title {
  border-left: 4px solid #7f810080;
  border-right: 4px solid #7f810080;
  background-color: #ffffee;
}

.building_powerPlant .riic_building_title {
  border-left: 4px solid #00810080;
  border-right: 4px solid #00810080;
  background-color: #eeffee;
}

.building_powerPlant .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_powerPlant .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_powerPlant .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_powerPlant .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_powerPlant .riic_building_parameter {
  display: inline-block;
}

.building_hr .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_hr .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_hr .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_hr .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_hr .riic_building_parameter {
  display: inline-block;
}

.building_meetingRoom .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_meetingRoom .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_meetingRoom .riic_building_parameter div:nth-child(1) {
  display: none;
}

.building_meetingRoom .riic_building_parameter div:nth-child(2) {
  display: none;
}

.building_meetingRoom .riic_building_parameter {
  display: inline-block;
}

.riic_building_operatorArray {
  padding: 0px 20px;
  line-height: 40px;
  display: inline-block;
}

.riic_building_operator {
  /* background-color: rgb(23, 148, 90); */
  margin: 12px;
  display: inline-block;
}

.riic_building_parameter {
  /* background-color: rgb(110, 148, 23); */
  margin: 12px 12px 0px 12px;
  padding-left: 12px;
  /* display: inline-block; */
}

.parameter_text {
  width: 96px;
  display: inline-block;
}

.parameter_inputbox {
  width: 262px;
  display: inline-block;
  height: 20px;
  position: relative;
  top: -2px;
}

.operator_text {
  width: 20px;
  display: inline-block;
}

.operator_inputbox {
  width: 117px;
  display: inline-block;
  height: 20px;
  position: relative;
  top: -2px;
}

#riic_jsonData {
  margin-top: 24px;
  /* background-color: rgb(255, 255, 255); */
  width: 444px;
  height: 300px;
  overflow: auto;
}

.el-select {
  width: 116px;
}
</style>
