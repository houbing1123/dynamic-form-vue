<template>
  <div class="home">
    <formData :data="tablesForm" add="add" @submit="FormSubmit" form="form" />
  </div>
</template>

<script>
// @ is an alias to /src
import formData from "@/components/formData.vue";

export default {
  name: "Home",
  components: {
    formData,
  },
  data() {
    return {
      tablesForm: {
        headerRows: [
          { label: "序号", key: "index" },
          { label: "姓名", key: "name" },
          { label: "职务", key: "duty" },
          { label: "住址", key: "address" },
          { label: "邮箱", key: "email" },
        ],
        rows: [
          [
            { type: "text", value: "", id: 0 },
            { type: "text", value: "" },
            { type: "text", value: "" },
            { type: "text", value: "" },
            { type: "text", value: "" },
          ],
        ],
      },
    };
  },
  methods: {
    FormSubmit(res) {
      console.log(res, "sssss");
    },
  },
};
</script>
