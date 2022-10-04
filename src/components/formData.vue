<template>
  <div class="formData">
    <table border>
      <thead>
        <th></th>
        <th v-for="title in data.headerRows" :key="title.key">
          {{ title.label }}
        </th>
      </thead>
      <tbody>
        <tr v-for="(row, index) in data.rows" :key="index">
          <td>
            <input
              type="checkbox"
              @change="checked(row, $event)"
              class="checkbox"
            />
          </td>
          <td v-for="(ts, i) in row" :key="i">
            <input
              :type="ts.type"
              v-model="ts.value"
              v-if="ts.type !== 'node'"
            />
            <span v-if="ts.type === 'node'">{{ ts.value }}</span>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="button">
      <button @click="addRow" v-if="add">新增</button
      ><button @click="delRow" v-if="add">删除</button
      ><button @click="submit" v-if="form">提交</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "formData",
  props: ["data", "add", "form"],
  data() {
    return {
      tablesForm: this.data,
      copy_rows: [],
      checkeds: [],
      formData: {},
    };
  },
  methods: {
    // 新增
    addRow() {
      if (this.tablesForm.rows.length !== 0) this.copyRow();
      this.copy_rows[0].id = parseInt(Math.random() * 100);
      console.log(this.copy_rows);
      this.tablesForm.rows.push(this.copy_rows);
    },
    // 提交
    submit() {
      this.tablesForm.headerRows.forEach((item, index) => {
        let strArr = [];
        this.data.rows.forEach((rowItem) => {
          strArr.push(rowItem[index].value);
        });
        this.formData[item.key] = strArr;
      });
      this.$emit("submit", this.formData);
    },
    // 选中状态
    checked(num, el) {
      console.log(num, el);
      console.log(num[0].id);
      if (el.target.checked) {
        this.checkeds.push(num[0].id);
      } else {
        this.checkeds = this.checkeds.filter((item) => item !== num[0].id);
      }
      console.log(this.checkeds);
    },
    // 删除
    delRow() {
      if (this.tablesForm.rows.length === 1) {
        this.copy_rows = this.tablesForm.rows[0];
      }
      this.tablesForm.rows = this.tablesForm.rows.filter(
        (item) => !this.checkeds.includes(item[0].id)
      );
      this.checkeds = [];
      const ck = document.querySelectorAll(".checkbox");
      ck.forEach((item) => {
        item.checked = false;
      });
    },
    // 备份
    copyRow() {
      let arr = [];
      this.tablesForm.rows[0].forEach((item) => {
        arr.push({ ...item });
      });
      this.copy_rows = arr;
    },
  },
  created() {
    this.copyRow();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.formData {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.button {
  margin: 20px 0;
}
</style>
