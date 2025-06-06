<template>
  <TLayoutPage>
    <TLayoutPageItem>
      <TForm ref="TFormDemo" v-model="formOpts.ref" :formOpts="formOpts" />
    </TLayoutPageItem>
  </TLayoutPage>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'
// import { TLayoutPage, TLayoutPageItem, TForm } from '@wocwin/t-ui-plus'
// 获取ref
const TFormDemo: any = ref<HTMLElement | null>(null)
// 提交formOpts.ref 方式form表单
const submitForm = () => {
  if (formOpts.ref) {
    formOpts.ref.validate(valid => {
      console.log(88, valid)
      console.log(77, formOpts.formData)
      if (!valid) return
      console.log('最终数据', formOpts.formData)
    })
  }
}

// 重置form表单
const resetForm = () => {
  TFormDemo.value.resetFields()
}
const accountFocus = ({ type }: { type: string }, row: any) => {
  console.log('账号聚焦事件', type, row)
}
const accountClear = () => {
  console.log('账号清空事件')
}
const accountBlur = ({ type }: { type: string }) => {
  console.log('账号失焦事件', type)
}
interface NameBlurEvent {
  type: string
}

const nameBlur = ({ type }: NameBlurEvent) => {
  console.log('昵称失焦事件', type)
}
interface FormInstance {
  validate: (callback: (valid: boolean) => void) => void
  resetFields?: () => void
}

const formOpts = reactive({
  ref: null as null | FormInstance,
  formData: {
    account: 'wocwin', // *用户账号
    password: null, // *用户密码
    name: null, // *用户昵称
    qq: null, // qq
    email: null, // 邮箱
    desc: null // 描述
  },
  fieldList: [
    {
      label: '账号',
      value: 'account',
      type: 'input',
      comp: 'el-input',
      eventHandle: {
        focus: (val: { type: string }, row: any) => accountFocus(val, row),
        clear: () => accountClear(),
        blur: (val: { type: string }) => accountBlur(val)
      }
    },
    { label: '密码', value: 'password', type: 'password', comp: 'el-input' },
    {
      label: '昵称',
      value: 'name',
      type: 'input',
      comp: 'el-input',
      eventHandle: {
        blur: (val: NameBlurEvent) => nameBlur(val)
      }
    },

    { label: 'QQ', value: 'qq', type: 'input', comp: 'el-input' },
    { label: '邮箱', value: 'email', type: 'input', comp: 'el-input' },
    {
      label: '描述',
      value: 'desc',
      type: 'textarea',
      comp: 'el-input',
      widthSize: 1
    }
  ],
  operatorList: [
    { label: '提交', bind: { type: 'danger' }, fun: submitForm },
    { label: '重置', bind: { type: 'primary' }, fun: resetForm }
  ]
})
</script>
