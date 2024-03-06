<template>
    <div>

        <div id="next_container" @click="nextSelected()"> 
        
        <div id="next_text">NEXT</div>
        <div id="next_button" >
        <svg height="150%" stroke-miterlimit="10" style="fill-rule:nonzero;clip-rule:evenodd;stroke-linecap:round;stroke-linejoin:round;" version="1.1" viewBox="914.054 531.043 143.575 143.575" width="100%" xml:space="preserve" xmlns="http://www.w3.org/2000/svg" xmlns:vectornator="http://vectornator.io" xmlns:xlink="http://www.w3.org/1999/xlink">
            <defs/>
            <clipPath id="ArtboardFrame">
            <rect height="143.575" width="143.575" x="914.054" y="531.043"/>
            </clipPath>
            <g clip-path="url(#ArtboardFrame)" id="Layer" vectornator:layerName="Layer">
            <g opacity="1" vectornator:layerName="Gruppieren 31">
            <path :fill="color1" d="M914.588 602.83C914.588 563.478 946.489 531.577 985.841 531.577C1025.19 531.577 1057.09 563.478 1057.09 602.83C1057.09 642.182 1025.19 674.083 985.841 674.083C946.489 674.083 914.588 642.182 914.588 602.83Z" fill-rule="nonzero" opacity="1" stroke="#000000" stroke-linecap="butt" stroke-linejoin="round" stroke-opacity="0.975225" stroke-width="1.06879" vectornator:layerName="Oval 4"/>
            <path :fill="color2" d="M929.689 602.83C929.689 571.818 954.829 546.677 985.841 546.677C1016.85 546.677 1041.99 571.818 1041.99 602.83C1041.99 633.843 1016.85 658.983 985.841 658.983C954.829 658.983 929.689 633.843 929.689 602.83Z" fill-rule="nonzero" opacity="1" stroke="#000000" stroke-linecap="butt" stroke-linejoin="round" stroke-opacity="0.975225" stroke-width="1.06879" vectornator:layerName="Oval 5"/>
            <path :fill="color3" d="M935.119 602.83C935.119 574.817 957.828 552.107 985.841 552.107C1013.85 552.107 1036.56 574.817 1036.56 602.83C1036.56 630.844 1013.85 653.553 985.841 653.553C957.828 653.553 935.119 630.844 935.119 602.83Z" fill-rule="nonzero" opacity="1" stroke="#000000" stroke-linecap="butt" stroke-linejoin="round" stroke-opacity="0.975225" stroke-width="1.06879" vectornator:layerName="Oval 6"/>
            </g>
            </g>
        </svg>
        </div>
        </div>  
    </div>
</template>

<script>

let colors = ['FFE6A9','FFCEA3', 'FFAB9A', 'FF9BA1','FF889F', 'FF5F8D', 'FF2472' ]
// let colors = ['FF2472', 'FF5F8D', 'FF889F', 'FF9BA1', 'FFAB9A', 'FFCEA3', 'FFE6A9']
    
export default {
    name: 'NextComponent',
    props:
    {
        nextDifficulty: {
            type: Number,
            required: true
        }
    }, 
    data() {
        return {
            choiceTimerId: null,
            time: 0,
            color1: '#'+colors[Math.floor(Math.random() * colors.length)],
            color2: '#'+colors[Math.floor(Math.random() * colors.length)],
            color3: '#'+colors[Math.floor(Math.random() * colors.length)],
            points : [3, 6, 12, 20, 30, 45, 75],
            difficulties: colors.map((color, index) => {
                    return {    
                        id: colors.length-index,
                        color: color,
                        opacity: 1,
                    }
            }),
        }

    },
    mounted(){
        this.choiceTimerId = setInterval(() => {
            this.time += 100
        }, 100);
        },
    unmounted(){
        clearInterval(this.choiceTimerId);
        },

    methods: {
        nextSelected(){
            console.log('nextSelected')
            clearInterval(this.choiceTimerId)
            console.log('nextDifficulty', this.nextDifficulty)
            this.$emit('task-selected',this.nextDifficulty, this.time)
        }
        } 
    }
</script>

<style scoped>
#next_container {
    position: relative;
    /* top: 15vh; */
    height: 80vh;
}
#next_button {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 70%;
}
#next_text {
    z-index: 1;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 9vh;
    color: black;
    font-family: Kodchasan;
    position: relative;
    letter-spacing: 0.1em;
    font-weight: 700;

    font-size: 40px;
}
</style>
