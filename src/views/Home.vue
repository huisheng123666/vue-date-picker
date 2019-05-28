<template>
  <div class="date">
    <div class="control">
      <i class="prev" @click="prevMonth"></i>
      <p>{{currentDate}}</p>
      <i class="next" @click="nextMonth"></i>
    </div>
    <div class="head" v-for="item in week" :key="item">{{item}}</div>
    <div class="start" v-for="item in startArr" :key="item">{{item}}</div>
    <div class="current" @click="changeDate(item)" v-for="item in currentArr" :key="item.date">{{item.day}}</div>
    <div class="end" v-for="item in endArr" :key="item">{{item}}</div>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      date: new Date(),
      week: ['日', '一', '二', '三', '四', '五', '六']
    }
  },
  computed: {
    currentDate () {
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      return `${year}-${month + 1 > 9 ? month + 1 : '0' + (month + 1)}`
    },
    currentArr () {
      let arr = []
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      let day = this.getLastDay(year, month + 1)
      for (let i = 0; i < day; i++) {
        arr.push({
          date: `${year}-${month + 1 >= 10 ? month + 1 : '0' + (month + 1)}-${i + 1 >= 10 ? i + 1 : '0' + (i + 1)}`,
          day: i + 1
        })
      }
      return arr
    },
    startArr () {
      let arr = []
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      let week = this.getFirstWeek()
      let prevMonthDay = this.getLastDay(year, month)
      for (let i = week; i > 0; i--) {
        arr.push(prevMonthDay - i + 1)
      }
      return arr
    },
    endArr () {
      let arr = []
      let year = this.date.getFullYear()
      let month = this.date.getMonth() + 1
      // 获取当月第一天星期
      let week = this.getFirstWeek()
      // 当月加上月天数是否在35内
      let day = this.getLastDay(year, month) + week
      let end = 0
      // 最少35，最多42
      if (day / 7 <= 5) {
        end = 35 - day
      } else {
        end = 42 - day
      }
      for (let i = 0; i < end; i++) {
        arr.push(i + 1)
      }
      return arr
    }
  },
  methods: {
    getLastDay (year, month) {
      return new Date(year, month, 0).getDate()
    },
    getFirstWeek () {
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      return new Date(year, month, 1).getDay()
    },
    prevMonth () {
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      this.date = new Date(year, month - 1)
    },
    nextMonth () {
      let year = this.date.getFullYear()
      let month = this.date.getMonth()
      this.date = new Date(year, month + 1)
    },
    changeDate (date) {
      this.$emit('change', date.date)
      console.log(date)
    }
  }
}
</script>

<style lang="stylus" scoped>
.date
  padding-top 30px
  width 350px
  display flex
  flex-wrap wrap
  border 1px solid #eee
  position absolute
  &>div
    flex 0 0 50px
    line-height 30px
    text-align center
    &.start, &.end
      color #ccc
    &.current
      transition all 0.2s
      cursor pointer
      &:hover
        background-color aqua
  .control
    position absolute
    width 100%
    display flex
    align-items center
    justify-content space-between
    top 0
    left 0
    line-height 30px
    border-bottom 1px solid #eee
    &>i
      width 0
      height 0
      border 10px solid transparent
      cursor pointer
      &.prev
        border-right-color aqua
      &.next
        border-left-color aqua
    &>p
      margin 0
      padding 0
</style>
