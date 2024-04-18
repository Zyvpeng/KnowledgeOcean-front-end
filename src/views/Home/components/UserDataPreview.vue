<template>
  <!-- 全国用户详情 -->
  <div class="user-data-preview">
    <!-- 左侧数据预览 -->
    <div class="data-overview left-layout">
      <div class="user-details">
        <p class="content-title">Blockchain Information</p>
        <ul>
          <li
            v-for="(item, index) in userData"
            :key="index"
            :class="[
              { allUser: item.id === 'allUser' },
              { allUserOpen: item.id === 'allUserOpen' },
              { allOpen: item.id === 'allOpen' },
              { allUserFace: item.id === 'allUserFace' },
            ]"
          >
            <div class="user-flop-box">
              <div>
                <span
                  :class="[
                    { 'flop-figure': !isNaN(ls) },
                    { 'flop-comma': isNaN(ls) },
                  ]"
                  v-for="(ls, index) in item.valueArr"
                  :key="item.id + index"
                >
                  <i v-if="!isNaN(ls)">0123456789</i>
                  <span v-else>{{ ls }}</span>
                </span>
              </div>
              <div>
                <img :src="item.type === 0 ? riseImage : declineImage" alt="" />
                <span :class="{ decline: item.type === 1 }">{{
                  item.typeValue
                }}</span>
              </div>
            </div>
            <div>{{ item.name }}</div>
          </li>
        </ul>
      </div>
      <div class="city-ranking">
        <p class="content-title">Summary of Transaction Information</p>
        <ScrollRankingBoard
          class="ranking"
          :config="rankingConfig"
        ></ScrollRankingBoard>
      </div>
    </div>

    <div class="live-status right-layout">
      <dv-border-box-8 class="status-details" :style="{ height: height }">
        <p class="content-title">Personal Center</p>
        <svg
          t="1662271516630"
          class="icon_"
          viewBox="0 0 1024 1024"
          version="1.1"
          xmlns="http://www.w3.org/2000/svg"
          p-id="4605"
          width="135"
          height="135"
          style="position: relative; top: 45px; left: 60px"
        >
<path d="M789.333333 791.893333C759.893333 751.146667 597.333333 682.666667 597.333333 682.666667h-170.666666s-162.56 68.48-192 109.226666A286.506667 286.506667 0 0 0 192 917.333333h640a286.506667 286.506667 0 0 0-42.666667-125.44z" fill="#CFD8DC" p-id="5282"></path><path d="M661.333333 813.226667c0-56.106667-31.786667-116.266667-42.666666-121.173334L597.333333 682.666667h-85.333333v234.666666l85.333333-85.333333-21.333333-42.666667zM362.666667 813.226667c0-56.106667 31.786667-116.266667 42.666666-121.173334l21.333334-9.386666h85.333333v234.666666l-85.333333-85.333333 21.333333-42.666667z" fill="#ECEFF1" p-id="5283"></path><path d="M743.253333 479.786667c-9.386667 42.666667-17.92 53.546667-44.8 53.546666a57.386667 57.386667 0 0 1-61.44-53.76 59.306667 59.306667 0 0 1 64-52.906666c41.813333 0 51.84 10.666667 42.24 53.12zM280.746667 479.786667c9.386667 42.666667 17.92 53.546667 44.8 53.546666a57.386667 57.386667 0 0 0 61.44-53.76 59.306667 59.306667 0 0 0-64-52.906666c-41.813333 0-51.84 10.666667-42.24 53.12z" fill="#FB8C00" p-id="5284"></path><path d="M426.666667 490.666667v192c0 64 85.333333 192 85.333333 192s85.333333-128 85.333333-192v-192z" fill="#1E88E5" p-id="5285"></path><path d="M426.666667 490.666667v192a79.573333 79.573333 0 0 0 85.333333 85.333333 79.36 79.36 0 0 0 85.333333-85.333333v-192z" fill="#FB8C00" p-id="5286"></path><path d="M298.666667 378.666667C298.666667 591.786667 413.44 682.666667 512 682.666667V192c-123.52 0-213.333333 106.666667-213.333333 186.666667zM725.333333 378.666667C725.333333 591.786667 610.56 682.666667 512 682.666667V192c123.52 0 213.333333 106.666667 213.333333 186.666667z" fill="#FFB74D" p-id="5287"></path><path d="M480.426667 548.48a86.4 86.4 0 0 0 64 0 33.28 33.28 0 0 1-64 0z" fill="#FCA230" p-id="5288"></path><path d="M458.666667 588.8a170.666667 170.666667 0 0 0 106.666666 0A75.093333 75.093333 0 0 1 512 618.666667a75.093333 75.093333 0 0 1-53.333333-29.866667z" fill="#FB8C00" p-id="5289"></path><path d="M704 490.666667s-106.666667 42.666667-149.333333 42.666666a51.2 51.2 0 0 0-42.666667-42.666666v-85.333334h192zM320 490.666667s106.666667 42.666667 149.333333 42.666666a51.2 51.2 0 0 1 42.666667-42.666666v-85.333334h-192z" fill="#FFD698" p-id="5290"></path><path d="M640 469.333333a66.773333 66.773333 0 0 1-42.666667 21.333334 66.773333 66.773333 0 0 1-42.666666-21.333334 66.773333 66.773333 0 0 1 42.666666-21.333333 66.773333 66.773333 0 0 1 42.666667 21.333333z" fill="#FFFFFF" p-id="5291"></path><path d="M597.333333 469.333333m-21.333333 0a21.333333 21.333333 0 1 0 42.666667 0 21.333333 21.333333 0 1 0-42.666667 0Z" fill="#3E2723" p-id="5292"></path><path d="M469.333333 469.333333a66.773333 66.773333 0 0 1-42.666666 21.333334 66.773333 66.773333 0 0 1-42.666667-21.333334 66.773333 66.773333 0 0 1 42.666667-21.333333 66.773333 66.773333 0 0 1 42.666666 21.333333z" fill="#FFFFFF" p-id="5293"></path><path d="M426.666667 469.333333m-21.333334 0a21.333333 21.333333 0 1 0 42.666667 0 21.333333 21.333333 0 1 0-42.666667 0Z" fill="#3E2723" p-id="5294"></path><path d="M320 384h384v21.333333H320z" fill="#37474F" p-id="5295"></path><path d="M512 106.666667c-117.76 0-234.666667 74.24-234.666667 192a725.333333 725.333333 0 0 0 33.493334 192 1135.786667 1135.786667 0 0 0 9.173333-128c195.626667 0 298.666667-106.666667 298.666667-106.666667s16 81.706667 85.333333 106.666667c4.266667 91.946667 8.96 128 8.96 128A887.68 887.68 0 0 0 746.666667 298.666667c0-85.333333-42.666667-149.333333-128-149.333334a139.733333 139.733333 0 0 0-106.666667-42.666666z" fill="#8D6E63" p-id="5296"></path><path d="M650.666667 435.2A174.506667 174.506667 0 0 0 597.333333 426.666667c-29.653333 0-42.666667 0-42.666666-10.666667s15.146667-10.666667 42.666666-10.666667a60.8 60.8 0 0 1 53.333334 29.866667zM373.333333 435.2A174.506667 174.506667 0 0 1 426.666667 426.666667c29.653333 0 42.666667 0 42.666666-10.666667s-15.146667-10.666667-42.666666-10.666667a60.8 60.8 0 0 0-53.333334 29.866667z" fill="#A1887F" p-id="5297"></path><path d="M746.666667 597.333333h85.333333v149.333334h-85.333333z" fill="#90CAF9" p-id="5298"></path><path d="M832 704h-85.333333l-81.706667 163.413333a34.56 34.56 0 0 0-3.626667 15.36A34.346667 34.346667 0 0 0 695.893333 917.333333h186.88A34.346667 34.346667 0 0 0 917.333333 882.773333a34.56 34.56 0 0 0-3.626666-15.36z" fill="#90CAF9" p-id="5299"></path><path d="M736 597.333333H789.333333v21.333334h-53.333333a10.666667 10.666667 0 0 1-10.666667-10.666667 10.666667 10.666667 0 0 1 10.666667-10.666667zM842.666667 618.666667H789.333333v-21.333334h53.333334a10.666667 10.666667 0 0 1 10.666666 10.666667 10.666667 10.666667 0 0 1-10.666666 10.666667z" fill="#42A5F5" p-id="5300"></path><path d="M853.333333 746.666667h-128l-60.373333 120.746666A34.56 34.56 0 0 0 695.893333 917.333333h186.88a34.56 34.56 0 0 0 30.933334-49.92z" fill="#D500F9" p-id="5301"></path><path d="M789.333333 874.666667h-85.333333l42.666667-128h42.666666v128z" fill="#EA80FC" p-id="5302"></path><path d="M789.333333 704h-30.506666L746.666667 746.666667h42.666666v-42.666667z" fill="#BBDEFB" p-id="5303"></path>

        </svg>
        <button
          class="bbtn"
          style="position: relative; top: -90px; left: 250px"
          @click="link()"
        >
          <svg
            height="24"
            width="24"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path d="M0 0h24v24H0z" fill="none"></path>
            <path
              d="M5 13c0-5.088 2.903-9.436 7-11.182C16.097 3.564 19 7.912 19 13c0 .823-.076 1.626-.22 2.403l1.94 1.832a.5.5 0 0 1 .095.603l-2.495 4.575a.5.5 0 0 1-.793.114l-2.234-2.234a1 1 0 0 0-.707-.293H9.414a1 1 0 0 0-.707.293l-2.234 2.234a.5.5 0 0 1-.793-.114l-2.495-4.575a.5.5 0 0 1 .095-.603l1.94-1.832C5.077 14.626 5 13.823 5 13zm1.476 6.696l.817-.817A3 3 0 0 1 9.414 18h5.172a3 3 0 0 1 2.121.879l.817.817.982-1.8-1.1-1.04a2 2 0 0 1-.593-1.82c.124-.664.187-1.345.187-2.036 0-3.87-1.995-7.3-5-8.96C8.995 5.7 7 9.13 7 13c0 .691.063 1.372.187 2.037a2 2 0 0 1-.593 1.82l-1.1 1.039.982 1.8zM12 13a2 2 0 1 1 0-4 2 2 0 0 1 0 4z"
              fill="currentColor"
            ></path>
          </svg>
          <span>Connect metamask</span>
        </button>
        <button
          class="btn"
          style="
            text-transform: uppercase;
            letter-spacing: 0.1em;
            padding: 10px 30px;
            transition: 0.5s;
            position: relative;
            left: 250px;
            top: -50px;
            font-size: 17px;
            background: #333;
            border: none;
            color: #fff;
          "
        >
          <span style="transform: translateY(-50px)">Search</span><i></i>
        </button>
      </dv-border-box-8>
      <dv-border-box-8 class="portrait-crowd" :style="{ height: height }">
        <p class="content-title">Knowledge Label</p>
        <div class="content">
          <div class="portrait">
            <div>
              <img src="../../../assets/范围广.png" alt="" />
              <p>theory</p>
              <p>{{ manRate }}</p>
            </div>
            <div>
              <img src="../../../assets/agora_大范围.png" alt="" />
              <p>application</p>
              <p>{{ womanRate }}</p>
            </div>
          </div>
          <div id="crowdCanvas"></div>
        </div>
      </dv-border-box-8>
      <dv-border-box-8 class="channel-proportion" :style="{ height: height }">
        <p class="content-title">Sort Of Knowledge</p>
        <div class="content">
          <div id="channelCanvas" style="width: 100%; height: 225px"></div>
        </div>
      </dv-border-box-8>
    </div>
  </div>
</template>

<script>
import {
  previewData,
  userRankingData,
  userStates,
  userPortraitData,
  userChannelData,
} from "../../../utils/jsonData";
import axios from "axios";


// 自定义排名轮播
import ScrollRankingBoard from "@/components/ScrollRankingBoard/src/index";
export default {
  name: "UserDataPreview",
  components: { ScrollRankingBoard },
  props: {
    riseImage: {
      type: String,
    },
    declineImage: {
      type: String,
    },
    height: {
      type: String,
      default: "",
    },
    fullscreen: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      message: {
        num: 111,
      },
      timer: null,
      numTimer: null,
      pointTimer: null,
      applyRate: 0,
      faceRate: 0,
      applyTotal: 0,
      faceTotal: 0,
      manRate: 0,
      womanRate: 0,
      userData: [],
      allUserNumCount: "",
      allCommunityCount: "",
      cityInfoList: [],
      rankingConfig: {
        data: [],
        formatter: true,
      },
    };
  },
  mounted() {
    // 获取全用户数据
    setTimeout(() => {
      // this.initUserMap()
      this.initUserDataPreview();
      this.initUserRanking();
      //this.initUserStates();
      this.initUserPortrait();
      this.initUserChannelCanvas();
    }, 1000);
  },
  methods: {
    // 用户数据概览
    initUserDataPreview() {
      var data;
      axios
        .get("http://localhost:8080/queryBlockInfo")
        .then((res) => {
          data = res.data.data;
          console.log(data);
          console.log(data.blockHeight);
          //alert(res.data)
          if (previewData.code !== 0) return;
          this.allUserNumCount = previewData.data.allUserNumCount;
          this.userData = [];
          this.userData.push(
            {
              id: "allUser",
              name: "BlockChain Height",
              value: data.blockHeight,
              valueArr: String(data.blockHeight).split(""),
              type: previewData.data.allUserNumUpType,
              typeValue: `${(
                +previewData.data.allUserNumPercentage * 100
              ).toFixed(1)}%`,
            },
            {
              id: "allUserOpen",
              name: "Transactions",
              value: data.tradingCapacity,
              valueArr: String(data.tradingCapacity).split(""),
              type: previewData.data.allUserOpenNumUpType,
              typeValue: `${(
                +previewData.data.allUserOpenNumPercentage * 100
              ).toFixed(1)}%`,
            },
            {
              id: "allOpen",
              name: "IPFS",
              value: this.$parent.formatter(previewData.data.allOpenNumCount),
              valueArr: this.$parent
                .formatter(previewData.data.allOpenNumCount)
                .split(""),
              type: previewData.data.allOpenNumUpType,
              typeValue: `${(
                +previewData.data.allOpenNumCountPercentage * 100
              ).toFixed(1)}%`,
            },
            {
              id: "allUserFace",
              name: "Users",
              value: this.$parent.formatter(
                previewData.data.allUserFaceOpenNumCount
              ),
              valueArr: this.$parent
                .formatter(previewData.data.allUserFaceOpenNumCount)
                .split(""),
              type: previewData.data.allUserFaceOpenNumCountUpType,
              typeValue: `${(
                +previewData.data.allUserFaceOpenNumCountPercentage * 100
              ).toFixed(1)}%`,
            }
          );
          setTimeout(() => {
            this.$parent.timedRefresh(this.userData, "user");
          }, 1000);
        })
        .catch((error) => {
          //console.log(123123213)
          console.log(error);
        });
    },
    // 用户开门用户城市排行
    initUserRanking() {
      if (userRankingData.code !== 0) return;
      let list = [];
      userRankingData.data.list &&
        userRankingData.data.list.length &&
        userRankingData.data.list.length > 0 &&
        userRankingData.data.list.forEach((t) => {
          list.push({
            name: t.cityName,
            value: t.userNumCount,
          });
        });
      this.rankingConfig = {
        data: list,
        formatter: true,
      };
    },
    // 用户实时状态
    initUserStates() {
      if (userStates.code !== 0) return;
      let applyData = [
        { value: userStates.data.openDoorApprovedCount, name: "申请通过" },
        {
          value:
            userStates.data.openDoorApplyCount -
            userStates.data.openDoorApprovedCount,
          name: "申请不通过",
        },
      ];
      let faceData = [
        { value: userStates.data.userFaceNumCount, name: "用户人脸数" },
        {
          value:
            userStates.data.userNumCount - userStates.data.userFaceNumCount,
          name: "非用户人脸数",
        },
      ];
      this.applyRate = `${(
        (userStates.data.openDoorApprovedCount /
          userStates.data.openDoorApplyCount) *
        100
      ).toFixed(2)}%`;
      this.faceRate = `${(
        (userStates.data.userFaceNumCount / userStates.data.userNumCount) *
        100
      ).toFixed(2)}%`;
      this.applyTotal = this.$parent.formatter(userStates.data.userAllNum);
      this.faceTotal = this.$parent.formatter(userStates.data.userFaceNumCount);
      this.initUserApply(applyData);
      this.initUserFaceRate(faceData);
    },
    // 用户通过率
    initUserApply(data) {
      let myChart = this.$echarts.init(document.getElementById("passRate"));
      myChart.setOption({
        series: [
          {
            name: "实时状态",
            type: "pie",
            radius: ["45%", "70%"],
            legendHoverLink: false,
            hoverAnimation: false,
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            itemStyle: {
              borderWidth: 2,
              borderColor: "rgba(16,16,21,0.4)",
            },
            color: ["#41A5FF", "#536382"],
            data: data,
          },
        ],
      });
    },
    // 用户人脸开通率
    initUserFaceRate(data) {
      let myChart = this.$echarts.init(document.getElementById("faceRate"));
      myChart.setOption({
        series: [
          {
            name: "实时状态",
            type: "pie",
            radius: ["45%", "70%"],
            legendHoverLink: false,
            hoverAnimation: false,
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            itemStyle: {
              borderWidth: 2,
              borderColor: "rgba(16,16,21,0.4)",
            },
            color: ["#41A5FF", "#536382"],
            data: data,
          },
        ],
      });
    },
    // 用户人群画像
    initUserPortrait() {
      var data, name;
      axios
        .get("http://localhost:8080/queryFlag")
        .then((res) => {
          console.log(222);
          data = Object.values(res.data.data);
          name = Object.keys(res.data.data);
          if (userPortraitData.code !== 0) return;
          this.manRate = `${(
            (userPortraitData.data.manNumCount /
              (userPortraitData.data.manNumCount +
                userPortraitData.data.womanNumCount)) *
            100
          ).toFixed(2)}%`;
          this.womanRate = `${(
            (userPortraitData.data.womanNumCount /
              (userPortraitData.data.manNumCount +
                userPortraitData.data.womanNumCount)) *
            100
          ).toFixed(2)}%`;
          let optionData = [];
          for (let i = 0; i < data.length; i++) {
            optionData = optionData.concat([
              { value: data[i] || 0, name: name[i] },
            ]);
          }
          let myChart = this.$echarts.init(
            document.getElementById("crowdCanvas")
          );
          myChart.clear();
          let option = {
            tooltip: {
              trigger: "item",
              // formatter: '{a} <br/>{b} : {c} ({d}%)',
              backgroundColor: "rgba(74, 144, 226, 0.84)",
              formatter: (params) => {
                return `<div>${params.seriesName} <br> ${
                  params.data.name
                }：${this.$parent.formatter(params.data.value)} (${
                  params.percent
                }%)</div>`;
              },
            },
            legend: {
              orient: "vertical",
              right: 10,
              bottom: 20,
              textStyle: {
                color: "#999",
                padding: [3, 0, 0, 3],
              },
              itemWidth: 10,
              itemHeight: 10,
              selectedMode: false,
              icon: "circle",
              data: name,
            },
            series: [
              {
                name: "Knowledge flag",
                type: "pie",
                radius: "55%",
                left: "-20%",
                top: -70,
                center: ["50%", "60%"],
                itemStyle: {
                  borderWidth: 2,
                  borderColor: "rgba(16,16,21,0.4)",
                  normal: {
                    label: {
                      show: false,
                    },
                    labelLine: {
                      show: false,
                    },
                  },
                },
                color: ["#61AECF", "#E8684A", "#F6BD16", "#5D7092", "#5AD8A6"],
                data: optionData,
              },
            ],
          };
          myChart.setOption(option);
          this.$LoopShowTooltip(myChart, option, {
            loopSeries: true,
            interval: 4000,
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    // 用户开门渠道占比
    initUserChannelCanvas() {
      var data, name;
      axios
        .get("http://localhost:8080/querySort")
        .then((res) => {
          console.log(222);
          data = Object.values(res.data.data);
          name = Object.keys(res.data.data);
          if (userChannelData.code !== 0) return;
          let optionData = [];
          for (let i = 0; i < data.length; i++) {
            optionData = optionData.concat([
              { value: data[i] || 0, name: name[i] },
            ]);
          }
          let myChart = this.$echarts.init(
            document.getElementById("channelCanvas")
          );
          let option = {
            tooltip: {
              trigger: "item",
              // formatter: '{a} <br/>{b}: {c} ({d}%)',
              backgroundColor: "rgba(74, 144, 226, 0.84)",
              formatter: (params) => {
                return `<div>${params.seriesName} <br> ${
                  params.data.name
                }：${this.$parent.formatter(params.data.value)} (${
                  params.percent
                }%)</div>`;
              },
            },
            legend: {
              orient: "vertical",
              right: 10,
              bottom: 20,
              textStyle: {
                color: "#999",
                padding: [2, 0, 0, 3],
              },
              itemWidth: 10,
              itemHeight: 10,
              icon: "circle",
              selectedMode: false,
              data: name,
            },

            series: [
              {
                name: "Knowledge sort",
                type: "pie",
                left: "-22%",
                top: "3%",
                radius: ["50%", "70%"],
                avoidLabelOverlap: false,
                label: {
                  show: false,
                  position: "center",
                },
                labelLine: {
                  show: true,
                },
                itemStyle: {
                  borderWidth: 2,
                  borderColor: "rgba(16,16,21,0.4)",
                },
                emphasis: {
                  label: {
                    color: "#4a90e2",
                    show: true,
                    fontSize: "14",
                    lineHeight: 22,
                    formatter: "{d}% \n {b}",
                  },
                },
                color: [
                  "#60ADCD",
                  "#517EDA",
                  "#C95C45",
                  "#F6BD16",
                  "#51BC93",
                  "#536382",
                ],
                data: optionData,
              },
            ],
          };
          myChart.setOption(option);
          this.$LoopShowTooltip(myChart, option, {
            loopSeries: true,
            interval: 4000,
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    link() {
      console.log(1);
      //web3前面必须有window
      // const Web3 = require("./getWeb3");
      //
      // Web3.default.getWeb3().then((res) => {
      //   window.web3 = res;
      //   console.log("getWeb3", res);
      // });
      if (window.ethereum) {
        window.ethereum.enable().then((res) => {
          alert("Current wallet address:" + res[0]);
        });
      } else {
        alert( "Please install metamask Wallet");
      }

    },
  },
};
</script>

<style>
/* From uiverse.io by @adamgiebl */
/* From uiverse.io by @mrhyddenn */
.btn {
  text-transform: uppercase;
  letter-spacing: 0.1em;
  padding: 10px 30px;
  transition: 0.5s;
  position: relative;
  font-size: 17px;
  background: #333;
  border: none;
  color: #fff;
}

.btn:hover {
  letter-spacing: 0.25em;
  background: #ff1867;
  color: #ff1867;
  box-shadow: 0 0 45px #ff1867;
}

.btn::before {
  content: "";
  position: absolute;
  inset: 2px;
  background: #222222;
}

.btn span {
  position: relative;
  top: -5px;
  z-index: 1;
}

.btn i {
  position: absolute;
  inset: 0;
  display: block;
}

.btn i::before {
  content: "";
  position: absolute;
  border: 2px solid #ff1867;
  width: 7px;
  height: 4px;
  top: -3.5px;
  left: 80%;
  background: #222222;
  transform: translateX(-50%);
  transition: 0.5s;
}

.btn:hover i::before {
  width: 20px;
  left: 20%;
}

.btn i::after {
  content: "";
  position: absolute;
  border: 2px solid #ff1867;
  width: 7px;
  height: 4px;
  bottom: -3.5px;
  left: 20%;
  background: #222222;
  transform: translateX(-50%);
  transition: 0.5s;
}

.btn:hover i::after {
  width: 20px;
  left: 80%;
}
.bbtn {
  display: flex;
  align-items: center;
  font-family: inherit;
  font-weight: 500;
  font-size: 17px;
  padding: 0.8em 1.3em 0.8em 0.9em;
  color: white;
  background: #ad5389;
  background: linear-gradient(to right, #0f0c29, #302b63, #24243e);
  border: none;
  letter-spacing: 0.05em;
  border-radius: 16px;
}

.bbtn svg {
  margin-right: 3px;
  transform: rotate(30deg);
  transition: transform 0.5s cubic-bezier(0.76, 0, 0.24, 1);
}

.bbtn span {
  transition: transform 0.5s cubic-bezier(0.76, 0, 0.24, 1);
}

.bbtn:hover svg {
  transform: translateX(5px) rotate(90deg);
}

.bbtn:hover span {
  transform: translateX(7px);
}
</style>