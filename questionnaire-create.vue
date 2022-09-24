<template>
    <a-card
      title="问卷问题"
      style="margin: auto; margin-top: 20px"
      :bordered="false"
    >
      <selectElect
        ref="selectLength"
        @selectData="MonitoringData"
        :dataElect="test.questions"
      />
      <div class="addBtn" v-if="power">
        <a-button type="primary" shape="round" @click="SubjectAdd(1)">
          <a-icon type="plus-circle" />单选题</a-button
        >
        <a-button type="primary" shape="round" @click="SubjectAdd(2)">
          <a-icon type="plus-circle" />多选题</a-button
        >
      </div>
      <div class="addBtn" v-else>
        <a-button type="primary" shape="round" disabled>
          <a-icon type="plus-circle" />单选题</a-button
        >
        <a-button type="primary" shape="round" disabled>
          <a-icon type="plus-circle" />多选题</a-button
        >
      </div>
    </a-card>
</template>

<script>
// 引入脚本文件
import selectElect from "./select_elect.vue";
export default {
  name: "selectCreate",
  components: { selectElect },
  data() {
    return {
      // 数据格式
      test: {
        title: "",
        expire: null,
        published: 1,
        questions: [],
        community_id: 1,
      },
      // 显示隐藏可编辑和不可编辑状态
      power: true,
    };
  },
  methods: {
    // 数据操作
    MonitoringData(e) {
      console.log(e, "MonitoringData");
      if (e.type === "closeCommand") {
        this.test.questions[e.index].options.splice(e.contentIndex, 1);
      } else if (e.type === "appendCommand") {
        this.test.questions[e.index].options.push("新增选项");
      } else if (e.type === "deleteCommand") {
        this.test.questions.splice(e.index, 1);
        console.log(this.test.questions, e.index);
      } else if (e.type === "validationProblem") {
        // 开关
        this.power = e.power;
      }
    },
    // 选题添加
    SubjectAdd(typeNumber) {
      if (this.power) {
        this.test.questions.push({
          type: typeNumber,
          title: typeNumber === 1 ? "单选题" : "多选题",
          options: ["选项1", "选项2", "选项3", "选项4"],
        });
        this.$refs.selectLength.refresh();
        // 开关
        this.power = false;
      }
    },
  },
};
</script>

<style lang="less" scoped>
.addBtn {
  margin-top: 60px;
}
.layout-content-header {
  display: flex;
  margin: 0px -30px 0px -30px;
  margin-bottom: 26px;
  padding: 16px 32px;
  background: #fff;
  border-bottom: 1px solid #e8eaec;
  position: relative;
}
.layout-content-header-title {
  color: #17233d;
  font-weight: 500;
  font-size: 16px;
}
.ant-form-item-control {
  display: flex;
  > span {
    width: 18vw;
  }
}
.result {
  width: 72%;
  margin: 0 auto;
  padding: 32px 0;
  text-align: center;
}
.result-icon-success {
  background-color: #19be6b;
}
.result-icon {
  display: inline-block;
  width: 72px;
  height: 72px;
  line-height: 72px;
  font-size: 72px;
  margin-bottom: 24px;
  border-radius: 50%;
  color: #fff;
}
.result-title {
  margin-bottom: 16px;
  color: #17233d;
  font-weight: 500;
  font-size: 24px;
  line-height: 32px;
}
.result-description {
  margin-bottom: 24px;
  color: #808695;
  font-size: 14px;
  line-height: 22px;
}
.result-extra {
  padding: 24px 40px;
  text-align: left;
  background: #f8f8f9;
  border-radius: 4px;
}
.result-actions {
  margin-top: 32px;
}
</style>
