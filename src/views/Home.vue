<script>
import { RouterLink, RouterView } from 'vue-router'
export default{
    components:{
        RouterLink, 
        RouterView,
    },
    data(){
        return{
            startTime:"",
            endTime:"",
            titleSerach:"",
            testArr:[],
            showData:10,
            dataPage:1,
        }
    },
    methods:{
        dataInfo(){
            console.log(this.startTime)
            console.log(this.endTime)
            if(this.startTime > this.endTime){
                alert("別鬧了，朋友");
            }

            
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
        surveys(){
            let xx = {
                "survey_id": 0,
            }
            fetch("http://localhost:8080/all_Survey", {
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
                    console.log(this.testArr)
                })
                .then(function (error) {
                    console.log(error);
                })
        },
        addAns(id){
            let pk = {
                "survey_id": id,
            }
            console.log(id)
            fetch("http://localhost:8080/get_Survey", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(pk)
            })
                .then(function (response) {
                    return response.json();
                })
                .then(data => {
                    console.log(data);
                    sessionStorage.setItem("Item",JSON.stringify(data));
                })
                .then(function (error) {
                    console.log(error);
                })
        }
    },
    computed:{
        totalPages() {
        return Math.ceil(this.testArr.length / this.showData);
    },
    currentPageItems() {
        const startIndex = (this.dataPage - 1) * this.showData;
        const endIndex = startIndex + this.showData;
        return this.testArr.slice(startIndex, endIndex);
    },
    },
    created(){
        this.surveys();
    },
    updated(){
        
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
                        <RouterLink to='/userFn' @click="addAns(item.surveyId)" class="Title2"><span  >{{ item.surveyTitle }}</span></RouterLink>
                        <span class="Condition2">{{ item.surveyCondition }}</span>
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
.showArea{
    width:100%;
    height: 85vh;

    .topTitle{
        width: 100%;
        height: 25vh;
        display: flex;
        justify-content: center;
        align-items: center;
        

        .function{
            width: 70%;
            height: 20vh;
            border: 3px solid black;
            display: flex;
            justify-content:center;
            align-items:center;
            flex-direction: column;
            box-shadow: 3px 3px 3px black;

            .funTitle{
                width:80%;
                height: 10vh;
                font-size: 20pt;
                display: flex;
                justify-content: space-between;
                
                .titleSearch{
                    width: 80%;
                    height: 50%;
                    border: 3px solid black;
                    transition: 0.6s;

                    &:hover{
                        background-color: black;
                        color: white;
                    }
                }
            }

            .time{
                width:80%;
                height: 5vh;
                display: flex;
                justify-content: space-between;

                .timeBtn{
                    font-size: 20pt;
                    width: 10%;
                    height: 6vh;
                    border: 3px solid black;
                    box-shadow: 5px 5px 5px black;
                    transition: 0.6s;

                    &:hover{
                        background-color: black;
                        color: white;
                    }
                }

                .Date{
                    border: 3px solid black;
                    font-size: 15pt;
                    width: 20%;
                    transition: 0.6s;

                    &:hover{
                        background-color: black;
                        color: white;
                    }
                }
                
                span{
                    font-size: 20pt;
                }
            }
        }
    }


    .contentList{
        width: 100%;
        height: 80vh;
        // background-color: #888;
        display: flex;
        justify-content: center;
        
        .totalList{
            width:70%;
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
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        ;
                    }

                    .Condition2 {
                        width: 10%;
                        padding-left: 6px;
                        height: 4vh;
                        border-right: 3px solid black;
                        ;
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

            .listBtn{
                width: 40%;
                height: 5vh;
                display: flex;
                justify-content: space-around;
                position: absolute;
                bottom:20px;
            }

        }
    }
}
</style>