<template>
  <div class="page-home page">
    <section class="nav">
      <router-link :to="{ name: 'STARTED' }">
        快速开始
      </router-link>
      <router-link :to="{ name: 'HOME' }">
        事件行为埋点
      </router-link>
      <router-link :to="{ name: 'CUSTOM_EVENTS' }">
        自定义事件埋点
      </router-link>
      <router-link :to="{ name: 'TRACK_VIEW' }">
        页面行为埋点
      </router-link>
      <router-link :to="{ name: 'HOME' }">
        返回
      </router-link>
      <a href="https://github.com/l-hammer/v-track/issues">打开一个 issue</a>
    </section>

    <!----------------------- DEMO 1 ------------------------>
    <section class="demo">
      <el-alert
        center
        type="info"
        title="这是一个只会上报一次的区域展现埋点"
        :closable="false"
      >
      </el-alert>
      <div class="section-content large">
        <el-card shadow="always" v-track:18028.show.once>
          我只想被曝光一次
        </el-card>
      </div>
    </section>
    <section class="snippets">
      <CodeSnippet
        class="snippet"
        lang="html"
        :code="trackViewComponentOnceSnippet"
      />
    </section>

    <!----------------------- DEMO 2 ------------------------>
    <section class="demo">
      <el-alert
        center
        type="info"
        title="这是一个会连续上报的区域展现埋点"
        :closable="false"
      >
      </el-alert>
      <div class="section-content large">
        <el-card shadow="always" v-track:18027.show>
          我想被曝光无数次
        </el-card>
      </div>
    </section>
    <section class="snippets">
      <CodeSnippet
        class="snippet"
        lang="html"
        :code="trackViewComponentSnippet"
      />
    </section>
  </div>
</template>

<script>
import CodeSnippet from "../components/code-snippet";

const trackViewComponentSnippet = `
<el-card shadow="always" v-track:18027.show>我想被曝光无数次</el-card>
`;
const trackViewComponentOnceSnippet = `
<el-card shadow="always" v-track:18027.show.once>我只想被曝光一次</el-card>
`;

export default {
  name: "BlockShow",
  components: {
    CodeSnippet
  },
  data() {
    return {
      trackViewComponentSnippet,
      trackViewComponentOnceSnippet,
      show: false,
      rest1: null,
      rest2: null
    };
  },
  methods: {
    async fetchRest(delay) {
      const response = await new Promise(resolve => {
        setTimeout(() => {
          resolve({ data: "success" });
        }, 300);
      });

      if (
        (!delay && this.rest1 === response.data) ||
        (delay && this.rest2 === response.data)
      ) {
        this.$message.success(
          "异步事件返回成功，但返回结果和上一次相等，埋点不会上报"
        );
      } else {
        this.$message.success("异步事件返回成功");
        !delay ? (this.rest1 = response.data) : (this.rest2 = response.data);
      }
    }
  }
};
</script>
