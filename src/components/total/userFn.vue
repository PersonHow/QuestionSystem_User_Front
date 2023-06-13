<script>
import addAns from '../User/addAns.vue';
import showAns from '../User/showAns.vue';

export default {
    data() {
        return {
            isShow: false,
            surveyData: "",
            surveyQuestion: "",
            surveySelected: "",
            surveySelectedInfo: "",
            userName: "",
            userPhone: "",
            userEmail: "",
            userAge: "",
            textInput: "",
            radioInput: "",
            checkInput: "",
        }
    },
    components: {
        addAns,
        showAns,
    },
    methods: {
        comfrim() {
            this.isShow = !this.isShow;
        },
        cancel(){
            this.isShow = !this.isShow;   
        }
    },
    mounted() {


    },
    created() {
        this.surveyData = JSON.parse(sessionStorage.getItem('Item')).surveyInfo;
        console.log(this.surveyData)
        this.surveyQuestion = this.surveyData.surveyQuestion.split(',');
        this.surveySelected = this.surveyData.surveyAnswer.split('|');
        console.log(this.surveySelected);
    },
    updated() {
        // console.log(this.userName);
        // console.log(this.userPhone);
        // console.log(this.userEmail);
        // console.log(this.userAge);
        // console.log(this.radioInput);
        // console.log(this.checkInput)
    }
}
</script>

<template>
    <div class="info">
        <addAns v-if="isShow == false" v-model:surveyData="surveyData" v-model:surveyQuestion="surveyQuestion"
            v-model:surveySelected="surveySelected" v-model:surveySelectedInfo="surveySelectedInfo"
            v-model:userName="userName" v-model:userPhone="userPhone" v-model:userEmail="userEmail"
            v-model:userAge="userAge" v-model:textInput="textInput" v-model:radioInput="radioInput"
            v-model:checkInput="checkInput" @nextPage="comfrim" />

        <showAns v-if="isShow == true" v-bind:surveyData="surveyData" v-bind:surveyQuestion="surveyQuestion"
        v-bind:surveySelected="surveySelected" v-bind:surveySelectedInfo="surveySelectedInfo"
        v-bind:userName="userName" v-bind:userPhone="userPhone" v-bind:userEmail="userEmail"
        v-bind:userAge="userAge" v-bind:textInput="textInput" v-bind:radioInput="radioInput"
        v-bind:checkInput="checkInput" 
            @backPage="cancel"/>
    </div>
</template>


<style lang="scss" scoped>
.info {
    width: 100%;
    height: 101vh;
}
</style>