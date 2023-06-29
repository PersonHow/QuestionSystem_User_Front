<script>
export default {
    data() {
        return {
            0: '',
            1: '',
            2: '',
            3: '',
            4: ''
        }
    },
    props: [
        'surveyData',
        'surveyQuestion',
        'surveySelected',
        'surveySelectedInfo',
        'userName',
        'userPhone',
        'userEmail',
        'userAge',
        'userAns1',
        'userAns2',
        'userAns3',
        'userAns4',
        'userAns0',
    ],
    methods: {

    },
    created() {
        this[0] = this.userAns0;
        this[1] = this.userAns1;
        this[2] = this.userAns2;
        this[3] = this.userAns3;
        this[4] = this.userAns4;
    }
}

</script>


<template>
    <div class="info">
        <div class="title">
            <div class="time">
                <span>{{ surveyData.surveyStartTime }} ~ {{ surveyData.surveyEndTime }}</span>
            </div>
            <div class="text">
                <span>{{ surveyData.surveyTitle }}</span>
            </div>
        </div>
        <div class="content">
            <span>{{ surveyData.surveyContent }}</span>
        </div>
        <div class="userInfo">
            <div class="text">
                <span>姓名：</span>
                <span>手機：</span>
                <span>Email：</span>
                <span>年齡：</span>
            </div>
            <div class="dataInput">
                <span>{{ userName }}</span>
                <span>{{ userPhone }}</span>
                <span>{{ userEmail }}</span>
                <span>{{ userAge }}</span>
            </div>
        </div>
        <div class="userAns">
            <div class="queArea">
                <div class="question" v-for="(item, index) in surveyQuestion" :key="index">
                    <span>{{ item }}</span>
                </div>
            </div>
            <div class="selectArea">
                <div class="selected" v-for="(elements, index) in surveySelected" :key="index">
                    <div class="input">
                        <input type="text" name="" id="" class="textIn" v-if="elements.includes('文字')" v-model=this[index]
                            disabled>
                        <span name="radioName" :id="'radioIn' + [index]"
                            v-if="this[index] !== '' && elements.includes('單選')" :key="index">{{ this[index] }}</span>
                        <span name="checkName" id="checkIn" v-if="this[index] !== '' && elements.includes('複選')"
                            :key="index">{{ this[index] }}</span>
                    </div>

                </div>
            </div>


        </div>
        <div class="sumbitBtn">
            <button class="btn cancel" @click="$emit('backPage')">取消</button>
            <button class="btn comfrim" @click="$emit('sentInfo')">送出</button>
        </div>
    </div>
</template>


<style lang="scss" scoped>
.info {
    width: 100%;
    height: 100vh;

    .title {
        width: 100%;
        height: 15vh;
        // border: 1px solid black;

        .time {
            font-size: 16pt;
            width: 100%;
            height: 30%;
            position: relative;

            span {
                position: absolute;
                right: 5%;
            }
        }

        .text {
            width: 100%;
            height: 70%;
            font-size: 30pt;
            position: relative;

            span {
                position: absolute;
                left: 40%;
                bottom: 5%;
            }
        }
    }

    .content {
        width: 100%;
        height: 15vh;
        // border: 1px solid black;

        span {
            width: 100%;
            height: 100%;
            font-size: 14pt;
            margin-left: 15%;
        }
    }

    .userInfo {
        width: 100%;
        height: 35vh;
        font-size: 16pt;
        display: flex;

        .text {
            width: 30%;
            height: 35vh;
            display: flex;
            justify-content: space-around;
            align-items: end;
            flex-direction: column;
            // border: 1px solid black;
        }

        .dataInput {
            width: 70%;
            height: 35vh;
            display: flex;
            justify-content: space-around;
            flex-direction: column;
            padding-left: 20px;

            input {
                width: 50%;
                border: 3px solid black;
                border-radius: 5pt;
            }
        }
    }

    .userAns {
        width: 90%;
        height: 25vh;
        display: flex;
        justify-content: space-around;
        align-items: center;
        flex-direction: column;
        overflow: auto;

        .queArea {
            width: 100%;
            height: 5vh;
            display: flex;
            justify-content: space-around;
        }

        .question {
            width: 20%;
            font-size: 16pt;
            font-weight: bolder;

        }

        .selectArea {
            width: 90%;
            height: 20vh;
            display: flex;
            justify-content: space-around;
            overflow: auto;

            span {
                font-size: 16pt;
            }

            .selected {
                height: 15vh;
                width: 20%;
                font-size: 12pt;
                display: flex;

                .input {
                    width: 50%;
                    height: 15vh;
                    display: flex;
                    justify-content: space-around;
                    align-items: start;
                    flex-direction: column;

                    .textIn {
                        border-radius: 5pt;
                    }
                }

                .res {
                    width: 50%;
                    height: 15vh;
                    display: flex;
                    justify-content: space-around;
                    align-items: start;
                    flex-direction: column;
                }
            }
        }

    }

    .sumbitBtn {
        width: 100%;
        height: 7vh;
        position: relative;

        .btn {
            width: 10%;
            height: 7vh;
            align-items: center;
            font-size: 20pt;
            border: 3px solid black;
            box-shadow: 5px 5px 5px black;
        }

        .cancel {
            position: absolute;
            left: 40%;
            transition: 0.6s;

            &:hover {
                background-color: black;
                color: white;
                font-size: 24pt;
            }
        }

        .comfrim {
            transition: 0.6s;
            position: absolute;
            right: 5%;
            background-color: rgb(2, 252, 35);

            &:hover {
                background-color: black;
                color: white;
                font-size: 24pt;
            }
        }
    }
}
</style>