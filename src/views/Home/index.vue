<template>
  <div class="home-container">
    <!-- loading -->
    <div class="loading" v-if="isLoading">
      <dv-loading>Loading...</dv-loading>
    </div>
    <!-- 头部 -->
    <header>
      <div class="header-back">
        <div>
          <img class="img" src="@/assets/home_back.png" alt="" />
        </div>
<!--        <img-->
<!--          class="full-img"-->
<!--          @click="showFullScreen"-->
<!--          src="@/assets/img.png"-->
<!--          alt=""-->
<!--        />-->
      </div>
      <button style="transform: translateX(7rem)"><a href="http://127.0.0.1:8080/uploadPage">
        Share Resources</a> </button>

      <div
        class="group animate__animated animate__fadeInDownBig"
        v-show="isshow"
      >
        <svg class="icon" aria-hidden="true" viewBox="0 0 24 24">
          <g>
            <path
              d="M21.53 20.47l-3.66-3.66C19.195 15.24 20 13.214 20 11c0-4.97-4.03-9-9-9s-9 4.03-9 9 4.03 9 9 9c2.215 0 4.24-.804 5.808-2.13l3.66 3.66c.147.146.34.22.53.22s.385-.073.53-.22c.295-.293.295-.767.002-1.06zM3.5 11c0-4.135 3.365-7.5 7.5-7.5s7.5 3.365 7.5 7.5-3.365 7.5-7.5 7.5-7.5-3.365-7.5-7.5z"
            ></path>
          </g>
        </svg>
        <input
          placeholder="Search"
          type="search"
          class="input"
          v-model="username"
          @keyup.enter="search()"
        />
      </div>

      <div class="header-city">
        <p style="transform: translateX(-100px)">Knowledge Ocean</p>
        <dv-decoration-5
          style="width: 300px; height: 40px; transform: translateX(-100px)"
        />
      </div>
      <div class="right">
        <div style="width: 75px">
          <p
            :class="{ active: active === 2 }"
            @click="handleChangeType(2)"
            style="width: 100%"
          >
            graph
          </p>
        </div>
        <dv-decoration-3 style="width: 150px; height: 30px" />
      </div>
    </header>
    <dv-decoration-10 style="width: 100%; height: 5px" />

    <section ref="sectionRef">
      <!-- 地图 -->
      <div id="3d-graph" ref="graph"></div>
      <keep-alive>
        <UserDataPreview
          ref="user"
          :height="height"
          :fullscreen="fullscreen"
          v-if="active === 2"
          :riseImage="riseImage"
          :declineImage="declineImage"
        ></UserDataPreview>
        <DeviceDataPreview
          v-if="active === 1"
          :height="height"
          :fullscreen="fullscreen"
          :riseImage="riseImage"
          :declineImage="declineImage"
        ></DeviceDataPreview>
      </keep-alive>
    </section>

    <!-- 气泡详情 -->
    <div ref="popover" class="city-popover">
      <p class="popover-title">区域： {{ bubbleData.cityName }}</p>
      <div>
        <div>
          <canvas id="cityPopover" width="120px" height="120px"></canvas>
        </div>
        <div>
          <p>系统用户 &nbsp; {{ bubbleData.openUserCount }}</p>
          <p>灾害信息统计 &nbsp; {{ bubbleData.iotDoorControlCount }}</p>
          <p>社区总数 &nbsp; {{ bubbleData.communityCount }}</p>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import axios from 'axios'
import "animate.css";
import { Scene, PointLayer, Popup } from "@antv/l7";
import { GaodeMap } from "@antv/l7-maps";
import UserDataPreview from "./components/UserDataPreview";
import { cityData } from "../../utils/jsonData";
import ForceGraph3D from "3d-force-graph";
import {CSS2DObject,CSS2DRenderer} from "three/examples/jsm/renderers/CSS2DRenderer.js";
import * as THREE from "three";
//  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";



// import  three from three.
export default {
  name: "DataPreview",
  components: { UserDataPreview },
  data() {
    return {
      username: "",
      isshow: true,
      myGraph: this.myGraph,
      active: 2,
      isLoading: false,
      fullscreen: false,
      height: "",
      cityInfoList: [],
      updateTime: "",
      nodedata:{
        nodes: [
          { id: "BasicKnowledge", group: 1 },
          { id: "PHP", group: 1 },
          { id: "Java", group: 1 },
          { id: "Design", group: 1 },
          { id: "Testing technique", group: 1 },
          { id: "Physics", group: 1 },
          { id: "Application", group: 1 },
          { id: "Software", group: 1 },
          { id: "Count", group: 1 },
          { id: "C", group: 1 },
          { id: "C++", group: 2 },
          { id: "computer", group: 2 },
          // { id: "computer", group: 3 },
          { id: "Data", group: 2 },
          { id: "C#", group: 2 },
          // { id: "Computer", group: 2 },
          { id: "Maths", group: 3 },
          { id: "NetWork", group: 3 },
          { id: "Windows", group: 3 },
          { id: "Voice Networks", group: 3 },
          { id: "Favourite", group: 3 },
          { id: "Unbuntu", group: 3 },
          { id: "data structure", group: 3 },
          { id: "Knowledege", group: 3 },
          { id: "python", group: 4 },
          { id: "Thenardier", group: 4 },
          { id: "Cosette", group: 5 },
          { id: "Javert", group: 4 },
          { id: "Servlet", group: 0 },
          { id: "Jsp", group: 2 },
          { id: "Cookie", group: 3 },
          { id: "Session", group: 2 },
          { id: "listener", group: 2 },
          { id: "filter", group: 2 },
          { id: "Judge", group: 2 },
          { id: "Interceptor", group: 2 },
          { id: "mysql", group: 2 },
          { id: "moncomputerdb", group: 2 },
          { id: "postsql", group: 2 },
          { id: "mybatis", group: 4 },
          { id: "mybatisplus", group: 6 },
          { id: "Jpa", group: 4 },
          { id: "xml", group: 4 },
          { id: "spring", group: 5 },
          { id: "springMVC", group: 0 },
          { id: "springboot", group: 0 },
          { id: "springcloud", group: 7 },
          { id: "springsecurity", group: 7 },
          { id: "JWT", group: 8 },
          { id: "Redis", group: 5 },
          { id: "Token", group: 5 },
          { id: "UML", group: 5 },
          { id: "front-end", group: 5 },
          { id: "back-end", group: 5 },
          { id: "HTML", group: 5 },
          { id: "CSS", group: 8 },
          { id: "Js", group: 5 },
          { id: "ajax", group: 8 },
          { id: "axios", group: 8 },
          { id: "vue", group: 8 },
          { id: "webpack", group: 8 },
          { id: "nginx", group: 8 },
          { id: "RV", group: 8 },
          { id: "ComputerCV", group: 8 },
          { id: "MachineLearing", group: 8 },
          { id: "ML", group: 8 },
          { id: "Matlab", group: 8 },
          { id: "Open-source", group: 9 },
          { id: "AI", group: 4 },
          { id: "SQL", group: 4 },
          { id: "secret", group: 4 },
          { id: "encode", group: 4 },
          { id: "decode", group: 5 },
           { id: "blockchain", group: 10 },
           { id: "web3", group: 10 },
          { id: "ipfs", group: 4 },
          { id: "SW", group: 8 },
        ],
        links: [
          { source: "PHP", target: "BasicKnowledge", value: 1 },
          { source: "Java", target: "BasicKnowledge", value: 8 },
          { source: "Design", target: "BasicKnowledge", value: 10 },
          { source: "Design", target: "Java", value: 6 },
          { source: "Testing technique", target: "BasicKnowledge", value: 1 },
          { source: "Physics", target: "BasicKnowledge", value: 1 },
          { source: "Application", target: "BasicKnowledge", value: 1 },
          { source: "Software", target: "BasicKnowledge", value: 1 },
          { source: "Count", target: "BasicKnowledge", value: 2 },
          { source: "C", target: "BasicKnowledge", value: 1 },
          { source: "computer", target: "C++", value: 1 },
          { source: "computer", target: "Design", value: 3 },
          { source: "computer", target: "Java", value: 3 },
          { source: "computer", target: "BasicKnowledge", value: 5 },
          { source: "computer", target: "computer", value: 1 },
          { source: "Data", target: "computer", value: 1 },
          { source: "C#", target: "computer", value: 1 },
          { source: "computer", target: "computer", value: 1 },
          { source: "NetWork", target: "Maths", value: 4 },
          { source: "Windows", target: "Maths", value: 4 },
          { source: "Windows", target: "NetWork", value: 4 },
          { source: "Voice Networks", target: "Maths", value: 4 },
          { source: "Voice Networks", target: "NetWork", value: 4 },
          { source: "Voice Networks", target: "Windows", value: 4 },
          { source: "Favourite", target: "Maths", value: 3 },
          { source: "Favourite", target: "NetWork", value: 3 },
          { source: "Favourite", target: "Windows", value: 3 },
          { source: "Favourite", target: "Voice Networks", value: 4 },
          { source: "Unbuntu", target: "Maths", value: 3 },
          { source: "Unbuntu", target: "NetWork", value: 3 },
          { source: "Unbuntu", target: "Windows", value: 3 },
          { source: "Unbuntu", target: "Voice Networks", value: 3 },
          { source: "Unbuntu", target: "Favourite", value: 5 },
          { source: "data structure", target: "Maths", value: 3 },
          { source: "data structure", target: "NetWork", value: 3 },
          { source: "data structure", target: "Windows", value: 3 },
          { source: "data structure", target: "Voice Networks", value: 3 },
          { source: "data structure", target: "Favourite", value: 4 },
          { source: "data structure", target: "Unbuntu", value: 4 },
          { source: "Knowledege", target: "Maths", value: 3 },
          { source: "Knowledege", target: "NetWork", value: 3 },
          { source: "Knowledege", target: "Windows", value: 3 },
          { source: "Knowledege", target: "Voice Networks", value: 3 },
          { source: "Knowledege", target: "Favourite", value: 4 },
          { source: "Knowledege", target: "Unbuntu", value: 4 },
          { source: "Knowledege", target: "data structure", value: 4 },
          { source: "Knowledege", target: "computer", value: 2 },
          { source: "Knowledege", target: "computer", value: 9 },
          { source: "python", target: "Knowledege", value: 2 },
          { source: "python", target: "computer", value: 7 },
          { source: "Thenardier", target: "python", value: 13 },
          { source: "Thenardier", target: "Knowledege", value: 1 },
          { source: "Thenardier", target: "computer", value: 12 },
          { source: "Cosette", target: "python", value: 4 },
          { source: "Cosette", target: "computer", value: 31 },
          { source: "Cosette", target: "Maths", value: 1 },
          { source: "Cosette", target: "Thenardier", value: 1 },
          { source: "Javert", target: "computer", value: 17 },
          { source: "Javert", target: "Knowledege", value: 5 },
          { source: "Javert", target: "Thenardier", value: 5 },
          { source: "Javert", target: "python", value: 1 },
          { source: "Javert", target: "Cosette", value: 1 },
          { source: "Servlet", target: "computer", value: 8 },
          { source: "Servlet", target: "Javert", value: 1 },
          { source: "Jsp", target: "Knowledege", value: 1 },
          { source: "Jsp", target: "Javert", value: 1 },
          { source: "Jsp", target: "computer", value: 2 },
          { source: "Cookie", target: "Knowledege", value: 1 },
          { source: "Session", target: "Cookie", value: 2 },
          { source: "Session", target: "computer", value: 3 },
          { source: "Session", target: "Knowledege", value: 2 },
          { source: "Session", target: "Javert", value: 1 },
          { source: "listener", target: "computer", value: 1 },
          { source: "filter", target: "computer", value: 2 },
          { source: "filter", target: "Javert", value: 1 },
          { source: "Judge", target: "computer", value: 3 },
          { source: "Judge", target: "Jsp", value: 2 },
          { source: "Interceptor", target: "computer", value: 3 },
          { source: "Interceptor", target: "Judge", value: 3 },
          { source: "Interceptor", target: "Jsp", value: 2 },
          { source: "mysql", target: "Judge", value: 2 },
          { source: "mysql", target: "Interceptor", value: 2 },
          { source: "mysql", target: "computer", value: 2 },
          { source: "mysql", target: "Jsp", value: 1 },
          { source: "moncomputerdb", target: "Judge", value: 2 },
          { source: "moncomputerdb", target: "Interceptor", value: 2 },
          { source: "moncomputerdb", target: "mysql", value: 2 },
          { source: "moncomputerdb", target: "computer", value: 2 },
          { source: "moncomputerdb", target: "Jsp", value: 1 },
          { source: "postsql", target: "Judge", value: 2 },
          { source: "postsql", target: "Interceptor", value: 2 },
          { source: "postsql", target: "mysql", value: 2 },
          { source: "postsql", target: "moncomputerdb", value: 2 },
          { source: "postsql", target: "computer", value: 2 },
          { source: "postsql", target: "Jsp", value: 1 },
          { source: "mybatis", target: "Thenardier", value: 1 },
          { source: "mybatisplus", target: "Thenardier", value: 1 },
          { source: "Jpa", target: "python", value: 2 },
          { source: "Jpa", target: "Thenardier", value: 3 },
          { source: "xml", target: "Jpa", value: 2 },
          { source: "xml", target: "Thenardier", value: 2 },
          { source: "xml", target: "python", value: 1 },
          { source: "spring", target: "computer", value: 3 },
          { source: "spring", target: "Cosette", value: 1 },
          { source: "spring", target: "Javert", value: 1 },
          { source: "springMVC", target: "Servlet", value: 3 },
          { source: "springMVC", target: "computer", value: 1 },
          { source: "springboot", target: "Servlet", value: 2 },
          { source: "springsecurity", target: "springcloud", value: 1 },
          { source: "JWT", target: "springsecurity", value: 2 },
          { source: "JWT", target: "Thenardier", value: 1 },
          { source: "JWT", target: "Javert", value: 1 },
          { source: "JWT", target: "computer", value: 1 },
          { source: "Redis", target: "Cosette", value: 3 },
          { source: "Redis", target: "computer", value: 2 },
          { source: "Token", target: "Redis", value: 1 },
          { source: "Token", target: "python", value: 1 },
          { source: "UML", target: "Redis", value: 9 },
          { source: "UML", target: "Cosette", value: 2 },
          { source: "UML", target: "computer", value: 2 },
          { source: "front-end", target: "UML", value: 1 },
          { source: "front-end", target: "mybatis", value: 1 },
          { source: "back-end", target: "UML", value: 1 },
          {
            source: "HTML",
            target: "UML",
            value: 2,
          },
          { source: "HTML", target: "Redis", value: 1 },
          { source: "HTML", target: "Cosette", value: 1 },
          { source: "CSS", target: "UML", value: 6 },
          { source: "CSS", target: "Redis", value: 12 },
          { source: "CSS", target: "mybatis", value: 1 },
          { source: "front-end", target: "HTML", value: 1 },
          { source: "CSS", target: "Cosette", value: 21 },
          { source: "CSS", target: "computer", value: 19 },
          { source: "CSS", target: "Maths", value: 1 },
          { source: "CSS", target: "Thenardier", value: 2 },
          { source: "CSS", target: "Jpa", value: 5 },
          { source: "CSS", target: "JWT", value: 4 },
          { source: "Js", target: "Redis", value: 1 },
          { source: "Js", target: "CSS", value: 1 },
          { source: "ajax", target: "CSS", value: 1 },
          { source: "ajax", target: "Jpa", value: 1 },
          { source: "ajax", target: "JWT", value: 1 },
          { source: "axios", target: "CSS", value: 7 },
          { source: "axios", target: "JWT", value: 7 },
          { source: "axios", target: "Javert", value: 6 },
          { source: "axios", target: "ajax", value: 1 },
          { source: "axios", target: "computer", value: 4 },
          { source: "vue", target: "axios", value: 15 },
          { source: "vue", target: "CSS", value: 5 },
          { source: "vue", target: "JWT", value: 6 },
          { source: "vue", target: "ajax", value: 2 },
          { source: "webpack", target: "JWT", value: 1 },
          { source: "webpack", target: "axios", value: 4 },
          { source: "webpack", target: "vue", value: 2 },
          { source: "nginx", target: "JWT", value: 2 },
          { source: "nginx", target: "axios", value: 6 },
          { source: "nginx", target: "webpack", value: 2 },
          { source: "nginx", target: "vue", value: 5 },
          { source: "nginx", target: "ajax", value: 1 },
          { source: "nginx", target: "CSS", value: 1 },
          { source: "RV", target: "CSS", value: 9 },
          { source: "RV", target: "axios", value: 17 },
          { source: "RV", target: "vue", value: 13 },
          { source: "RV", target: "JWT", value: 7 },
          { source: "RV", target: "ajax", value: 2 },
          { source: "RV", target: "Jpa", value: 1 },
          { source: "RV", target: "nginx", value: 6 },
          { source: "RV", target: "webpack", value: 3 },
          { source: "back-end", target: "vue", value: 5 },
          { source: "back-end", target: "JWT", value: 5 },
          { source: "back-end", target: "RV", value: 6 },
          { source: "back-end", target: "ajax", value: 2 },
          { source: "back-end", target: "axios", value: 4 },
          { source: "back-end", target: "nginx", value: 3 },
          { source: "back-end", target: "webpack", value: 2 },
          { source: "front-end", target: "CSS", value: 1 },
          { source: "back-end", target: "CSS", value: 5 },
          { source: "MachineLearing", target: "RV", value: 12 },
          { source: "back-end", target: "JWT", value: 5 },
          { source: "MachineLearing", target: "ComputerCV", value: 4 },
          { source: "front-end", target: "axios", value: 10 },
          { source: "front-end", target: "nginx", value: 6 },
          { source: "MachineLearing", target: "webpack", value: 2 },
          { source: "front-end", target: "vue", value: 9 },
          { source: "front-end", target: "ajax", value: 1 },
          { source: "MachineLearing", target: "computer", value: 1 },
          { source: "ML", target: "ComputerCV", value: 5 },
          { source: "ML", target: "MachineLearing", value: 7 },
          { source: "ML", target: "JWT", value: 3 },
          { source: "ML", target: "RV", value: 5 },
          { source: "ML", target: "axios", value: 5 },
          { source: "ML", target: "nginx", value: 5 },
          { source: "ML", target: "webpack", value: 2 },
          { source: "ML", target: "vue", value: 5 },
          { source: "ML", target: "ajax", value: 1 },
          { source: "ML", target: "CSS", value: 2 },
          { source: "Matlab", target: "MachineLearing", value: 3 },
          { source: "Matlab", target: "axios", value: 3 },
          { source: "Matlab", target: "vue", value: 1 },
          { source: "Matlab", target: "RV", value: 2 },
          { source: "Matlab", target: "ML", value: 2 },
          { source: "Matlab", target: "JWT", value: 1 },
          { source: "Matlab", target: "ComputerCV", value: 1 },
          { source: "Matlab", target: "nginx", value: 1 },
          { source: "Matlab", target: "webpack", value: 1 },
          { source: "Open-source", target: "ajax", value: 3 },
          { source: "AI", target: "Thenardier", value: 5 },
          { source: "AI", target: "computer", value: 1 },
          { source: "AI", target: "python", value: 1 },
          { source: "AI", target: "Javert", value: 1 },
          { source: "AI", target: "JWT", value: 1 },
          { source: "AI", target: "Jpa", value: 1 },
          { source: "SQL", target: "Thenardier", value: 6 },
          { source: "SQL", target: "AI", value: 6 },
          { source: "SQL", target: "computer", value: 1 },
          { source: "SQL", target: "python", value: 1 },
          { source: "SQL", target: "Javert", value: 2 },
          { source: "SQL", target: "JWT", value: 1 },
          { source: "SQL", target: "Jpa", value: 1 },
          { source: "secret", target: "Thenardier", value: 4 },
          { source: "secret", target: "SQL", value: 4 },
          { source: "secret", target: "AI", value: 4 },
          { source: "secret", target: "computer", value: 1 },
          { source: "secret", target: "python", value: 1 },
          { source: "secret", target: "Javert", value: 1 },
          { source: "secret", target: "Jpa", value: 1 },
          { source: "secret", target: "axios", value: 1 },
          { source: "encode", target: "Javert", value: 1 },
          { source: "encode", target: "SQL", value: 2 },
          { source: "encode", target: "AI", value: 2 },
          { source: "encode", target: "secret", value: 2 },
          { source: "encode", target: "computer", value: 1 },
          { source: "encode", target: "JWT", value: 1 },
          { source: "encode", target: "Jpa", value: 1 },
          { source: "encode", target: "Thenardier", value: 1 },
          { source: "decode", target: "Cosette", value: 2 },
          { source: "decode", target: "Javert", value: 1 },
          { source: "decode", target: "computer", value: 1 },
          { source: "blockchain", target: "JWT", value: 2 },
          { source: "web3", target: "JWT", value: 2 },
          { source: "web3", target: "blockchain", value: 3 },
          { source: "ipfs", target: "SQL", value: 3 },
          { source: "ipfs", target: "AI", value: 3 },
          { source: "ipfs", target: "Thenardier", value: 3 },
          { source: "ipfs", target: "JWT", value: 1 },
          { source: "ipfs", target: "Jpa", value: 1 },
          { source: "ipfs", target: "secret", value: 1 },
          { source: "ipfs", target: "encode", value: 1 },
          { source: "SW", target: "MachineLearing", value: 1 },
          { source: "SW", target: "ML", value: 1 },
          { source: "SW", target: "Matlab", value: 1 },
          { source: "SW", target: "ComputerCV", value: 1 },
          { source: "SW", target: "RV", value: 1 },
          { source: "SW", target: "JWT", value: 1 },
          { source: "SW", target: "axios", value: 1 },
        ],

      },
      riseImage: require("@/assets/rise.png"),
      declineImage: require("@/assets/decline.png"),
      bubbleData: {
        cityCode: "",
        cityName: "",
        proportion: "",
        communityCount: "",
        openUserCount: "",
        iotDoorControlCount: "",
      },
    };
  },
  mounted() {

    this.initUserMap();
    this.showLoading();
    this.setElementHeight();
    this.initGraph();

      window.addEventListener("resize", this.setElementHeight);
      this.$once("hook:beforeDestroy", () => {
        window.removeEventListener("resize", this.setElementHeight);
      });



  },
  methods: {
    // 用户地图
    initUserMap() {
      const scene = new Scene({
        id: "userMap",
        map: new GaodeMap({
          pitch: 0,
          style: "dark",
          mapStyle: "amap://styles/darkblue",
          center: [113.631419, 34.753439],
          zoom: 5,
          token: "ced0d726cd96553ba8b5b3521671aaf4",
        }),
      });
      scene.on("loaded", () => {
        if (cityData.code !== 0) return;
        this.allCommunityCount = cityData.data.communityCount;
        this.cityInfoList = [];
        this.cityInfoList.push(
          {
            id: "u-iotdoor",
            name: "英德市灾害信息总量",
            value: this.formatter(cityData.data.iotdoorControlCount),
            valueArr: this.formatter(cityData.data.iotdoorControlCount).split(
              ""
            ),
            type: cityData.data.iotdoorControlCountUpType,
            percentage: `${(
              cityData.data.iotdoorControlCountPercentage * 100
            ).toFixed(1)} %`,
          },
          {
            id: "u-community",
            name: "英德市核心城镇",
            value: this.formatter(cityData.data.cityCount),
            valueArr: this.formatter(cityData.data.cityCount).split(""),
            type: cityData.data.cityCountUpType,
            percentage: `${(cityData.data.cityCountPercentage * 100).toFixed(
              1
            )}%`,
          },
          {
            id: "u-city",
            name: "英德市住宅社区",
            value: this.formatter(cityData.data.communityCount),
            valueArr: this.formatter(cityData.data.communityCount).split(""),
            type: cityData.data.communityCountUpType,
            percentage: `${(
              cityData.data.communityCountPercentage * 100
            ).toFixed(1)}%`,
          }
        );
        this.timedRefresh(this.cityInfoList, "city");
        cityData.data.cityList.sort((a, b) => {
          return a.communityCount - b.communityCount;
        });
        cityData.data.cityList.forEach((t, index) => {
          if (t.communityCount === 1) {
            t.count = 100;
          } else {
            t.count = 101 + index * 0.1;
          }
        });
        const pointLayer = new PointLayer({ zIndex: 1 })
          .source(cityData.data.cityList, {
            parser: {
              type: "json",
              x: "longitude",
              y: "latitude",
            },
          })
          .shape("circle")
          .animate(true)
          .size("count", [0, 45])
          .color("count", ["#3CA0FF", "#3CA0FF", "#3CA0FF"])
          .active(true)
          .style({
            opacity: 0.5,
            strokeWidth: 0,
          });
        pointLayer.on("mousemove", (e) => {
          clearTimeout(this.pointTimer);
          this.pointTimer = setTimeout(() => {
            const iotPercentage =
              (
                e.feature.iotDoorControlCount /
                cityData.data.iotdoorControlCount
              ).toFixed(2) > 0.001
                ? (
                    e.feature.iotDoorControlCount /
                    cityData.data.iotdoorControlCount
                  ).toFixed(2)
                : 0.001;
            const openPercentage =
              (e.feature.openUserCount / cityData.data.userCommunity).toFixed(
                2
              ) > 0.001
                ? (
                    e.feature.openUserCount / cityData.data.userCommunity
                  ).toFixed(2)
                : 0.001;
            const communityPercentage =
              (e.feature.communityCount / cityData.data.communityCount).toFixed(
                2
              ) > 0.001
                ? (
                    e.feature.communityCount / cityData.data.communityCount
                  ).toFixed(2)
                : 0.001;
            const ringData = [
              {
                id: "open",
                radius: 45,
                value: openPercentage,
                percentageData: [
                  { color: "#3aacf3", value: openPercentage },
                  {
                    color: "rgba(183, 226, 255, 0.3)",
                    value: 1 - openPercentage,
                  },
                ],
              },
              {
                id: "iot",
                radius: 30,
                value: iotPercentage,
                percentageData: [
                  { color: "#5ad8a6", value: iotPercentage },
                  {
                    color: "rgba(183, 226, 255, 0.3)",
                    value: 1 - iotPercentage,
                  },
                ],
              },
              {
                id: "community",
                radius: 15,
                value: communityPercentage,
                percentageData: [
                  { color: "#ff6a00", value: communityPercentage },
                  {
                    color: "rgba(183, 226, 255, 0.3)",
                    value: 1 - communityPercentage,
                  },
                ],
              },
            ];
            this.bubbleData = {
              cityCode: e.feature.cityCode,
              cityName: e.feature.cityName,
              proportion: `${(
                (e.feature.communityCount / this.allCommunityCount) *
                100
              ).toFixed(2)}%`,
              communityCount: this.formatter(e.feature.communityCount),
              openUserCount: this.formatter(e.feature.openUserCount),
              iotDoorControlCount: this.formatter(
                e.feature.iotDoorControlCount
              ),
            };
            const popup = new Popup({
              offsets: [0, 0],
              className: "city-popup",
              closeButton: false,
            })
              .setLnglat(e.lngLat)
              .setDOMContent(this.$refs.popover);
            scene.addPopup(popup);
            this.initUserPopover(ringData);
          }, 100);
        });
        scene.addLayer(pointLayer);
      });
    },
    // 用户城市气泡
    initUserPopover(arrList) {
      let canvas = document.getElementById("cityPopover");
      let ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      //填充画布
      ctx.lineWidth = 8;
      let beginDeg = Math.PI * 1.5;
      let endDeg = Math.PI * 1.5;
      arrList.forEach((t, index) => {
        t.percentageData.forEach((l) => {
          endDeg = beginDeg + 2 * Math.PI * l.value;
          ctx.beginPath();
          ctx.strokeStyle = l.color;
          ctx.arc(60, 60, t.radius, beginDeg, endDeg, false);
          ctx.stroke();
          beginDeg = endDeg;
          ctx.closePath();

          ctx.moveTo(0, 0); //移动笔触到原点
          ctx.fillStyle = "white"; //文本颜色
          ctx.font = "12px normal ";
          ctx.textAlign = "right";
          ctx.fillText(`${(t.value * 100).toFixed(1)}%`, 55, (index + 1) * 16);
        });
      });
    },
    // 视图切换
    handleChangeType(val) {
      if (this.active === val) return;
      this.active = val;
      this.showLoading();
    },
    showToggle() {
      this.isshow = !this.isshow;
    },
    // 关闭延时器
    showLoading() {
      this.isLoading = true;
      this.timer = setTimeout(() => {
        this.isLoading = false;
        window.clearTimeout(this.timer);
      }, 1500);
    },
    // 全屏
    showFullScreen() {

      let element = document.documentElement;
      if (this.fullscreen) {
        if (document.exitFullscreen) {
          document.exitFullscreen();
        } else if (document.webkitCancelFullScreen) {
          document.webkitCancelFullScreen();
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen();
        } else if (document.msExitFullscreen) {
          document.msExitFullscreen();
        }
      } else {
        if (element.requestFullscreen) {
          element.requestFullscreen();
        } else if (element.webkitRequestFullScreen) {
          element.webkitRequestFullScreen();
        } else if (element.mozRequestFullScreen) {
          element.mozRequestFullScreen();
        } else if (element.msRequestFullscreen) {
          // IE11
          element.msRequestFullscreen();
        }
      }
      this.fullscreen = !this.fullscreen;
    },
    // 数字3位数加分隔符
    formatter(str) {
      let reg = /(?=(?!\b)(\d{3})+$)/g;
      return str.toString().replace(reg, ",");
    },
    // 设置DOM高度
    setElementHeight() {
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.showLoading();
        this.height = `${(this.$refs.sectionRef.offsetHeight - 30) / 3}px`;
      }, 100);
    },
    // 设置文字滚动
    setNumberTransform(item) {
      const numberItems = document.querySelectorAll(`.${item.id}  i`);
      // const numberItems = this.$refs[`${item.id}`]
      const numberArr = item.valueArr.filter((item) => !isNaN(item));
      numberItems.forEach((t) => {
        t.style.transition = "transform 0s ease-in-out";
        t.style.transform = `translateY(-0%)`;
      });
      setTimeout(() => {
        numberItems.forEach((ls, index) => {
          ls.style.transition = "transform 0.8s ease-in-out";
          ls.style.transform = `translateY(-${numberArr[index] * 10}%)`;
        });
      }, 15);
    },
    // 定时
    initInterval(targetList, time) {
      const { setNumberTransform } = this;
      return setInterval(
        (function fn() {
          targetList.forEach((t) => {
            setTimeout(() => {
              setNumberTransform(t);
            }, 50);
          });
          return fn;
        })(),
        time
      );
    },
    // 定时刷新数字翻牌器
    timedRefresh(targetList, type) {
      let cityTimer;
      let userTimer;
      let deviceTimer;
      if (type === "city") {
        cityTimer = this.initInterval(targetList, 8000);
      } else if (type === "user") {
        userTimer = this.initInterval(targetList, 10000);
      } else if (type === "device") {
        deviceTimer = this.initInterval(targetList, 8000);
      }
      this.$once("hook:beforeDestroy", () => {
        clearInterval(cityTimer);
        clearInterval(userTimer);
        clearInterval(deviceTimer);
        userTimer = null;
        cityTimer = null;
        deviceTimer = null;
      });
    },



    async initGraph(){
      //const imgs = ['../imgs/boy.png','../imgs/girl.png'];

      // Random connected graph
      const gData = this.nodedata;



      let  nodes=  this.nodedata.nodes
      let links=   this.nodedata.links;



      try {

        axios({
          method: "get",
          url: "http://localhost:8080/queryAllNode",

        }).then((res) => {


          console.log(res)
          let da =res.data.data;

          for (var i=0;i<da.length;i++){
            var f = 0;
            for(var j=0;j<nodes.length;j++){
              if(nodes[j].id===da[i].id){

                console.log(1123123131+nodes[j].id)
                f = 1;
                break;
              }

            }
            if(f===1){
              continue;
            }
            nodes.push(da[i]);
          }

          // nodes.push(...da)


          axios({
            method: "get",
            url: "http://localhost:8080/queryAllLink",

          }).then((res) => {



            let d =res.data.data;
            // for (var i=0 ;i<da.length;i++){
            //   if(da[i].id==="blockchain"){
            //     da[i].img=require("@/assets/blockchain.png");
            //     continue;
            //   }
            //   if(da[i].id==="computer"){
            //     da[i].img=require("@/assets/computer.png");
            //     continue;
            //   }
            //   if(da[i].id==="web"){
            //     da[i].img=require("@/assets/web.png");
            //     continue;
            //   }
            //   if(da[i].id==="smartcontract"){
            //     da[i].img=require("@/assets/contract.png");
            //     continue;
            //   }
            //
            //   da[i].img=require("@/assets/resource.png")
            // }


            links.push(...d)
            console.log(this.nodedata.nodes)
            console.log(this.nodedata.links)

            //const distance = 600;
            this.myGraph =  ForceGraph3D({
               controlType: "trackball", // orbit沿2d轨迹绕着拖动，fly 固定不动
              rendererConfig: { antialias: true, alpha: true },
              extraRenderers: [new CSS2DRenderer()],
            })(this.$refs.graph)
                .nodeThreeObject(({ img }) => {
                  console.log(123+`${img}`);
                  const imgTexture = new THREE.TextureLoader().load(`${img}`);
                  const material = new THREE.SpriteMaterial({ map: imgTexture });
                  const sprite = new THREE.Sprite(material);
                  sprite.scale.set(15, 15);
                  return sprite;
                })
                .graphData(gData)

                .nodeThreeObject(node => {
                  console.log(234234234+node)
                  const nodeEl = document.createElement('div');
                  nodeEl.textContent = node.id;
                  nodeEl.style.color = node.color;
                  nodeEl.className = 'node-label';
                  return new CSS2DObject(nodeEl);
                })
            //
                .nodeThreeObjectExtend(true)
                .nodeLabel("id")
                .nodeAutoColorBy("group")
                .linkWidth(1)
                // .enableNodeDrag(false)
                // .enableNavigationControls(false)
                // .showNavInfo(false)
                //.cameraPosition({ z: distance })
                .linkDirectionalParticles("value")
                .linkDirectionalParticleSpeed(d => d.value * 0.01)
                .onNodeClick((node) => {




                  // Aim at node from outside it
                  const distance = 40;
                  const distRatio = 1 + distance / Math.hypot(node.x, node.y, node.z);

                  const newPos =
                      node.x || node.y || node.z
                          ? {
                            x: node.x * distRatio,
                            y: node.y * distRatio,
                            z: node.z * distRatio,
                          }
                          : {
                            x: 0,
                            y: 0,
                            z: distance,
                          }; // special case if node is in (0,0,0)

                  this.myGraph.cameraPosition(
                      newPos, // new position
                      node, // lookAt ({ x, y, z })
                      3000 // ms transition duration
                  );

                  //var location = window.location.href
                  axios({
                    methods: "get",
                    url:"http://localhost:8080/searchData?name="+node.id,

                  }).then((res) =>{
                    console.log(res.data);

                    window.location.href = "http://localhost:8080/checkPage"
                  });
                })

            //
            // let angle = 0;
            // setInterval(() => {
            //   this.myGraph.cameraPosition({
            //
            //     x: distance * Math.sin(angle),
            //
            //     z: distance * Math.cos(angle)
            //   });
            //   angle += Math.PI / 300;
            // }, 10);








          });

        });

      }catch (e){
        alert("error")
      }



































      //let da;



      // function sleep(numberMillis) {
      //
      //   var now = new Date();
      //
      //   var exitTime = now.getTime() + numberMillis;
      //
      //   // eslint-disable-next-line no-constant-condition
      //   while (now.getTime() < exitTime) {
      //
      //     now = new Date();
      //
      //
      //   }
      //
      // }
      // window.location.href="http://localhost:8089";


    },

    search() {
      console.log(this.username);
      axios({
        method: "get",
        url: "",
        params: {
          something: this.username,
        },
      }).then((res) => {
        console.log("数据：", res);
      });
      location.href="http://127.0.0.1:8080/test";
    },
  },
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import './index.styl';
</style>
<style>
/* From uiverse.io by @kirzin */
button {
  text-decoration: none;
  position: absolute;
  border: none;
  font-size: 14px;
  font-family: inherit;
  color: #fff;
  width: 9em;
  height: 2.6em;
  line-height: 2em;
  text-align: center;
  background: linear-gradient(90deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
  background-size: 300%;
  border-radius: 30px;
  z-index: 1;
}

button:hover {
  animation: ani 8s linear infinite;
  border: none;
}

@keyframes ani {
  0% {
    background-position: 0%;
  }

  100% {
    background-position: 400%;
  }
}

button:before {
  content: "";
  position: absolute;
  top: -5px;
  left: -5px;
  right: -5px;
  bottom: -5px;
  z-index: -1;
  background: linear-gradient(90deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
  background-size: 400%;
  border-radius: 35px;
  transition: 1s;
}

button:hover::before {
  filter: blur(20px);
}

button:active {
  background: linear-gradient(32deg, #03a9f4, #f441a5, #ffeb3b, #03a9f4);
}
</style>
<style>
/* From uiverse.io by @alexruix */
.group {
  top: 100px;
  left: 300px;
  z-index: 999999;
  position: relative;
  display: flex;
  line-height: 28px;
  align-items: center;
  position: relative;
  max-width: 300px;
}

.input {
  width: 400px;
  height: 40px;
  line-height: 28px;
  padding: 0 1rem;
  padding-left: 2.5rem;
  border: 2px solid transparent;
  border-radius: 8px;
  outline: none;
  background-color: #f3f3f4;
  color: #0d0c22;
  transition: 0.3s ease;
}

.input::placeholder {
  color: #9e9ea7;
}

.input:focus,
input:hover {
  outline: none;
  border-color: rgba(234, 76, 137, 0.4);
  background-color: #fff;
  box-shadow: 0 0 0 4px rgb(234 76 137 / 10%);
}

.icon {
  position: absolute;
  left: 1rem;
  fill: #9e9ea7;
  width: 1rem;
  height: 1rem;
}
</style>