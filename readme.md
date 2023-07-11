# uniApp 简单的日期时间选择器

uniApp 日期时间选择器, 可选择年, 月, 日, 时, 分, 秒.

## screenshot

<p>
  <img align=top src="https://img-cdn-aliyun.dcloud.net.cn/stream/plugin_screens/19363640-a869-11e9-a6c7-7fb99abe2bbf_0.png?1629144139" width="300px" height="auto">
</p>

## 安装

推荐从[dcloud 插件市场](https://ext.dcloud.net.cn/plugin?id=592) 安装.

## 用法

在 template 中：

```vue
<template>
  <view>
    <button type="default" @click="openDatetimePicker">open picker</button>
    <buuug7-simple-datetime-picker
      ref="myPicker"
      @submit="handleSubmit"
      :start-year="2000"
      :end-year="2099"
      :time-init="1688860800000"
      :time-hide="[true, true, true, true, true, false]"
      :time-label="['年', '月', '日', '时', '分', '秒']"
    />
  </view>
</template>
```

在 script 中：

- 该插件遵循 easycom 规范, 不用显式导入就可以使用 `<buuug7-simple-datetime-picker />`
- 如需显式导入可以使用`import SimpleDateTimePicker from "uni_modules/buuug7-simple-datetime-picker/components/buuug7-simple-datetime-picker/buuug7-simple-datetime-picker.vue";`

```javascript
export default {
  data() {
    return {
      birthday: "",
    };
  },
  methods: {
    // 打开picker
    openDatetimePicker() {
      this.$refs.myPicker.show();
    },

    // 关闭picker
    closeDatetimePicker() {
      this.$refs.myPicker.hide();
    },

    handleSubmit(e) {
      // console.log(e);
      // {year: "2023", month: "07", day: "11", hour: "15", minute: "21", seconds: '55'}
      this.birthday = `${e.year}-${e.month}-${e.day} ${e.hour}:${e.minute}:${seconds}`;
    },
  },
};
```

> Note: 不要把组件放 swiper 里面或者 v-for 里面等, 最好放在页面根部. 通常情况下打开 picker 需要调用`this.$refs.refName.show()`, 在选择完毕后 picker 会自动隐藏，不需要调用`this.$refs.refName.hide()`来手动隐藏。

## 属性说明

#### start-year

类型 `Number`，选择开始年份

#### end-year

类型 `Number`, 选择结束年份

#### time-init

类型`Number`, 自定义初始时间, 默认为当前时间, 值为时间戳

#### time-hidden

类型 `Array`, 自定义时间列显示，默认显示年月日日分 `[true, true, true, true, true, false]`

- 只显示年月日, 则可以设置为`[true, true, true, false, false, false]`
- 时分秒, 则可以设置为`[false, false, false, true, true, true]`

#### time-label

类型 `Array`, 自定义各个时间单位，默认为 `['年', '月', '日', '时', '分', '秒']`, 比如想切换成显示英文的年月日, 可以设置 `['year', 'month', 'day', 'hour', 'minute', 'second']`

#### @submit

类型 `function`, 监听选择事件, 回调函数的第一个参数包含了选择时间的完整信息
