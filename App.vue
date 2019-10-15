<template>
    <div>
        <calendar></calendar>
    </div>
</template>


<style> 

</style>


<script>
// 日历
import Calendar from './Calendar';

export default {
    // 数据
    data() {
        return {
            msg : 'hellow world'
        }
    },
    // 局部组件，使用的是引入的组件
    components: {
        Calendar: Calendar,
    }
}
</script>




