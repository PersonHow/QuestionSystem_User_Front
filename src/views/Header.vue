<script>
export default{
    
}

</script>

<template>
    <!-- icon 需要的網址 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" integrity="sha512-iecdLmaskl7CVkqkXNQ/ZH/XLlvWZOJyj7Yy7tcenmpD1ypASozpmT/E0iPtmFIB46ZmdtAc9eNBvH0H/ZpiBw==" crossorigin="anonymous" referrerpolicy="no-referrer" />

<div class="top">
    <!-- 左方icon區 -->
    <div class="btnsArea">
        <!-- 目的在於背景顯白 -->
        <div class="team">
            <i class="fa-sharp fa-solid fa-circle-arrow-left"></i>
            <i class="fa-sharp fa-solid fa-circle-arrow-right"></i>
            <i class="fa-sharp fa-solid fa-xmark"></i>
            <i class="fa-sharp fa-solid fa-house"></i>
        </div>
    </div>
    <!-- 上方偽網址區 -->
    <div class="showTitle">
        <div class="fakeUrl">
            <span>這是人豪的問卷調查系統</span>
        </div>
    </div>
    <!-- 偽搜尋按鈕 -->
    <div class="search">
        <div class="fakeBtn">
            <i class="fa-sharp fa-solid fa-magnifying-glass"></i>
            <span></span>
        </div>
    </div>
</div>

</template>

<style lang="scss" scoped>
.top{
    width: 100%;
    height: 15vh;
    background-color: #777;
    display: flex;
    justify-content: center;

    .btnsArea{
        width:20%;
        height: 15vh;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 30pt;

        .team{
            width: 80%;
            height: 10vh;
            background-color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 20pt;
            i{
                margin: 0 2%;
                background-color: white;
            }
        }
        
    }

    .showTitle{
        width: 60%;
        height: 15vh;
        font-size: 24pt;
        display: flex;
        justify-content: center;
        align-items: center;
        .fakeUrl{
            width: 70%;
            height: 10vh;
            background-color: white;
            border-radius: 20pt;
            display: flex;
            justify-content: center;
            text-align: center;
            align-items: center;
        }
    }

    .search{
        width: 10%;
        height: 15vh;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 30pt;

        .fakeBtn{
            width: 80%;
            height: 10vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: white;
            border-radius: 20pt;
        }
    }
}
</style>