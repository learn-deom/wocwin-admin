<template>
  <t-layout-page>
    <t-layout-page-item>
      <t-form ref="TFormDemo" v-model="formOpts.ref" :formOpts="formOpts" />
    </t-layout-page-item>
  </t-layout-page>
</template>

<script setup lang="tsx">
// 获取ref
const TFormDemo: any = ref<HTMLElement | null>(null);
// 提交formOpts.ref 方式form表单
const submitForm = () => {
  formOpts.ref.validate((valid: any) => {
    console.log(88, valid);
    console.log(77, formOpts.formData);
    if (!valid) return;
    console.log("最终数据", formOpts.formData);
  });
};
// 提交form表单
// const submitForm = async () => {
//   const { valid, formData } = await TFormDemo.value.validate()
//   console.log('formOpts.ref', formOpts.ref)
//   console.log('formOpts.formData', formData)
//   if (!valid) return
//   console.log('最终数据', formData)
// }
// 重置form表单
const resetForm = () => {
  formOpts.formData = {};
  // 清空校验
  TFormDemo.value.clearValidate();
};
// 清除校验
const accountFocus = (val: { type: any }) => {
  console.log("账号聚焦事件", val.type);
};
const accountClear = () => {
  console.log("账号清空事件");
};
const accountBlur = (val: { type: any }) => {
  console.log("账号失焦事件", val.type);
};
const formOpts = reactive<FormTypes.FormOpts>({
  ref: null,
  formData: {
    account: null, // *用户账号
    password: null, // *用户密码
    name: null, // *用户昵称
    sex: null, // *性别: 0:男 1:女
    hobby: [], // *爱好: 0:男 1:女
    phone: null, // 手机号码
    valDate: null, // el日期选择范围
    wechat: null, // 微信
    qq: null, // qq
    email: null, // 邮箱
    desc: null, // 描述
    number: null, // 计算器
    status: null // *状态: 0：停用，1：启用(默认为1)',
  },
  fieldList: [
    {
      label: "账号",
      value: "account",
      type: "input",
      comp: "el-input",
      event: "account",
      eventHandle: {
        focus: (val: { type: any }) => accountFocus(val),
        clear: () => accountClear(),
        blur: (val: { type: any }) => accountBlur(val)
      }
    },
    { label: "密码", value: "password", type: "password", comp: "el-input" },
    { label: "昵称", value: "name", type: "input", comp: "el-input" },
    {
      label: "性别",
      value: "sex",
      type: "select-arr",
      comp: "el-select",
      list: "sexList",
      arrLabel: "key",
      arrKey: "value"
    },
    {
      label: "状态",
      value: "status",
      type: "select-arr",
      list: "statusList",
      comp: "el-select",
      arrLabel: "key",
      arrKey: "value"
    },
    {
      label: "爱好",
      value: "hobby",
      type: "checkbox",
      comp: "el-checkbox-group",
      list: "hobbyList",
      event: "checkbox",
      arrKey: "value"
    },
    {
      label: "手机号码",
      value: "phone",
      type: "input",
      comp: "el-input",
      bind: { maxlength: 11 }
    },
    {
      label: "日期",
      value: "valDate",
      type: "daterange",
      comp: "el-date-picker",
      bind: {
        rangeSeparator: "-",
        startPlaceholder: "开始日期",
        endPlaceholder: "结束日期",
        valueFormat: "YYYY-MM-DD"
      }
    },
    { label: "QQ", value: "qq", type: "input", comp: "el-input" },
    { label: "邮箱", value: "email", type: "input", comp: "el-input" },
    {
      label: "计数器",
      value: "number",
      type: "inputNumber",
      widthSize: 1,
      bind: { controls: false, min: 2, max: 99 },
      comp: "el-input-number"
    },
    {
      label: "描述",
      value: "desc",
      type: "textarea",
      comp: "el-input",
      widthSize: 1
    }
  ],
  operatorList: [
    { label: "提交", bind: { type: "danger" }, fun: submitForm },
    { label: "重置", bind: { type: "primary" }, fun: resetForm }
  ],
  // 相关列表
  listTypeInfo: {
    hobbyList: [
      { label: "吉他", value: "0" },
      { label: "看书", value: "1" },
      { label: "美剧", value: "2" },
      { label: "旅游", value: "3" },
      { label: "音乐", value: "4" }
    ],
    sexList: [
      { key: "女", value: 1 },
      { key: "男", value: 0 }
    ],
    statusList: [
      { key: "启用", value: 1 },
      { key: "停用", value: 0 }
    ]
  }
});
</script>
