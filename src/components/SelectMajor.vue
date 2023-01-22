<script>
const facultyData = [
  "คณะวิทยาศาสตร์",
  "คณะผลิตกรรมการเกษตร",
  "คณะวิศวะกรรมและอุตสาหกรรมเกษตร",
  "คณะพัฒนาการท่องเที่ยว",
  "คณะเศรษฐศาสตร์",
  "คณะบริหารธุรกิจ",
  "คณะศิลปศาสตร์",
  "คณะเทคโนโลยีการประมงและทรัพยากรทางน้ำ",
  "คณะสถาปัตยกรรมศาสตร์และการออกแบบสิ่งแวดล้อม",
];
const majorData = {
  คณะวิทยาศาสตร์: [
    "คณิตศาสตร์",
    "เคมี",
    "เทคโนโลยีสารสนเทศ",
    "วิทยาการคอมพิวเตอร์",
    "สถิติ",
    "เทคโนโลยีชีวภาพ",
    "วัสดุศาสตร์",
  ],
  คณะผลิตกรรมการเกษตร: [
    "อารักขาพืช",
    "ปฐพีศาสตร์",
    "สัตวศาสตร์",
    "เกษตรศาสตร์ (พืชไร่)",
    "พืชศาสตร์ (พืชสวน)",
    "เกษตรเคมี",
  ],
  คณะวิศวะกรรมและอุตสาหกรรมเกษตร: [
    "วิทยาศาสตร์และเทคโนโยลีการอาหาร",
    "วัสดุศาสตร์ (อุตสาหกรรมยาง)",
    "วิศวกรรมเกษตร",
    "วิศวกรรมอาหาร",
    "เทคโนโลยีหลังการเก็บเกี่ยว",
  ],
  คณะพัฒนาการท่องเที่ยว: ["พัฒนาการท่องเที่ยว"],
  คณะเศรษฐศาสตร์: [
    "เศรษฐศาสตร์สหกรณ์",
    "เศรษฐศาสตร์สิ่งแวดล้อม",
    "เศรษฐศาสตร์เกษตร",
    "เศรษฐศาสตร์",
  ],
  คณะบริหารธุรกิจ: [
    "การบัญชี",
    "การเงิน",
    "บริหารธุรกิจ",
    "เทคโนโลยีสารสนเทศทางธุรกิจ",
    "การจัดการทั่วไป",
    "การตลาด",
    "เทคโนโลยีสารสนเทศทางธุรกิจ",
  ],
  คณะศิลปศาสตร์: ["นิเทศศาสตร์บูรณาการ", "ภาษาอังกฤษ"],
  คณะเทคโนโลยีการประมงและทรัพยากรทางน้ำ: ["การประมง"],
  คณะสถาปัตยกรรมศาสตร์และการออกแบบสิ่งแวดล้อม: ["ภูมิสถาปัตยกรรม"],
};
import { defineComponent, reactive, toRefs, computed, watch } from "vue";

export default defineComponent({
  setup() {
    const province = facultyData[0];
    const state = reactive({
      province,
      facultyData,
      majorData,
      secondCity: majorData[province][0],
    });
    const cities = computed(() => {
      return majorData[state.province];
    });
    watch(
      () => state.province,
      (val) => {
        state.secondCity = state.majorData[val][0];
      }
    );
    return {
      ...toRefs(state),
      cities,
    };
  },
  methods: {
    undo() {
      this.$refs.signaturePad.undoSignature();
    },
    save() {
      const { isEmpty, data } = this.$refs.signaturePad.saveSignature();
      console.log(isEmpty);
      console.log(data);
    },
  },
});
</script>
<template>
  <a-col :span="6 * 2">
    <a-form-item label="คณะ / สาขาวิชา">
      <a-space :style="{ width: '100%' }">
        <a-select
          v-model:value="province"
          :style="{ minWidth: '150px' }"
          :options="facultyData.map((pro) => ({ value: pro }))"
        ></a-select>
        <a-select
          v-model:value="secondCity"
          :style="{ minWidth: '150px' }"
          :options="cities.map((city) => ({ value: city }))"
        ></a-select>
      </a-space>
    </a-form-item>
  </a-col>
</template>
