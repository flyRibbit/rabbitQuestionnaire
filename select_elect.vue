<template>
  <div>
    <template v-for="(value, index, key) in dataElect">
      <div v-if="value" :key="key">
        <!-- 编辑中 -->
        <a-card
          v-if="editableLength === index"
          style="max-width: 720px; margin: auto; margin-top: 20px"
        >
          <div class="headline">
            <span class="title-row">{{ index + 1 }}.</span>
            <p>
              <a-input
                :suffix="`${dataElect[index].title.length}/128`"
                v-model="dataElect[index].title"
              />
            </p>
          </div>
          <template
            v-for="(contentVal, contentIndex, contentKey) in value.options"
          >
            <p
              class="content"
              :key="contentKey"
              v-if="dataElect[index].options[contentIndex]"
            >
              <span>
                <a-radio v-if="value.type === 1" disabled :value="1" />
                <a-checkbox v-else :default-checked="false" disabled />
              </span>
              <a-input
                :suffix="`${dataElect[index].options[contentIndex].length}/128`"
                v-model="dataElect[index].options[contentIndex]"
              />

              <span class="deleteBtn" v-if="contentIndex >= 2">
                <a-button
                  shape="circle"
                  icon="close"
                  value="small"
                  @click="closeCommand(index, contentIndex)"
                />
              </span>
            </p>
          </template>
          <div class="operatingBtn">
            <a-button
              shape="round"
              style="
                background-color: #2db7f5;
                border-color: #2db7f5;
                color: #fff;
              "
              value="small"
              @click="appendCommand(index)"
              >添加选项</a-button
            >
            <a-button
              shape="round"
              type="danger"
              value="small"
              @click="deleteCommand(index)"
              >删除问题</a-button
            >
            <a-button
              shape="round"
              type="primary"
              value="small"
              @click="validationProblem"
              >确认问题</a-button
            >
          </div>
        </a-card>
        <!-- 编辑完毕 -->
        <a-card
          v-else
          :bordered="false"
          style="max-width: 720px; margin: auto; margin-top: 20px"
        >
          <div class="headline">
            <span class="title-row"
              >{{ index + 1 }}.
              <span class="title-row_Text">{{ dataElect[index].title }}</span>
              <span class="title-row_icon"
                ><a-icon @click="editableLength = index" type="edit"
              /></span>
            </span>
          </div>
          <template
            v-for="(contentVal, contentIndex, contentKey) in value.options"
          >
            <p
              class="content"
              :key="contentKey"
              v-if="dataElect[index].options[contentIndex]"
            >
              <span>
                <a-radio v-if="value.type === 1" disabled :value="1" />
                <a-checkbox v-else :default-checked="false" disabled />
                {{ dataElect[index].options[contentIndex] }}
              </span>
            </p>
          </template>
        </a-card>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "select_elect",
  props: ["dataElect"],
  data() {
    return {
      editableLength: this.dataElect.length - 1,
    };
  },
  methods: {
    // 刷新
    refresh() {
      this.editableLength = this.dataElect.length - 1;
    },
    // 发送组件
    SendParameters(data) {
      this.$emit("selectData", data);
    },
    // 单行删除操作
    closeCommand(index, contentIndex) {
      this.SendParameters({ index, contentIndex, type: "closeCommand" });
    },
    // 添加选项
    appendCommand(index) {
      this.SendParameters({ index, type: "appendCommand" });
    },
    // 删除问题
    deleteCommand(index) {
      this.SendParameters({ index, type: "deleteCommand" });
      this.SendParameters({ power: true, type: "validationProblem" });
    },
    // 确认问题
    validationProblem() {
      this.editableLength = undefined;
      this.SendParameters({ power: true, type: "validationProblem" });
    },
  },
};
</script>

<style lang="less" scoped>
.headline {
  display: flex;
  margin-bottom: 10px;
  .title-row {
    font-size: 14px;
    font-weight: 600;
    color: #222;
    padding-right: 10px;
    line-height: 35px;
    .title-row_Text {
      margin-left: 8px;
      font-weight: 400;
    }
    .title-row_icon {
      margin-left: 8px;
    }
  }
  > p {
    width: 100%;
  }
}
.content {
  display: flex;
  width: 100%;
  > span:nth-child(1) {
    margin-right: 10px;
  }
  > .deleteBtn {
    margin-left: 10px;
  }
}
.operatingBtn {
  display: flex;
  justify-content: flex-end;
  > button {
    margin: 0 5px;
  }
}
</style>
