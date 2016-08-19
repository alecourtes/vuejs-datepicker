<style lang="scss">
  
  $header-height: 100px;
  $day-size: 41px;
  $main-color: #4CAF50;

  .datepicker{
    position: absolute;
    top: 100%;
    z-index: 5;
    background-color: #fff;
    -webkit-box-shadow: 0 14px 45px rgba(0,0,0,0.25), 0 10px 18px rgba(0,0,0,0.22);
    box-shadow: 0 14px 45px rgba(0,0,0,0.25), 0 10px 18px rgba(0,0,0,0.22);
    padding: 5px;
    width:315px;
  }

  .datepicker__header{
    background-color: $main-color;
    color: #FFF;
    padding: 20px;
    height: $header-height;
  }

  .datepicker__year{
    opacity: 0.7;
    margin-bottom: 10px;
    line-height: 16px;
  }

  .datepicker__date{
    font-size: 32px;
    line-height: 32px;
  }

  .datepicker__week{
    font-size: 12px;
    line-height: 12px;
    color: rgba(0,0,0,0.8);
  }

  .datepicker__weekdays{
    float: left;
    width: $day-size;
    text-align:center;
  }

  .datepicker__days{
    position:relative;
    overflow: hidden;
    margin-top:20px;
    margin-bottom: 0;
    height: $day-size * 5;
    transition: height 0.3s;
    &.has-6-weeks{
      height:$day-size * 6;
    }
  }

  .datepicker__day{
    position:relative;
    width: $day-size;
    height: $day-size;
    float: left;
    text-align: center;
    line-height: $day-size;
    cursor: pointer;
  }

  .datepicker__day__effect{
    position:absolute;
    background-color: $main-color;
    width: 36px;
    height: 36px;
    top: 2px;
    left: 2px;
    border-radius: 50%;
    transition: all 450ms cubic-bezier(.16,.88,.83,.67);
    transform: scale(0);
    opacity:0;
  }

  .datepicker__day:hover{
    color: #FFF;
    .datepicker__day__effect{
      transform: scale(1);
      opacity: 0.6;
    }
  }

  .datepicker__day.selected{
    color: #FFF;
    .datepicker__day__effect{
      transform: scale(1);
      opacity: 1;
    }
  }

  .datepicker__day__text{
    position: relative;
  }

  .datepicker__controls{
    position: relative;
    height: 56px;
    line-height: 56px;
    text-align: center;
    button{
      position: relative;
      border: none;
      background-color: transparent;
      user-select: none;
      outline: none;
      cursor: pointer;
      height:56px;
      with:56px;
    }
    svg{
      width: 30px;
      height: 30px;
      cursor: pointer;
      fill: $main-color;
      vertical-align:middle;
    }
  }

  .datepicker__next{
    float: right;
  }

  .datepicker__prev{
    float: left;
  }

  .datepicker__month{
    position:absolute;
    height: 16px;
    line-height: 16px;
    overflow: hidden;
    top:20px;
    bottom:0;
    left:0;
    right:0;
  }

  .datepicker__actions{
    text-align:right;
    padding: 8px;
  }

  .datepicker__actions button{
    border: none;
    background-color: transparent;
    display: inline-block;
    cursor: pointer;
    outline: none;
    color:$main-color;
    font-size: 14px;
    text-transform: uppercase;
    font-weight: 500;
    min-width: 88px;
    text-align: center;
    -webkit-appearance: none;
    transition: all 0.3s;
    line-height: 36px;
    &:hover{
      background-color: rgba(153,153,153,0.20);
    } 
    
  }

  .datepicker-slide-transition{
    opacity: 1;
    transition: all 0.3s;
    transform: translateY(0);
  }

  .datepicker-slide-leave, .datepicker-slide-enter{
    opacity: 0;
    transform: translateY(-50px);
  }

  @import 'animations';

</style>

<template>
  <div class="datepicker" v-if="visible" transition="datepicker-slide" @click.stop>
    <div class="datepicker__header">
      <div class="datepicker__year">
        {{year}}
      </div>
      <div class="datepicker__date">
        {{date_formatted}}
      </div>
    </div>
    <div class="datepicker__controls">
      <div class="datepicker__month">
        <span v-for="month in [month]" :transition="directionh">{{ month.getFormatted() }}</span>
      </div>
      <button class="datepicker__next" @click="nextMonth()">
       <svg viewBox="0 0 20 20">
          <path d="M12.522,10.4l-3.559,3.562c-0.172,0.173-0.451,0.176-0.625,0c-0.173-0.173-0.173-0.451,0-0.624l3.248-3.25L8.161,6.662c-0.173-0.173-0.173-0.452,0-0.624c0.172-0.175,0.451-0.175,0.624,0l3.738,3.736C12.695,9.947,12.695,10.228,12.522,10.4 M18.406,10c0,4.644-3.764,8.406-8.406,8.406c-4.644,0-8.406-3.763-8.406-8.406S5.356,1.594,10,1.594C14.643,1.594,18.406,5.356,18.406,10M17.521,10c0-4.148-3.374-7.521-7.521-7.521c-4.148,0-7.521,3.374-7.521,7.521c0,4.147,3.374,7.521,7.521,7.521C14.147,17.521,17.521,14.147,17.521,10"></path>
        </svg>
      </button>
      <button class="datepicker__prev" @click="prevMonth()">
       <svg viewBox="0 0 20 20">
              <path d="M11.739,13.962c-0.087,0.086-0.199,0.131-0.312,0.131c-0.112,0-0.226-0.045-0.312-0.131l-3.738-3.736c-0.173-0.173-0.173-0.454,0-0.626l3.559-3.562c0.173-0.175,0.454-0.173,0.626,0c0.173,0.172,0.173,0.451,0,0.624l-3.248,3.25l3.425,3.426C11.911,13.511,11.911,13.789,11.739,13.962,M18.406,10c0,4.644-3.763,8.406-8.406,8.406S1.594,14.644,1.594,10S5.356,1.594,10,1.594S18.406,5.356,18.406,10 M17.521,10c0-4.148-3.373-7.521-7.521-7.521c-4.148,0-7.521,3.374-7.521,7.521c0,4.148,3.374,7.521,7.521,7.521C14.147,17.521,17.521,14.148,17.521,10"></path>
            </svg>
      </button>


    </div>
    <div class="datepicker__week">
        <div v-for="day in days" track-by="$index" class="datepicker__weekdays">
        {{day}}
        </div>
    </div>
    <div class="datepicker__days" :class="classWeeks">
      <div v-for="month in [month]" :transition="directionv">
        <div class="datepicker__day" :style="{width: (month.getWeekStart()*41) + 'px'}">
        </div>
        <div class="datepicker__day" v-on:click="selectDate(day)" v-for="day in month.getDays()" :class="{selected: isSelected(day)}">
          <span class="datepicker__day__effect"></span>
          <span class="datepicker__day__text">{{ day.format('D') }}</span>
        </div>
      </div>
      
    </div>
    <div class="datepicker__actions">
      <button @click="cancel">Annuler</button>
      <button @click="submit">Ok</button>
    </div>
    
  </div>
</template>

<script>
  import Month from '../../modules/month.js'
  export default {
    props: {
      date: {},
      visible: {type: Boolean, default: true},
      directionv: {type: String, default: 'off'},
      directionh: {type: String, default: 'off'}
    },
    data () {
      return {
        days: ['L', 'M', 'M', 'J', 'V', 'S', 'D'],
        month: new Month(this.date.month(), this.date.year())
      }
    },
    methods: {
      isSelected: function (day) {
        return this.date.unix() === day.unix()
      },
      selectDate: function (day) {
        this.date = day.clone()
      },
      nextMonth: function () {
        let month = this.month.month + 1
        let year = this.month.year
        if (month > 11) {
          year += 1
          month = 0
        }
        this.month = new Month(month, year)
        this.slideDirection('slidev-left')
        this.weekDirection('slideh-top')
      },
      prevMonth: function () {
        let month = this.month.month - 1
        let year = this.month.year
        if (month < 0) {
          year -= 1
          month = 11
        }
        this.month = new Month(month, year)
        this.slideDirection('slidev-right')
        this.weekDirection('slideh-bottom')
      },
      submit: function () {
        this.$dispatch('change', this.date)
        this.slideDirection('off')
        this.weekDirection('off')
      },
      cancel: function () {
        this.$dispatch('cancel')
        this.slideDirection('off')
        this.weekDirection('off')
      },
      slideDirection: function (direction) {
        this.directionv = direction
      },
      weekDirection: function (direction) {
        this.directionh = direction
      }
    },
    computed: {
      year () {
        return this.date.format('YYYY')
      },
      date_formatted () {
        return this.date.format('dddd DD MMM')
      },
      classWeeks () {
        return 'has-' + this.month.getWeeks() + '-weeks'
      }
    }
  }
</script>


