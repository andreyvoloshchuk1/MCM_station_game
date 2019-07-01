<template>
    <div id="content">
        <div class="circle">
            <div class="circle__item" v-for="(value, key) in rows" :class="[`circle__item_${value}`, `position_${key}`]">
                <div class="circle__text" v-html="t.buttons[value - 1]"></div>
            </div>
        </div>

        <div class="refresh" @touchend="$emit('next')">Try again</div>
    </div>
</template>

<script>
    import data from "@/data"
    
    export default {
        name: "slide-finish",
        data(){
            return {
                chooseItems: [null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null, null],
                t: data
            }
        },

        props: [
            "data",
            "rows"
        ],

        computed:{

        },

        mounted(){
            this.chooseItems.forEach((item, i) => {
                this.rows.forEach((current, index) => {
                    if (i === current){
                        this.$set(this.chooseItems, i, i)
                    }
                })
            })
        }
    }
</script>

<style scoped lang="scss">
    #content {
        width: 100%;
        height: 100%;

        position: absolute;
        top: 0;
        left: 0;

        background-image: url(../assets/result.png);
        background-size: 100% auto;
    }

    .circle {
        width: 1360px;
        height: 1360px;

        position: absolute;
        bottom: 300px;
        left: 50%;
        transform: translateX(-50%);

        border-radius: 50%;

        &__item {
            width: 250px;
            height: 250px;
            position: absolute;
            border-radius: 50%;
            background-size: 100% auto;
            background-position: center center;


            @for $i from 1 through 19 {
                &_#{$i} {
                    background-image: url('../assets/icons/#{$i}.png')
                }
            }
        }

        &__item_17{
            .circle__text {
                width: 700px !important;
            }
        }
        &__text {
            font-size: 42.5px;
            color: #63666A;

            position: absolute;
        }
    }

    .position{
        &_0 {
            left: 50%;
            transform: translateX(-50%);
            top: 0px;

            .circle__text {
                    width: 3000px;
                    text-align: center;
                    left: 50%;
                    transform: translateX(-50%);
                    bottom: 240px;
            }
        }
        &_1 {
            left: 390px;
            top: 30px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 206px;
                    bottom: 180px;
            }
        }
        &_2 {
            right: 390px;
            top: 30px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 206px;
                    bottom: 180px;
            }
        }
        &_3 {
            left: 220px;
            top: 90px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    bottom: 130px;
            }
        }
        &_4 {
            right: 220px;
            top: 90px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    bottom: 130px;
            }
        }
        &_5 {
            left: 90px;
            top: 198px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_6 {
            right: 90px;
            top: 198px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_7 {
            left: -35px;
            top: 340px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_8 {
            right: -35px;
            top: 340px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_9 {
            left: -100px;
            top: 495px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_10 {
            right: -100px;
            top: 495px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
         
        &_11 {
            left: -132px;
            top: 673px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_12 {
            right: -132px;
            top: 673px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_13 {
            left: -100px;
            top: 840px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_14 {
            right: -100px;
            top: 840px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_15 {
            left: -35px;
            top: 1000px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_16 {
            right: -35px;
            top: 1000px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_17 {
            left: 90px;
            top: 1140px;

            .circle__text {
                    width: 3000px;
                    text-align: right;
                    right: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }
        &_18 {
            right: 90px;
            top: 1140px;

            .circle__text {
                    width: 3000px;
                    text-align: left;
                    left: 250px;
                    top: 44%;
                    transform: translateY(-50%)
            }
        }

    }

    .refresh {
        width: 1224px;
        height: 186px;

        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        bottom: 66px;

        font-size: 85px;
        color: #fff;

        display: flex;
        align-items: center;
        justify-content: center;

        background-color: #7CCC6C;
        border-radius: 106px;
        box-shadow: 0px 8px 80px rgba(0, 156, 222, 0.2);
        transition: .1s linear opacity;
    }
</style>