<template>
    <div class="content">
        <div class="header">
            <div class="mask">
                <person class="header__person" @touchend="openPopup(1)"/>
            </div>
            <product class="header__product" @touchend="openPopup(2)"/>
            <icon class="header__icon" />
        </div>

        <div class="calendar" ref="calendar">
            <div class="calendar__head">
                <div class="calendar__number" v-for="value in 12" v-html="value"></div>
            </div>
            <div class="calendar__table">
                <div class="calendar__column" v-for="value in 12" ref="column"></div>
                <div class="calendar__wrap" ref="wrap">
                    <div class="calendar__row" v-for="value in 8" ref="row"></div>
                </div>
            </div>
        </div>

        <div v-for="value in 19" class="category__item" :ref="`category__item_static_${value}`" :class="[`category__item_${value}`, {green: value <= 9}]" v-html="t.buttons[value - 1]" @touchstart="startDrag(value, $event)" @touchmove="moveDrag(value, $event)" @touchend="stopDrag(value, $event)"></div>
        <transition-group name="fade">
            <div draggable="true" v-for="value in 19" class="category__item category__item_drag" :ref="`category__item_${value}`" :class="[`category__item_${value}`, `category__item_drag_${value}`, {green: value <= 9}]" v-html="t.buttons[value - 1]" :key="`category_${value}`" v-show="dragItem[value - 1]"></div>
        </transition-group>

        <timer class="timer"/>
        <reminder class="reminder"/>
        <div class="month" v-html="t.month"></div>

        <div class="next-btn" :class="{disabled: isDisable}" v-html="t.nextBtn" @touchend.stop="next"></div>

        <transition name="fade">
            <div class="popup" v-if="isOpenPopup" :class="[`popup_${isOpenPopup}`]">
                <div class="close" @touchend="isOpenPopup = false"></div>
            </div>
        </transition>
    </div>
</template>

<script>
    import data from "@/data"
    import person from "@/assets/maria.svg"
    import product from "@/assets/duphalac.svg"
    import icon from "@/assets/icon.svg"
    import reminder from "@/assets/stage_reminder.svg"
    import timer from "@/assets/timer.svg"

    export default {
        name: "game",

        data(){
          return {
              t: data,

              dragItem: [false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false],
              table: [],

              currentX: Number,
              currentY: Number,

              startX: Number,
              startY: Number,

              currentText: String,

              dropZones: [],
              tableData: [],

              currentElement: null,
              prevItems: 0,
              nextItems: 0,

              isDisable: true,

              isOpenPopup: false,

              rows: []
          }
        },

        methods:{
            startDrag(key, e){
                this.$set(this.dragItem, key - 1, true);
                this.cuurentText = this.t.buttons[key - 1];

                this.startX = e.target.offsetLeft;
                this.startY = e.target.offsetTop;

                this.$refs[`category__item_${key}`][0].style.left = `${this.startX}px`;
                this.$refs[`category__item_${key}`][0].style.top = `${this.startY}px`;
            },

            moveDrag(key, e){
                this.currentX = e.changedTouches[0].clientX - 168;
                this.currentY = e.changedTouches[0].clientY - 80;

                this.$refs[`category__item_${key}`][0].style.left = `${this.currentX}px`;
                this.$refs[`category__item_${key}`][0].style.top = `${this.currentY}px`;
            },
            stopDrag(key, e){
                if ((this.dropZones[1][1][0] < this.$refs[`category__item_${key}`][0].offsetTop) && (this.dropZones[1][1][1] > (this.$refs[`category__item_${key}`][0].offsetTop))) {
                    this.dropZones.forEach((item, i) => {
                        if (item[0][0] - 130 < this.$refs[`category__item_${key}`][0].offsetLeft && item[0][1] - 130 > this.$refs[`category__item_${key}`][0].offsetLeft){
                            const element = document.createElement("div"),
                                  text = document.createElement("span");
                            let currentRow = 0,
                                lineSize = 0,
                                linePosition = 0,
                                self = this;
                            let currentElement = Math.ceil((e.changedTouches[0].clientX - 180) / 288) - 1;

                            text.textContent = this.cuurentText;
                            element.append(text);

                            this.isDisable = false;

                            // let inRows = false;
                            // for (let i = 0; i <= this.rows.length; i++){
                            //     if (this.rows[i] === key){
                            //         currentRow = i;
                            //         break;
                            //     }
                            //     else if (i === this.rows.length){
                            //         for(let index = 0; index <= this.rows.length; index++){
                            //             if (!this.rows[index]){
                            //                 this.rows[index] = key;
                            //                 currentRow = index;
                            //                 break
                            //             }
                            //         }
                            //         break;
                            //     }
                            // }

                            // if (this.tableData[i][currentRow]){
                            //     return
                            // }
                            for(let i = 0; i <=this.tableData[currentElement].length; i++){
                                if (this.tableData[currentElement][i] === key){
                                    return
                                }
                            }
                            for(let current of this.tableData[i]){
                                if (current !== null){
                                    currentRow++;
                                }
                                else {
                                    break
                                }
                            }

                            if (currentRow >= 8){
                                let row = document.createElement("div");
                                row.classList.add(`calendar__row`);
                                document.getElementsByClassName("calendar__wrap")[0].append(row);
                            }

                            this.tableData[i][currentRow] = key;

                            for(let index = i; index < this.tableData.length - 1; index++){
                                if (this.tableData[index][currentRow] === this.tableData[index + 1][currentRow] && this.tableData[index + 1][currentRow] !== null){
                                    lineSize++;
                                }
                                else {
                                    break
                                }
                            }
                            for (let index = i; index > 0; index--){
                                if (this.tableData[index][currentRow] === this.tableData[index - 1][currentRow] && this.tableData[index - 1][currentRow] !== null){
                                    lineSize++;
                                    linePosition = index - 1;
                                }
                                else {
                                    linePosition = index;
                                    break
                                }
                            }

                            if (lineSize > 0){
                                let startItem = document.getElementsByClassName(`item_${currentRow}_${linePosition}`)[0];
                                if (!startItem){
                                    element.classList.add("calendar__item", `calendar__item_${key}`, `item_${currentRow}_${i}`);
                                    element.style.left = `${this.$refs.column[0].offsetWidth * i}px`;
                                    this.$refs.row[currentRow].append(element);
                                    document.getElementsByClassName(`item_${currentRow}_${linePosition}`)[0].style.width = `${(lineSize + 1) * 288}px`;
                                    document.querySelector(`.item_${currentRow}_${linePosition + 1}`).remove();
                                }
                                else {
                                    if (document.querySelector(`.item_${currentRow}_${currentElement + 1}`) && document.querySelector(`.item_${currentRow}_${currentElement + 1}`).textContent === document.querySelector(`.item_${currentRow}_${currentElement - 1}`).textContent){
                                        document.querySelector(`.item_${currentRow}_${currentElement + 1}`).remove();
                                    }
                                    startItem.style.width = `${(lineSize + 1) * 288}px`;
                                }
                            }
                            else {
                                element.classList.add("calendar__item", `calendar__item_${key}`, `item_${currentRow}_${i}`);
                                element.style.left = `${this.$refs.column[0].offsetWidth * i}px`;
                                document.getElementsByClassName("calendar__row")[`${currentRow}`].append(element);
                            }

                            function removeItem(e){
                                let currentElement = Math.ceil((e.changedTouches[0].clientX - 180) / 288) - 1,
                                    nextElement = 0,
                                    prevElement = 0;

                                for(let index = currentElement; index < self.tableData.length - 1; index++){
                                    if (self.tableData[index][currentRow] === self.tableData[index + 1][currentRow] && self.tableData[index + 1][currentRow] !== null){
                                        nextElement++
                                    }
                                    else {
                                        break
                                    }
                                }

                                for (let index = currentElement; index > 0; index--){
                                    if (self.tableData[index][currentRow] === self.tableData[index - 1][currentRow] && self.tableData[index - 1][currentRow] !== null){
                                        prevElement++
                                    }
                                    else {
                                        break
                                    }
                                }

                                self.currentElement = currentElement;
                                self.prevItems = prevElement;
                                self.nextItems = nextElement;

                                document.querySelector(`.item_${currentRow}_${currentElement - prevElement}`).remove();
                                self.tableData[currentElement][currentRow] = null;

                                for (let i = 0; i < self.tableData.length; i++){
                                    for(let index = 0; index < self.tableData[i].length; index++){
                                        if (self.tableData[i][index] !== null){
                                            self.isDisable = false;
                                            break
                                        }
                                        else {
                                            self.isDisable = true;
                                        }
                                    }
                                    if (self.isDisable === false){
                                        break
                                    }
                                }

                                createLines(e);

                            }
                    

                            element.addEventListener("touchend", removeItem);

                            function createLines(e){
                                let line = document.createElement("div"),
                                    text = document.createElement("span");
                                text.innerHTML = e.target.innerText;
                                line.append(text);
                                let lineSecond = line.cloneNode(true);

                                line.classList.add("calendar__item", `calendar__item_${key}`, `item_${currentRow}_${self.currentElement - self.prevItems}`);
                                lineSecond.classList.add("calendar__item", `calendar__item_${key}`, `item_${currentRow}_${self.currentElement + 1}`);

                                if (self.prevItems > 0){
                                    line.style.width = `${self.prevItems * 288}px`;
                                    line.style.left = `${self.$refs.column[0].offsetWidth * (self.currentElement - self.prevItems)}px`;
                                    self.$refs.row[currentRow].append(line);
                                    line.addEventListener("touchend", removeItem);
                                }
                                if (self.nextItems > 0){
                                    lineSecond.style.width = `${self.nextItems * 288}px`;
                                    lineSecond.style.left = `${self.$refs.column[0].offsetWidth * (self.currentElement + 1)}px`;
                                    self.$refs.row[currentRow].append(lineSecond);
                                    lineSecond.addEventListener("touchend", removeItem);
                                }

                                console.log(self.rows);
                            }
                        }
                    });
                }
                this.$set(this.dragItem, key - 1, false);
            },

            next(){
                if (this.isDisable){
                    return
                }
                else {
                    for (let i = 0;i <= this.tableData.length - 1; i++){
                        for (let j = 0; j <= this.tableData[i].length - 1; j++){
                            if(this.tableData[i][j] !== null){
                                for (let x = 0; x <= this.rows.length; x++){
                                    if (this.tableData[i][j] === this.rows[x]){
                                        break
                                    }
                                    else if (x === this.rows.length){
                                        this.rows.push(this.tableData[i][j]) 
                                        break;   
                                    }
                                } 
                            }
                       }
                    }
                    this.$emit("next", this.tableData, this.rows)
                }
            },

            openPopup(key){
                this.isOpenPopup = key;
            }
        },

        mounted(){
            let xLeft = Number,
                xRight = Number,
                yTop = Number,
                yBottom = Number;
            this.$refs.column.forEach((item, i) => {
                xLeft = item.offsetParent.offsetParent.offsetLeft + (item.clientWidth * i);
                xRight = item.offsetParent.offsetParent.offsetLeft + (item.clientWidth * (i + 1));
                yTop = item.offsetParent.offsetParent.offsetTop;
                yBottom = item.offsetParent.offsetParent.offsetTop + item.clientHeight;

                this.dropZones.push([[xLeft, xRight], [yTop, yBottom]]);

                this.tableData.push([null, null, null, null, null, null, null, null]);
            });
        },

        components: {
            person,
            product,
            icon,
            reminder,
            timer
        }
    }
</script>

<style lang="scss">
    .content {
        position: absolute;
        top: 0;
        left: 0;

        width: 100%;
        height: 100%;
    }

    .mask {
        width: 1642px;
        height: 373px;

        position: absolute;
        left: 160px;
        top: 53px;
    }
    .header {
        &__person {
            position: absolute;
            top: -44px;
            left: -94px;
        }
        &__product {
            width: 1729px;
            height: 399px;

            position: absolute;
            top: 80px;
            left: 2035px;
        }

        &__icon {
            width: 347px;

            position: absolute;
            top: 42px;
            right: 94.6px;
        }
    }

    .calendar {
        width: 3520px;
        height: 798px;

        box-shadow: 0px 10px 160px rgba(0, 156, 222, 0.25);
        border-radius: 50px;

        position: absolute;
        left: 160px;
        top: 558px;

        &__head {
            width: 3333px;
            height: 128px;
            margin-bottom: 42.56px;

            position: absolute;
            top: -58.42px;
            left: 50%;
            transform: translateX(-50%);

            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        &__number {
            width: 128px;
            height: 128px;
            
            box-sizing: border-box;
            border: 10.64px solid #fff;
            border-radius: 50%;

            background-color: #D9D9D6;
            color: #fff;

            display: flex;
            justify-content: center;
            align-items: center;

            font-size: 53.2px;
        }

        &__table {
            width: 3458px;
            height: 673px;

            position: absolute;
            left: 50%;
            top: 114.4px;
            transform: translateX(-50%);

            display: flex;
            align-items: stretch;
            flex-wrap: wrap;
            overflow-y: scroll;
            overflow-x: hidden;
        }

        &__column {
            width: 288px;
            height: 100%;
            padding: 0 4px;
            box-sizing: border-box;

            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;

            background-image: url(../assets/Vector.png);
            background-size: auto 100%;
            background-repeat: no-repeat;
            background-position: top -20px right;

            &:nth-child(12) {
                background-image: none;
            }
        }

        &__wrap {
            width: 100%;
            height: 100%;

            position: absolute;
            top: 0;
            left: 0;
        }

        &__row {
            width: 100%;
            height: 78px;

            box-sizing: border-box;
            margin: 4px 0;
            position: relative;
        }
    }

    .calendar__item {
        width: 288px;
        height: 100%;

        border-radius: 40px;
        background-color: red;

        display: inline-block;
        position: absolute;

        font-size: 36.4px;
        color: #fff;

        span {
            width: 100%;
            box-sizing: border-box;
            display: block;
            padding: 0 40px;
            text-align: center;
            position: absolute;
            top: 50%;
            transform: translateY(-50%);

            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        &_1, &_2, &_3, &_4, &_5, &_6, &_7, &_8, &_9 {
            background: #72CA92;
            box-shadow: 0px 10.64px 53px rgba(4, 222, 0, 0.2);
        }

        &_10, &_11 {
            background: linear-gradient(114.51deg, #72CA92 30.1%, #729ACA 70.62%);
            box-shadow: 0px 10.64px 53px rgba(0, 182, 222, 0.2);
        }
        &_12 {
            background: #729ACA;
            box-shadow: 0px 10.64px 53px rgba(0, 182, 222, 0.2);
        }
        &_13, &_14 {
            background: linear-gradient(116.2deg, #729ACA 28.83%, #9577A6 73.89%);
            box-shadow: 0px 10.64px 53px rgba(149, 119, 166, 0.2);
        }
        &_15, &_16 {
            background: #9577A6;
            box-shadow: 0px 10.64px 53px rgba(149, 119, 166, 0.2);
        }
        &_17 {
            background: #E47287;
            box-shadow: 0px 10.64px 53px rgba(222, 0, 107, 0.2);
        }
        &_18 {

            background: linear-gradient(112.33deg, #E47287 25.53%, #E9CB8F 71.6%);
            box-shadow: 0px 10.64px 53px rgba(217, 169, 73, 0.2);
        }
        &_19 {
            background: #E9CB8F;
            box-shadow: 0px 10.64px 53px rgba(217, 169, 73, 0.2);
        }
    }

    .category {
        width: 3547px;
        height: 348px;

        position: absolute;
        bottom: 346px;
        left: 160px;

        display: flex;
        align-content: space-between;
        justify-content: center;
        flex-wrap: wrap;

        &__wrap {
            width: 336px;
            height: 160px;
            margin: 0 8px;
            position: relative;
        }

        &__item {
            width: 336px;
            height: 160px;

            border-radius: 53.2px;

            display: flex;
            align-items: center;
            justify-content: center;
            box-sizing: border-box;
            padding: 0 14px;

            font-size: 32px;
            line-height: 37.24px;
            text-align: center;
            color: #fff;

            position: absolute;

            &_1 {
                bottom: 524px;
                left: 146px;
            }
            &_2 {
                bottom: 524px;
                left: 500px;
            }
            &_3 {
                bottom: 524px;
                left: 856.5px;
            }
            &_4 {
                bottom: 524px;
                left: 1210.3px;
            }
            &_5 {
                bottom: 524px;
                left: 1564px;
            }
            &_6 {
                bottom: 524px;
                left: 1920.5px;
            }
            &_7 {
                bottom: 524px;
                left: 2274.3px;
            }
            &_8 {
                bottom: 524px;
                left: 2628px;
            }
            &_9 {
                bottom: 524px;
                left: 2984.5px;
            }
            &_10 {
                bottom: 524px;
                left: 3338.3px;
            }
            &_11 {
                bottom: 345.8px;
                left: 324.5px;
            }
            &_12 {
                bottom: 345.8px;
                left: 675.6px;
            }
            &_13 {
                bottom: 345.8px;
                left: 1029.4px;
            }
            &_14 {
                bottom: 345.8px;
                left: 1385.86px;
            }
            &_15 {
                bottom: 345.8px;
                left: 1742.3px;
            }
            &_16 {
                bottom: 345.8px;
                left: 2099px;
            }
            &_17 {
                bottom: 345.8px;
                left: 2454.5px;
            }
            &_18 {
                bottom: 345.8px;
                left: 2806.3px;
            }
            &_19 {
                bottom: 345.8px;
                left: 3160px;
            }

            &.green {
                background: #72CA92;
                box-shadow: 0px 10.64px 53px rgba(4, 222, 0, 0.2);
            }

            &_10, &_11 {
                background: linear-gradient(114.51deg, #72CA92 30.1%, #729ACA 70.62%);
                box-shadow: 0px 10.64px 53px rgba(0, 182, 222, 0.2);
            }
            &_12 {
                background: #729ACA;
                box-shadow: 0px 10.64px 53px rgba(0, 182, 222, 0.2);
            }
            &_13, &_14 {
                background: linear-gradient(116.2deg, #729ACA 28.83%, #9577A6 73.89%);
                box-shadow: 0px 10.64px 53px rgba(149, 119, 166, 0.2);
            }
            &_15, &_16 {
                background: #9577A6;
                box-shadow: 0px 10.64px 53px rgba(149, 119, 166, 0.2);
            }
            &_17 {
                background: #E47287;
                box-shadow: 0px 10.64px 53px rgba(222, 0, 107, 0.2);
            }
            &_18 {

                background: linear-gradient(112.33deg, #E47287 25.53%, #E9CB8F 71.6%);
                box-shadow: 0px 10.64px 53px rgba(217, 169, 73, 0.2);
            }
            &_19 {
                background: #E9CB8F;
                box-shadow: 0px 10.64px 53px rgba(217, 169, 73, 0.2);
            }

            &_drag {
                &.green {
                    box-shadow: 0px 20.8px 53px #72CA93;
                }

                &_10, &_11 {
                    box-shadow: 0px 20.8px 53px #729ACA;;
                }
                &_12 {
                    box-shadow: 0px 20.8px 53px #729ACA;
                }
                &_13, &_14 {
                    box-shadow: 0px 20.8px 53px #9577A6;
                }
                &_15, &_16 {
                    box-shadow: 0px 20.8px 53px #9577A6;
                }
                &_17 {
                    box-shadow: 0px 20.8px 53px #E47287;
                }
                &_18 {
                    box-shadow: 0px 20.8px 53px #E9CB8F;
                }
                &_19 {
                    box-shadow: 0px 20.8px 53px #E9CB8F;
                }

            }
        }
    }

    .timer {
        width: 340px;

        position: absolute;
        right: 96px;
        bottom: 90.5px;
    }

    .reminder {
        width: 596px;

        position: absolute;
        left: 146px;
        bottom: 154px;
    }

    .next-btn {
        width: 1224px;
        height: 186px;

        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        bottom: 80px;

        font-size: 85px;
        color: #fff;

        display: flex;
        align-items: center;
        justify-content: center;

        background-color: #7CCC6C;
        border-radius: 106px;
        box-shadow: 0px 8px 80px rgba(0, 156, 222, 0.2);
        transition: .1s linear opacity;

        &.disabled {
            opacity: 0.3;
        }
    }

    .month {
        font-size: 50.5px;
        color: #888B8D;
        opacity: 0.75;

        position: absolute;
        top: 450px;
        left: 80px;
    }

    .popup {
        &_1 {
            width: 1516px;
            height: 1490px;

            position: absolute;
            top: 45px;
            left: 67px;

            background-image: url(../assets/popup_TA.png);
            background-size: auto 100%;
            box-shadow: 0px 10px 150px rgba(0, 156, 222, 0.4);
            background-repeat: no-repeat;
            border-radius: 40px;

        }
        &_2 {
            width: 2026px;
            height: 1490px;

            position: absolute;
            top: 45px;
            right: 165px;

            background-image: url(../assets/popup_Duphalac.png);
            background-size: auto 100%;
            box-shadow: 0px 10px 150px rgba(0, 156, 222, 0.4);
            background-repeat: no-repeat;
            border-radius: 40px;
        }
        .close {
            width: 70px;
            height: 70px;

            position: absolute;
            top: 80px;
            right: 80px;

            &:before, &:after {
                content: "";
                width: 100%;
                height: 2px;

                background-color: #000;
                position: absolute;
                left: 50%;
                top: 50%;
                transform: translate(-50%, -50%) rotate(45deg);
            }
            &:after {
                transform: translate(-50%, -50%) rotate(-45deg);
            }
        }
    }
</style>