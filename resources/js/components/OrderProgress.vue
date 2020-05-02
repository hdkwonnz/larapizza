<template>
 <div>
    <div class="progress">
        <progressbar :now="progress" type="success" class="progress-bar progress-bar-striped progress-bar-animated"></progressbar>
    </div>
    <div class="order-status">
        <strong>Order Status:</strong> {{ statusNew }}
    </div>

    <img src="/img/delivery.gif" alt="delivery" v-if="progress >= 100">

    <!-- <div class="progress">
        <progressbar now="50" type="success" class="progress-bar progress-bar-striped progress-bar-animated"></progressbar>
    </div> -->  
    <!-- <div class="order-status">
        <strong>Order Status:</strong> {{ 'status' }}
    </div> -->
    <!-- <div>Order Progress component</div> -->
 </div>    
</template>

<script>
    import { progressbar } from 'vue-strap'
    export default {
        components: {
            progressbar
        },

        props:['status', 'initial', 'order_id'],

        data(){
            return{
                statusNew: this.status,
                progress: this.initial               
            }
        },

        mounted() {
            //Echo.channel('pizza-tracker.' + this.order_id)
            Echo.private('pizza-tracker.' + this.order_id)
            .listen('OrderStatusChanged', (order) => {
                //console.log('realtime pizza tracker')
                //console.log(order);
                this.statusNew = order.status_name
                this.progress = order.status_percent
            });
        }
        // mounted() {
        //     console.log('Component mounted')
        // }
    }
</script>
