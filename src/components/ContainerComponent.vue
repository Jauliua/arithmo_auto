<template>
    <div ontouchstart="" class="container">
    <!-- <KeepAlive :exclude="['GameComponent']"> -->
        <component :is="currentComponent" v-bind="currentProperties" v-on="currentEvents" class="main-component"/>
    <!-- </KeepAlive> -->
    </div>
</template>

<script>
import HomeComponent from './HomeComponent.vue';
import GameComponent from './GameComponent.vue';
let nextPoints = 0
export default {
    name: 'ContainerComp',
    components: {
        HomeComponent,
        GameComponent
    },
    data() {
        return {
            currentComponent: 'HomeComponent',
            missionPoints: this.$store.state.start_mission_points,
            prev_success_in_a_row: this.$store.state.success_in_a_row
        };
    },
    computed: {
        nth_mission: function(){
            return this.$store.state.nth_mission
        },
        success_in_a_row: function(){
            return this.$store.state.success_in_a_row
        },
        rank_now : function(){
            return this.$store.state.rank
        },
        
        
        currentProperties: function() {
            if(this.currentComponent === 'GameComponent'){
                return{ missionPoints: this.missionPoints, missionTime: 120}
            }
            else{
                return { missionPoints: this.missionPoints, missionTime: 120, 'prev_success_in_a_row': this.prev_success_in_a_row}
            }
        },
        currentEvents: function() {
            if (this.currentComponent === 'HomeComponent') {
                return { 'start-game': this.startMission, 'return-landing': this.returnLanding }
            }
            if (this.currentComponent === 'GameComponent') {
                return { 'return-home': this.returnHome }
            }
            return {};
        }
    },
    methods: {
        startMission() {
            this.currentComponent = 'GameComponent';
        },
        returnHome(missionPoints, achievedPoints, missionTime, usedTime, aborted) {
            this.prev_success_in_a_row = this.success_in_a_row;
            
            if (missionPoints){
                this.$store.commit('setPastAchievedPoints', achievedPoints)
                this.$store.commit('setPastResponseTimes', usedTime)
                if (aborted){
                    this.missionPoints = missionPoints
                }
                else if (achievedPoints >= missionPoints) {
                    this.$store.commit('setSuccessesInARow', this.prev_success_in_a_row+1==3? 0: this.prev_success_in_a_row+1)
                    nextPoints = (usedTime<=30)? 15 : (usedTime<=60)? 10: (usedTime<=90)? 5: 0;
                    this.missionPoints = missionPoints + nextPoints;
                } 
                else {
                    this.$store.commit('setSuccessesInARow', this.prev_success_in_a_row-1<=0? 0: this.prev_success_in_a_row-1)
                    nextPoints = (missionPoints-achievedPoints>=20) ? 15 : (missionPoints-achievedPoints>=10) ? 10 : 0;
                    this.missionPoints = Math.max(15, missionPoints - nextPoints);
                }

                fetch('https://taskdifficulty.robert-spang.de/next_task', {
                // fetch('http://127.0.0.1:5051/next_task', {
                
                    method: 'POST',
                    headers: {
                    'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        "session_id": this.$store.state.sessionID,
                        "log_mission_data": "true",
                        "nth_mission": this.nth_mission,
                        "mission_time": missionTime,
                        "mission_points": missionPoints,
                        "success": achievedPoints >= missionPoints,
                        "points_achieved": achievedPoints,
                        "response_time": usedTime,
                        "rank_now": this.prev_success_in_a_row+1==3 ? this.rank_now+1 : this.rank_now,
                        "successes_in_a_row_now": this.$store.state.success_in_a_row,
                        "skips": this.$store.state.skips,
                        "aborted": aborted.toString(),
                        "timestamp": new Date().toISOString(),
                        "game_type": "dda"
                    })
                })
                .then(response => response.json())
                .then(data => {

                })
                .catch((error) => {
                    console.error('Error:', error);
                });


            }
            this.currentComponent = 'HomeComponent';
        },

        returnLanding() {
            this.$emit('return-landing');
        }
    }
};
</script>

<style>
.container {
    height: 100vh;
    display: flex;
    flex-direction: column;
}

.navigation-bar{
    height: 5vh;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-left: 10px;
    padding-top: 5px;
    padding-bottom: 2px;
}
.exit-button,
.more-button {
    cursor: pointer;
    padding: 5px;
}

.main-component {
    flex-grow: 1;
}





</style>
