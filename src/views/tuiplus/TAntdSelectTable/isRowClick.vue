<template>
  <t-layout-page>
    <t-layout-page-item>
      <el-radio-group v-model="mode" size="small" style="margin-bottom: 15px">
        <el-radio-button value="radio">单选</el-radio-button>
        <el-radio-button value="multiple">多选</el-radio-button>
      </el-radio-group>
      <t-antd-select-table
        ref="tantdselecttable"
        selectWidth="40%"
        v-model="state.selectVal"
        :table="state.table"
        :columns="state.table.columns"
        :scroll="{ x: 2000, y: 400 }"
        :mode="mode"
        isShowPagination
        :keywords="{ label: 'materialName', value: 'materialCode' }"
        @checked-change="checkedChange"
        :defaultSelectVal="state.defaultSelectVal"
        @change="tablePaginationChange"
        disabledPorp="materialCode"
        disabledValue="555"
        placeholder="开启禁用模式，不支持点击整行选中"
      ></t-antd-select-table>
    </t-layout-page-item>
  </t-layout-page>
</template>
<script setup lang="ts">
import TAntdSelectTable from "@/components/TAntdSelectTable/index.vue";
import data from "./getData.json";
import data1 from "./getData2.json";
import { onMounted, reactive, ref } from "vue";
const tantdselecttable = ref<HTMLElement | any>(null);
const mode = ref("multiple");
const state: any = reactive({
  selectVal: null,
  defaultSelectVal: [], // "02.21", "LONG02"
  table: {
    pagination: {
      current: 1,
      total: 0, // 总数
      showSizeChanger: true,
      pageSizeOptions: ["10", "20", "40", "80", "100"],
      showTotal: (total: any) => `共 ${total} 条`, // 分页中显示总的数据
      pageSize: 10 // 每页条数，所有页设置统一的条数
    },
    data: [],
    columns: [
      { title: "物料编号", minWidth: 100, dataIndex: "materialCode" },
      { title: "物料名称", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称1", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称2", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称3", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称4", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称5", minWidth: 150, dataIndex: "materialName" },
      { title: "物料名称6", minWidth: 150, dataIndex: "materialName" }
    ]
  }
});
const checkedChange = (keys: any, row: any) => {
  console.log("传给后台的值", keys, row);
  console.log("传给v-model", state.selectVal);
};
// 获取下拉数据数据
const getList = async (pageNum: number | undefined) => {
  let res;
  if (pageNum === 1) {
    res = await data;
  } else {
    res = await data1;
  }
  if (res.success) {
    state.table.pagination.total = res.data.total;
    state.table.data = res.data.records;
  }
};
const tablePaginationChange = (pagination: { current: any; pageSize: any }) => {
  state.table.pagination.current = pagination?.current || 1; // 重新设置当前页
  state.table.pagination.pageSize = pagination?.pageSize || 20;
  // console.log("tantdselecttable", tantdselecttable.value);
  getList(state.table.pagination.current);
  setTimeout(() => {
    tantdselecttable.value.openSelectDropdown();
  }, 300);
};

onMounted(() => {
  getList(1);
});
</script>
