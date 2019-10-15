<template>
    <div class="calendar">
        <!-- 上方 -->
        <div class="date-header">
            <div class="prev-month" @click="handlePrev"></div>
            <div class="show-date">{{ year }} 年 {{ month }} 月 {{ day }} 日</div>
            <div class="next-month" @click="handleNext"></div>
        </div>
        <!-- 下方 -->
        <div class="dte-content">
            <div class="week-header">
                <div v-for="item in ['日','一','二','三','四','五','六']" :key="item">{{ item }}</div>
            </div>
            <div class="week-day">
                <div class="every-day" v-for="item in 42" :key="item">
                    <!-- 正常天数 -->
                    <div 
                    @click="handleChooseDay" 
                    :data-day="item - beginDay" 
                    :class="{
                        'now-day':`${year}-${month}-${item - beginDay}` === curDate,
                        'active-day':`${year}-${month}-${item - beginDay}` === `${year}-${month}-${day}`,
                        }" 
                    v-if="item - beginDay > 0 && item - beginDay <= curMonthTimes">
                        {{ item - beginDay }}
                    </div>
                    <!-- 上月天数 -->
                    <div v-else-if="item - beginDay <= 0" class="other-day">{{ item - beginDay + prevDays }}</div>
                    <!-- 下月天数 -->
                    <div v-else class="other-day">{{ item - beginDay - curMonthTimes }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    // 数据
    data() {
        return {
            year: null,
            month: null,
            day: null,
            curDate: null,
        }
    },
    // 生命周期函数 创建后
    created() {  
        this.getInitTime();
    },
    // 方法列表
    methods: {
        getInitTime() {
            const date = new Date();
            this.year = date.getFullYear();
            this.month = date.getMonth() + 1;
            this.day = date.getDate();
            this.curDate = `${this.year}-${this.month}-${this.day}`;
        },
        handleChooseDay(e) {
            this.day = e.target.dataset.day;
        },
        handlePrev() {
            if(this.month === 1) {
                this.month = 12;
                this.year --;       // 年减一
            } else {
                this.month --;
            }
            this.computedDay();
        },
        handleNext() {
            if(this.month === 12) {
                this.month = 1;
                this.year ++;       // 年加一
            } else {
                this.month ++;
            }
            this.computedDay();
        },
        computedDay() {
            // 获取的月份已经 ++ 或 -- 之后的天数
            const allDay = new Date(this.year,this.month,0).getDate();
            if(this.day > allDay) {
                this.day = allDay;
            }
        }
    },
    // 计算属性
    computed: {
        // 获取一个月的一号是星期几
        beginDay() {
            return new Date(this.year,this.month - 1,1).getDay();/*注意：此处必须是 getDay()*/
        },

        // 获取当前月份的天数
        // 一三五七八十腊，三十一天永不差。
        // 四六九冬三十整，唯有二月二十八。
        // 逢到闰年加一日，如果不加就算差。
        // 闰年：
        // 1：普通年能被4整除且不能被100整除的为闰年。(如2004年就是闰年，1900年不是闰年)
        // 2：世纪年能被400整除的就是闰年。(如2000年是闰年，1900年不是闰年)
        // 3：对于数值很大的年份，这年如果能整除3200，并且能整除172800则是闰年
        curMonthTimes() {
        // 方法一
            // if([1,3,5,7,8,10,12].includes(this.month)) {
            //     return 31; 
            // } else if([4,6,9,11].includes(this.month)) {
            //     return 30;
            // } else if(this.month % 4 == 0 && this.month % 100 != 0 || this.month % 400 == 0) {
            //     return 29;
            // } else{
            //     return 28;
            // }
        // 方法二
        return new Date(this.year,this.month,0).getDate();/*注意：此处必须是 getDate()*/
        },

        // 获取上月的天数
        prevDays() {
            return new Date(this.year,this.month - 1,0).getDate();/*注意：此处必须是 getDate()*/
        },
    },
}
</script>

<style scroped>
    .calendar{
        width:500px;
        margin:100px auto;
        padding:10px;
        border-radius:5px;
        border:1px solid black;
    }
    /* 上方 */
    .date-header{
        width:100%;
        display:flex;
        line-height:30px;
    }
    .prev-month,.next-month{
        border:15px solid transparent;
        cursor:pointer;
    }
    .prev-month{
        border-right-color:#007fff;
    }
    .next-month{
        border-left-color:#007fff;
    }
    .show-date{
        flex:1 1 0;
        text-align:center;
        color:#007fff;
    }
    /* 下方 */
    .week-header{
        display:flex;
        margin:5px 0;
    }
    .week-header > div{
        flex:1 1 0;
        text-align:center;
        line-height:30px;
        background-color:#007fff;
        color:#fff;
        font-weight:600;
    }
    .week-day{
        width:100%;
    }
    .week-day .every-day{
        display:inline-block;
        width:14.28%;
        text-align:center;
        line-height:50px;
        cursor:pointer;
    }
    .other-day{
        color:#ccc;
    }
    .now-day{
        background-color:#007fff;
        color:#fff;
        border-radius:8px;
    }
    .active-day:not(.other-day){
        color:#007fff;
        border:2px solid #007fff;
        line-height:46px;
        border-radius:8px;
        color:#000;
    }
</style>