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
            userAns0: "",
            userAns1: "",
            userAns2: "",
            userAns3: "",
            userAns4: "",
        }
    },
    components: {
        addAns,
        showAns,
    },
    props: {
    },
    methods: {
        addAns() {

            let pk = JSON.parse(sessionStorage.getItem("pk"));
            console.log(pk)
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
                    this.surveyData = data.surveyInfo;
                    this.surveyQuestion = this.surveyData.surveyQuestion.split(',');
                    this.surveySelected = this.surveyData.surveyAnswer.split('|');
                    console.log(this.surveyData)
                })
                .then(function (error) {
                    console.log(error);
                })
        },
        comfrim() {
            this.isShow = !this.isShow;
        },
        cancel() {
            if (window.confirm("資料或答案是否重填") == true) {
                this.isShow = !this.isShow;
            }
        },
        submitInfo() {
            if (window.confirm("確認送出問卷嗎??") == true) {
                let personInfo = {
                    "personInfo": {
                        "email": this.userEmail,
                        "name": this.userName,
                        "phone": this.userPhone,
                        "age": this.userAge,
                        "surveyNum": this.surveyData.surveyId
                    }
                }
                console.log(personInfo);

                let personAns = {
                    "personAns": {
                        "surveyId": this.surveyData.surveyId,
                        "userEmail": this.userEmail,
                        "user1Ans": this.userAns0,
                        "user2Ans": this.userAns1,
                        "user3Ans": this.userAns2,
                        "user4Ans": this.userAns3,
                        "user5Ans": this.userAns4,
                    }
                }
                console.log(personAns);

                fetch("http://localhost:8080/add_User", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(personInfo)
                })
                    .then(res => {
                        return res.json();
                    })
                    .then(data => {
                        console.log(data);
                    })
                    .catch(error => {
                        console.log(error);
                    })

                fetch("http://localhost:8080/add_Info", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify(personAns)
                })
                    .then(res => {
                        return res.json();
                    })
                    .then(data => {
                        console.log(data);
                    })
                    .catch(error => {
                        console.log(error);
                    })

                this.$router.push('/')
            } else {
                alert("還可以反悔")
            }
        }
    },
    mounted() {


    },
    created() {
        this.addAns();
    },
    updated() {
        // console.log(this.userName);
        // console.log(this.userPhone);
        // console.log(this.userEmail);
        // console.log(this.userAns0);
        // console.log(this.userAns1);
        // console.log(this.userAns2);
        // console.log(this.checkInput)
    }
}
</script>

<template>
    <div class="info">
        <addAns v-if="isShow == false" v-model:surveyData="surveyData" v-model:surveyQuestion="surveyQuestion"
            v-model:surveySelected="surveySelected" v-model:surveySelectedInfo="surveySelectedInfo"
            v-model:userName="userName" v-model:userPhone="userPhone" v-model:userEmail="userEmail"
            v-model:userAge="userAge" v-model:userAns1="userAns1" v-model:userAns2="userAns2" v-model:userAns3="userAns3"
            v-model:userAns4="userAns4" v-model:userAns0="userAns0" @nextPage="comfrim" />

        <showAns v-if="isShow == true" v-bind:surveyData="surveyData" v-bind:surveyQuestion="surveyQuestion"
            v-bind:surveySelected="surveySelected" v-bind:surveySelectedInfo="surveySelectedInfo" v-bind:userName="userName"
            v-bind:userPhone="userPhone" v-bind:userEmail="userEmail" v-bind:userAge="userAge" v-bind:userAns1="userAns1"
            v-bind:userAns2="userAns2" v-bind:userAns3="userAns3" v-bind:userAns4="userAns4" v-bind:userAns0="userAns0"
            @backPage="cancel" @sentInfo="submitInfo" />
    </div>
</template>


<style lang="scss" scoped>
.info {
    width: 100%;
    height: 101vh;
}
</style>