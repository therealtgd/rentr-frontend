<template>
    <div>
        <BusinessClientNavBar id="nav"/>
        <div class="content">
            <div class="">
                <button class="btn earnings-view"
                    :class="{'not-selected': !earningsView}"
                    @click="earningsViewClicked()"
                    >Earnings
                </button>
                <button class="btn reservations-view"
                    :class="{'not-selected': !reservationsView}"
                    @click="reservationsViewClicked()"
                    >Reservations
                </button>
                <button class="btn avg-rating-view"
                    :class="{'not-selected': !avgRatingView}"
                    @click="avgRatingViewClicked()"
                    >Avg. Rating
                </button>
            </div>
            <div>
               <BusinessClientEarningsReport v-if="earningsView"/>
               <BusinessClientReservationsReport v-if="reservationsView" />
               <BusinessClientAvgRatingReport v-if="avgRatingView"/> 
            </div>
        </div>
    </div>
</template>

<script>
import BusinessClientNavBar from '@/components/BusinessClientNavBar.vue'
import BusinessClientEarningsReport from '@/components/BusinessClientEarningsReport.vue'
import BusinessClientReservationsReport from '@/components/BusinessClientReservationsReport.vue'
import BusinessClientAvgRatingReport from '@/components/BusinessClientAvgRatingReport.vue'
export default {
    name: 'StatisticsView',
    components: {
        BusinessClientNavBar,
        BusinessClientEarningsReport,
        BusinessClientReservationsReport,
        BusinessClientAvgRatingReport,
    },
    data() {
        return {
            earningsView: true,
            reservationsView: false,
            avgRatingView: false,
        }
    },
    methods: {
        earningsViewClicked() {
            this.earningsView = true
            this.reservationsView = false;
            this.avgRatingView = false;
        },
        reservationsViewClicked() {
            this.earningsView = false
            this.reservationsView = true;
            this.avgRatingView = false;
        },
        avgRatingViewClicked() {
            this.earningsView = false
            this.reservationsView = false;
            this.avgRatingView = true;
        }
    }
   

}
</script>

<style scoped>
.content {
  margin-top: 70px;
  padding: 0 5%;
}

.earnings-view {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
}
.reservations-view {
    border-radius:  0;
}

.avg-rating-view {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
}

</style>