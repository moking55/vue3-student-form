<script setup>
import { reactive, ref } from "vue";
import { Form } from "ant-design-vue";
//import StudentID from "./components/StudentID.vue";
import SelectMajor from "./components/SelectMajor.vue";

const [activeKey, formItemLayout] = [ref(), ref()];
const useForm = Form.useForm;
const formState = reactive({
  layout: "vertical",
});
const fields = [
  "writeDate",
  "title",
  "to",
  "fname",
  "lname",
  "email",
  "telephone",
  "stdID",
  "reason",
];
const modelRef = reactive({
  ...fields.reduce((obj, field) => ({ ...obj, [field]: "" }), {}),
});
const rulesRef = reactive({
  ...fields.reduce(
    (obj, field) => ({
      ...obj,
      [field]: [
        { required: true, message: "Please enter a value in this field" },
      ],
    }),
    {}
  ),
});
const { validateInfos } = useForm(modelRef, rulesRef, {
  onValidate: (...args) => console.log(...args),
});

function getAllData() {
  console.log(modelRef);
  //Object.assign(modelRef, defaultValue);
  fields.resetFields();
  // axios.post('/some/url', this.form)
}
</script>

<template>
  <div>
    <a-layout class="layout">
      <a-layout-content class="container">
        <h1 style="margin: 20px 0; font-size: 2rem">เขียนคำร้องทั่วไป</h1>

        <a-layout-content
          :style="{
            background: 'white',
            minHeight: '280px',
            padding: '10px 30px',
          }"
        >
          <a-tabs v-model:activeKey="activeKey" animated>
            <a-tab-pane
              key="1"
              tab="เขียนคำร้อง"
              :style="{ padding: '10px 0px' }"
            >
              <a-form
                :layout="formState.layout"
                :model="formState"
                autocomplete="off"
                v-bind="formItemLayout"
              >
                <a-row type="flex" :wrap="true" :gutter="16">
                  <a-col :span="5">
                    <a-form-item label="เขียนเมื่อ">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="12"
                      >
                        <a-date-picker
                          v-model:value="modelRef.writeDate"
                          style="width: 100%"
                          placeholder="เขียนเมื่อ"
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col flex="auto">
                    <a-form-item label="เรื่อง" v-bind="validateInfos.title">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-input
                          v-model:value="modelRef.title"
                          show-count
                          :maxlength="100"
                          placeholder="เรื่อง"
                          req
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col :span="5">
                    <a-form-item label="ถึง" v-bind="validateInfos.to">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="12"
                      >
                        <a-input
                          v-model:value="modelRef.to"
                          placeholder="ถึง"
                          req
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                </a-row>

                <a-divider orientation="left" orientation-margin="0px">
                  ข้อมูลนักศึกษา
                </a-divider>
                <!-- Student Information -->
                <a-row type="flex" :wrap="true" :gutter="16">
                  <a-col :span="3 * 2">
                    <a-form-item label="ชื่อ" v-bind="validateInfos.fname">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-input
                          v-model:value="modelRef.fname"
                          placeholder="ชื่อ"
                          req
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col :span="3 * 2">
                    <a-form-item label="นามสกุล" v-bind="validateInfos.lname">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-input
                          v-model:value="modelRef.lname"
                          placeholder="นามสกุล"
                          req
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col :span="6 * 2">
                    <a-form-item
                      label="รหัสนักศึกษา"
                      v-bind="validateInfos.stdID"
                    >
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-input
                          v-model:value="modelRef.stdID"
                          placeholder="รหัสนักศึกษา"
                          req
                        />
                      </a-space>
                      <!-- <StudentID :digit-count="10" /> -->
                    </a-form-item>
                  </a-col>
                  <SelectMajor />
                  <a-col :span="3 * 2">
                    <a-form-item label="Email" v-bind="validateInfos.email">
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-input
                          v-model:value="modelRef.email"
                          placeholder="Email"
                          req
                        />
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col :span="3 * 2">
                    <a-form-item
                      label="เบอร์โทรศัพท์"
                      v-bind="validateInfos.telephone"
                      :rules="[
                        {
                          required: true,
                          message: 'Please input your password!',
                        },
                      ]"
                    >
                      <a-space
                        direction="vertical"
                        :style="{ width: '100%' }"
                        compact
                        :size="11"
                      >
                        <a-space
                          direction="vertical"
                          :style="{ width: '100%' }"
                          compact
                          :size="11"
                        >
                          <a-input
                            v-model:value="modelRef.telephone"
                            placeholder="เบอร์โทรศัพท์"
                            req
                          />
                        </a-space>
                      </a-space>
                    </a-form-item>
                  </a-col>
                  <a-col :span="12 * 2">
                    <a-form-item
                      label="ระบุเหตุผลประกอบคําร้อง"
                      v-bind="validateInfos.reason"
                      :style="{ width: '100%' }"
                    >
                      <a-textarea
                        v-model:value="modelRef.reason"
                        placeholder="textarea with clear icon"
                        allow-clear
                        :style="{ width: '100%', height: '150px' }"
                      />
                    </a-form-item>
                    <div :style="{ textAlign: 'center' }">
                      <a-button
                        type="primary"
                        @click="getAllData"
                        html-type="submit"
                        >ส่งคำร้อง</a-button
                      >
                    </div>
                  </a-col>
                </a-row>
              </a-form>
            </a-tab-pane>
            <a-tab-pane key="2" tab="คำร้องทั้งหมด">คำร้องทั้งหมด</a-tab-pane>
          </a-tabs>
        </a-layout-content>
      </a-layout-content>
    </a-layout>
  </div>
</template>

<style scoped>
.layout {
  min-height: 100vh;
}
.container {
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
  width: 100%;
}
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 970px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1170px;
  }
}
</style>
