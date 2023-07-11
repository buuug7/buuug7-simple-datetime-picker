<template>
  <view class="buuug7-simple-datetime-picker" v-if="done">
    <view
      class="mask"
      :class="{ show: open }"
      @touchmove.stop.prevent
      catchtouchmove="true"
    >
    </view>
    <view class="wrap" :class="{ show: open }">
      <view class="picker-header" @touchmove.stop.prevent catchtouchmove="true">
        <view class="btn-picker cancel" @click="open = false">取消</view>
        <view class="btn-picker submit" @click="_onSubmit">确定</view>
      </view>
      <view class="picker-body">
        <picker-view :value="value" @change="_onChange">
          <picker-view-column v-if="timeHide[0]">
            <view class="column-item" v-for="item in years" :key="item">
              {{ item + timeLabel[0] }}
            </view>
          </picker-view-column>
          <picker-view-column v-if="timeHide[1]">
            <view class="column-item" v-for="item in months" :key="item">
              {{ formatNum(item) + timeLabel[1] }}
            </view>
          </picker-view-column>
          <picker-view-column v-if="timeHide[2]">
            <view class="column-item" v-for="item in days" :key="item">
              {{ formatNum(item) + timeLabel[2] }}
            </view>
          </picker-view-column>
          <picker-view-column v-if="timeHide[3]">
            <view class="column-item" v-for="item in hours" :key="item">
              {{ formatNum(item) + timeLabel[3] }}
            </view>
          </picker-view-column>
          <picker-view-column v-if="timeHide[4]">
            <view class="column-item" v-for="item in minutes" :key="item">
              {{ formatNum(item) + timeLabel[4] }}
            </view>
          </picker-view-column>
          <picker-view-column v-if="timeHide[5]">
            <view class="column-item" v-for="item in seconds" :key="item">
              {{ formatNum(item) + timeLabel[5] }}
            </view>
          </picker-view-column>
        </picker-view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: "buuug7-simple-datetime-picker",
  props: {
    startYear: {
      type: Number,
      default: 2000,
    },
    endYear: {
      type: Number,
      default: 2099,
    },
    timeLabel: {
      type: Array,
      default: () => ["年", "月", "日", "时", "分", "秒"],
    },
    timeHide: {
      type: Array,
      default: () => [true, true, true, true, true, false],
    },
    timeInit: {
      type: Number,
      default: new Date().valueOf(),
    },
  },

  data() {
    return {
      open: false,
      years: [],
      months: [],
      days: [],
      hours: [],
      minutes: [],
      seconds: [],
      year: "",
      month: "",
      day: "",
      hour: "",
      minute: "",
      second: "",
      value: [0, 0, 0, 0, 0, 0],
      done: false,
    };
  },

  computed: {
    currentDatetime() {
      return new Date(this.timeInit);
    },
  },

  mounted() {
    this.init();
  },

  watch: {
    month() {
      this.initDays();
    },
  },

  methods: {
    init() {
      this.initYears();
      this.initMonths();
      this.initDays();
      this.initHours();
      this.initMinutes();
      this.initSeconds();
      this.setSelectValue();
      this.done = true;
    },

    initYears() {
      const years = [];
      for (let year = this.startYear; year <= this.endYear; year++) {
        years.push(year);
        if (this.currentDatetime.getFullYear() === year) {
          this.$set(this.value, 0, year - this.startYear);
        }
      }
      this.years = years;
    },

    initMonths() {
      const months = [];
      for (let month = 1; month <= 12; month++) {
        months.push(month);
        if (this.currentDatetime.getMonth() + 1 === month) {
          this.$set(this.value, 1, month - 1);
        }
      }
      this.months = months;
    },

    initDays() {
      const value = this.value;
      const selectedYear = this.years[value[0]];
      const selectedMonth = this.months[value[1]];
      const days = [];
      const totalDays = new Date(selectedYear, selectedMonth, 0).getDate();
      for (let day = 1; day <= totalDays; day++) {
        days.push(day);
        if (this.currentDatetime.getDate() === day) {
          this.$set(value, 2, day - 1);
        }
      }
      this.days = days;
    },

    initHours() {
      const hours = [];
      for (let hour = 0; hour <= 23; hour++) {
        hours.push(hour);
        if (this.currentDatetime.getHours() === hour) {
          this.$set(this.value, 3, hour);
        }
      }
      this.hours = hours;
    },

    initMinutes() {
      const minutes = [];
      for (let minute = 0; minute < 60; minute++) {
        minutes.push(minute);
        if (this.currentDatetime.getMinutes() === minute) {
          this.$set(this.value, 4, minute);
        }
      }
      this.minutes = minutes;
    },

    initSeconds() {
      const seconds = [];
      for (let second = 0; second < 60; second++) {
        seconds.push(second);
        if (this.currentDatetime.getSeconds() === second) {
          this.$set(this.value, 5, second);
        }
      }
      this.seconds = seconds;
    },

    show() {
      this.open = true;
    },

    hide() {
      this.open = false;
    },

    _onChange(e) {
      this.value = e.detail.value;
      this.setSelectValue();
    },

    setSelectValue() {
      const v = this.value;

      this.year = this.years[v[0]];
      this.month = this.months[v[1]];
      this.day = this.days[v[2]];
      this.hour = this.hours[v[3]];
      this.minute = this.minutes[v[4]];
      this.second = this.seconds[v[5]];
    },

    _onSubmit() {
      const {
        year,
        month,
        day,
        hour,
        minute,
        second,
        formatNum,
        timeHide,
        timeLabel,
      } = this;
      const result = {
        year: timeHide[0] ? formatNum(year) : "",
        month: timeHide[1] ? formatNum(month) : "",
        day: timeHide[2] ? formatNum(day) : "",
        hour: timeHide[3] ? formatNum(hour) : "",
        minute: timeHide[4] ? formatNum(minute) : "",
        second: timeHide[5] ? formatNum(second) : "",
      };

      this.$emit("submit", result);
      this.hide();
    },

    formatNum(num) {
      return num < 10 ? "0" + num : num + "";
    },
  },
};
</script>

<style lang="scss">
$transition: all 0.3s ease;
$primary: #488ee9;

.buuug7-simple-datetime-picker {
  position: relative;
  z-index: 999;
  picker-view {
    height: 100%;
  }
  .mask {
    position: fixed;
    z-index: 1000;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.4);
    visibility: hidden;
    opacity: 0;
    transition: $transition;
    &.show {
      visibility: visible;
      opacity: 1;
    }
  }
  .wrap {
    z-index: 1001;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    transition: $transition;
    transform: translateY(100%);
    &.show {
      transform: translateY(0);
    }
  }
  .picker-header {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 8px 8px;
    background-color: darken(#fff, 2%);
    background-color: #fff;
  }
  .picker-body {
    width: 100%;
    height: 420rpx;
    overflow: hidden;
    background-color: #fff;
  }
  .column-item {
    text-overflow: ellipsis;
    white-space: nowrap;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .btn-picker {
    position: relative;
    display: inline-block;
    padding-left: 10px;
    padding-right: 10px;
    box-sizing: border-box;
    text-align: center;
    text-decoration: none;
    line-height: 2;
    -webkit-tap-highlight-color: transparent;
    overflow: hidden;
    background-color: #eee;
    font-size: 14px;
    border-radius: 3px;
    color: #000;
    cursor: pointer;
  }
  .btn-picker.submit {
    background-color: $primary;
    color: #fff;
  }
}
</style>
