<script>
import { RouterLink, RouterView } from 'vue-router';
import userFn from '../components/total/userFn.vue';
export default {
    components: {
        RouterLink,
        RouterView,
        userFn,
    },
    data() {
        return {
            startTime: "",
            endTime: "",
            titleSerach: "",
            testArr: [],
            showData: 10,
            dataPage: 1,
            
        }
    }, props: [
        'surveyId'
    ],
    methods: {
        dataInfo() {
            if (this.startTime !== "" && this.endTime !== "") {
                if (this.startTime > this.endTime) {
                    alert("別鬧了，朋友");
                    return;
                }
            }
            if(this.startTime == "" && this.endTime == "" && this.titleSerach == ""){
                alert("別鬧了，朋友");
                return;
            }
            let target={
                "survey_Title":this.titleSerach,
                "survey_StartTime":this.startTime,
                "survey_EndTime":this.endTime,
            }
            console.log(target)
            fetch("http://localhost:8080/search_Survey", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(target)
            })
                .then(res => {
                    return res.json();
                })
                .then(data => {
                    console.log(data);
                    this.testArr = data;
                })
                .catch(error => {
                    console.log(error);
                })
        },
        previousPage() {
            if (this.dataPage > 1) {
                this.dataPage--;
            }
        },
        nextPage() {
            if (this.dataPage < this.totalPages) {
                this.dataPage++;
            }
        },
        surveys() {
            let xx = {
                "survey_id": 0,
            }
            fetch("http://localhost:8080/front_All_Survey", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(xx)
            })
                .then(function (response) {
                    return response.json();
                })
                .then(data => {
                    console.log(data);
                    this.testArr = data;
                    this.testArr.forEach(item =>{
                        if(item.surveyCondition == "已關閉"){
                            item.surveyCondition = "未開放";
                        }
                    })
                    console.log(this.testArr)
                })
                .then(function (error) {
                    console.log(error);
                })
        },
        addAns(id,condition) {
            if(condition !== "投票中"){
                console.log(condition);
                return
            }
            let pk = {
                "survey_id": id,
            }
            this.$router.push('/userFn');
            sessionStorage.setItem("pk", JSON.stringify(pk));
            console.log(id)
        },
    },
    computed: {
        totalPages() {
            return Math.ceil(this.testArr.length / this.showData);
        },
        currentPageItems() {
            const startIndex = (this.dataPage - 1) * this.showData;
            const endIndex = startIndex + this.showData;
            return this.testArr.slice(startIndex, endIndex);
        },
    },
    created() {
        this.surveys();
    },
    updated() {

    }
}
</script>

<template>
    <div class="showArea">

        <!-- 上方功能區 -->
        <div class="topTitle">
            <div class="function">
                <div class="funTitle">
                    <span>問卷標題</span>
                    <input type="text" class="titleSearch" v-model="titleSerach">

                </div>
                <div class="time">
                    <span>開始/結束</span>
                    <input type="date" name="" id="startDate" class="Date" v-model="startTime">
                    <input type="date" name="" id="endDate" class="Date" v-model="endTime">
                    <button class="timeBtn" @click="dataInfo">搜尋</button>
                </div>
            </div>
        </div>

        <!-- 下方統計區 -->
        <div class="contentList">
            <div class="totalList ">
                <div class="listTitle">
                    <span class="Num">#</span>
                    <span class="Title">問卷</span>
                    <span class="Condition">狀態</span>
                    <span class="Time">開始時間</span>
                    <span class="Time">結束時間</span>
                    <span class="spanLast">觀看統計</span>
                </div>
                <div class="dataContent">
                    <div class="listContent" v-for="(item, index) in currentPageItems" :key="index">
                        <span class="Num2">{{ item.surveyId }}</span>

                        <span v-if="item.surveyCondition == '投票中'" @click="addAns(item.surveyId,item.surveyCondition)" class="Title2ing">{{item.surveyTitle }}</span>
                        <span v-if="item.surveyCondition !== '投票中'" class="Title2">{{item.surveyTitle }}</span>
                        
                        <span class="Condition2" v-if="item.surveyCondition !== '投票中'">{{ item.surveyCondition }}</span>
                        <span class="Condition2ing" v-if="item.surveyCondition == '投票中'">{{ item.surveyCondition }}</span>

                        <span class="Time2">{{ item.surveyStartTime }}</span>
                        <span class="Time2">{{ item.surveyEndTime }}</span>
                        <span class="spanLast2">觀看統計</span>
                    </div>
                </div>
                <div class="listBtn">
                    <button @click="previousPage" :disabled="currentPage === 1">上一頁</button>
                    <button @click="nextPage" :disabled="currentPage === totalPages">下一頁</button>
                </div>

            </div>
        </div>

    </div>
</template>

<style lang="scss" scoped>
.showArea {
    width: 100%;
    height: 85vh;

    .topTitle {
        width: 100%;
        height: 25vh;
        display: flex;
        justify-content: center;
        align-items: center;


        .function {
            width: 70%;
            height: 20vh;
            border: 3px solid black;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            box-shadow: 3px 3px 3px black;

            .funTitle {
                width: 80%;
                height: 10vh;
                font-size: 20pt;
                display: flex;
                justify-content: space-between;

                .titleSearch {
                    width: 80%;
                    height: 50%;
                    border: 3px solid black;
                    transition: 0.6s;

                    &:hover {
                        background-color: black;
                        color: white;
                    }
                }
            }

            .time {
                width: 80%;
                height: 5vh;
                display: flex;
                justify-content: space-between;

                .timeBtn {
                    font-size: 20pt;
                    width: 10%;
                    height: 6vh;
                    border: 3px solid black;
                    box-shadow: 5px 5px 5px black;
                    transition: 0.6s;

                    &:hover {
                        background-color: black;
                        color: white;
                    }
                }

                .Date {
                    border: 3px solid black;
                    font-size: 15pt;
                    width: 20%;
                    transition: 0.6s;

                    &:hover {
                        background-color: black;
                        color: white;
                    }
                }

                span {
                    font-size: 20pt;
                }
            }
        }
    }


    .contentList {
        width: 100%;
        height: 80vh;
        // background-color: #888;
        display: flex;
        justify-content: center;

        .totalList {
            width: 70%;
            height: 76vh;
            border: 3px solid black;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            position: relative;

            .listTitle {
                position: absolute;
                top: 0;
                width: 100%;
                height: 6vh;
                font-size: 20pt;
                display: flex;
                justify-content: space-between;
                align-items: center;
                background-color: #dddddd;

                .Num {
                    width: 5%;
                    padding-left: 6px;
                    height: 4vh;
                    border-right: 3px solid black;
                    ;
                }

                .Title {
                    width: 50%;
                    padding-left: 6px;
                    height: 4vh;
                    border-right: 3px solid black;
                    text-align: center;
                }

                .Condition {
                    width: 10%;
                    padding-left: 6px;
                    height: 4vh;
                    border-right: 3px solid black;
                }

                .Time {
                    width: 10%;
                    padding-left: 6px;
                    height: 4vh;
                    border-right: 3px solid black;
                }

                .spanLast {
                    width: 15%;
                    height: 4vh;
                    padding-left: 6px;

                }
            }

            .dataContent {
                position: absolute;
                top: 6vh;
                height: 62vh;
                width: 100%;

                .listContent {
                    height: 5.5vh;
                    width: 100%;
                    background-color: antiquewhite;
                    margin-bottom: 5px;
                    display: flex;
                    justify-content: space-between;
                    align-items: center;

                    .Num2 {
                        width: 5%;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        ;
                    }

                    .Title2 {
                        width: 50%;
                        color: gray;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        
                    }
                    .Title2ing{
                        font-size: 20pt;
                        color: blue;
                        width: 50%;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        transition: 0.5s;

                        &:hover{
                            color: white;
                            cursor: pointer;
                        }
                    }

                    .Condition2 {
                        width: 10%;
                        color: gray;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        ;
                    }

                    .Condition2ing{
                        width: 10%;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        color:red;
                    }

                    .Time2 {
                        width: 10%;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        ;
                    }

                    .spanLast2 {
                        width: 15%;
                        height: 4vh;
                        padding-left: 6px;
                    }

                }
            }

            .listBtn {
                width: 40%;
                height: 5vh;
                display: flex;
                justify-content: space-around;
                position: absolute;
                bottom: 20px;
            }

        }
    }
}
</style>