<template>
  <div
    class="year-month-selector flex flex-direction-column"
    :class="{'dark': dark}"
  >
    <div class="flex justify-content-right">
      <CustomButton
        :color="dark ? '#757575' : '#424242'"
        :dark="dark"
        with-border
        @click="past++"
      >
        <span class="fs-16">
          <svg data-v-628b1b6b="" viewBox="0 0 1000 1000"><path data-v-628b1b6b="" d="M336.2 274.5l-210.1 210h805.4c13 0 23 10 23 23s-10 23-23 23H126.1l210.1 210.1c11 11 11 21 0 32-5 5-10 7-16 7s-11-2-16-7l-249.1-249c-11-11-11-21 0-32l249.1-249.1c21-21.1 53 10.9 32 32z"></path></svg>
        </span>
      </CustomButton>
      <CustomButton
        :color="dark ? '#757575' : '#424242'"
        :dark="dark"
        with-border
        @click="$emit('back')"
      >
        <span class="fs-16">
          ✕
        </span>
      </CustomButton>
    </div>
    <div class="flex-1 flex flex-wrap justify-content-between align-center">
      <CustomButton
        v-for="(m, index) in months"
        :key="index"
        :color="color"
        :selected="currentMonth === index"
        :dark="dark"
        class="month-button"
        with-border
        @click="selectMonth(index)"
      >
        {{ m }}
      </CustomButton>
      <CustomButton
        v-for="year in years"
        :key="year"
        :color="color"
        :dark="dark"
        :selected="currentYear === year"
        with-border
        @click="selectYear(year)"
      >
        {{ year }}
      </CustomButton>
    </div>
  </div>
</template>

<script>
  import { getMonthsShort } from '@/VueCtkDateTimePicker/modules/month'
  import CustomButton from '@/VueCtkDateTimePicker/_subs/CustomButton'

  const ArrayRange = (start, end) => {
    return Array(end - start + 1).fill().map((_, idx) => {
      const n = start + idx
      return n
    })
  }

  export default {
    name: 'YearMonthSelector',
    components: {
      CustomButton
    },
    props: {
      locale: { type: String, default: null },
      dark: { type: Boolean, default: null },
      color: { type: String, default: null },
      mode: { type: String, default: null },
      month: { type: Object, default: null }
    },
    data () {
      return {
        months: null,
        years: null,
        past: 0 // Отступ в прошлое
      }
    },
    computed: {
      currentMonth () {
        return this.month.month
      },
      currentYear () {
        return this.month.year
      },
      isMonthMode () {
        return this.mode === 'month'
      }
    },
    mounted () {
      if (this.isMonthMode) {
        this.getMonths()
      } else {
        this.getYears()
      }
    },
    methods: {
      getMonths () {
        this.years = null
        this.months = getMonthsShort(this.locale)
      },
      getYears () {
        this.months = null
        this.years = ArrayRange(this.month.year - 7 - this.past * 7, this.month.year + 7 - this.past * 7)
      },
      selectMonth (monthNumber) {
        this.$emit('input', { month: monthNumber, year: this.currentYear })
      },
      selectYear (year) {
        this.$emit('input', { month: this.currentMonth, year: year })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .year-month-selector{
    position: absolute;
    background-color: white;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    color: #424242;
    padding: 10px;
    &.dark {
      color: white;
      background-color: #424242;
    }
    .month-button {
      text-transform: capitalize;
    }
  }
</style>
