<template>
    <div class="news">
        <div class="newsList">
            <div class="newsList__item newItem" v-for="(newItem, index) in news" :key="index">
                <img v-if="newItem.image" :src="newItem.image" alt="" class="newItem__img">
                <div class="newItem__info" v-bind:class="newItem.image ? 'hoverAnimation' : ''">
                    <div class="newItem__date dateItem">
                        <span class="dateItem__day">{{newItem.fullDate.day}}</span>
                        <span>
                            <span class="dateItem__mounth">{{newItem.fullDate.mounth}}</span>
                            <span class="dateItem__year">{{newItem.fullDate.year}}</span>
                        </span>
                    </div>
                    <h2 class="newItem__title">{{newItem.name}}</h2>
                    <p class="newItem__desc">{{newItem.previewText}}</p>
                    
                </div>
                <div class="newItem__tags">
                    <div class="newItem__tag">
                        {{newItem.type.value}}
                    </div>
                </div>
            </div>
        </div>
        <button class="newsBtn" v-if="this.totalPages != currentPage" @click="loadMore">Загрузить еще</button>
    </div>
</template>
<script>
export default {
    
    data(){
      return{
        news:[],
        mounths:[
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December",
        ],
        // newsPerPage: 9,
        totalPages: 1,
        currentPage: 1,
        // currentNews: []
      }
    },
    methods:{
        async getData(page = "") {
            const res = await fetch("http://flems.github.io/test/api/news/"+page, {
              method: "GET",
              mode:"cors",
            });
            const finalRes = await res.json();
            finalRes.items.forEach(element => {
              element.fullDate = {
                day: Math.floor(((element.date % 31536000) % 2628000) / 86400),
                mounth: this.mounths[Math.floor((element.date % 31536000) / 2628000)],
                year: Math.floor(element.date / 31536000) + 1970,
              };
              this.news.push(element)
              
            });
            
            this.totalPages = finalRes.nav.total
            console.log(finalRes);
            // this.paginageNews();
        },
        
        // paginageNews(){
        //     this.currentNews = this.news.items.slice(0, this.currentPage * this.newsPerPage)
        // },

        loadMore(){
            this.currentPage++;
            this.getData(`${this.currentPage}/`)
            // this.paginageNews();
        }
    },
    mounted(){
        this.getData();
    }
}
</script>
<style>
.news{
    padding-top: 64px;
    padding-bottom: 72px;
}
.newsList{
    padding: 3px;
    

    
    margin: 0 auto;
    max-width: 1710px;
    
    display: grid; 
    grid-template-columns: repeat(3,minmax(320px, 1fr));
    grid-template-rows: 1fr 1fr 1fr 1fr 1fr repeat(1fr, 20); 
    grid-auto-flow: row;
    gap: 48px; 
    align-items: start;
    justify-items: center;
    
}
.newItem{
    border-radius: 16px;
    border: 1px solid  #0F62FE;
    height: 100%;
    position: relative;
    margin-bottom: 16px;
    
}
.newItem__img{
    width: 100%;
    height: 250px;
    border-radius: 16px 16px 0px 0px;
    overflow: hidden;
}

.newItem__info{
    border-radius: 16px 16px 0px 0px;
    position: relative;
    bottom: 0;
    transition: bottom .5s;
    background: white;
    padding: 32px;
    padding-bottom: 100px;

    display: flex;
    flex-direction: column;
    gap: 16px;
}
.newItem__date{
    color:  #A1A7B5;
}
.dateItem{
    display: flex;
    align-items: center;
    gap: 4px;
}
.dateItem__day{
    font-size: 36px;
    font-weight: 400;

    
}
.dateItem__mounth, .dateItem__year{
    display: block;
    font-size: 15px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%; /* 16.5px */
    letter-spacing: -0.15px;
}
.dateItem__year{

}
.newItem__title{
    color:  #0C5BEF;
    font-size: 22px;
    font-weight: 400;
    line-height: 120%;
}
.newItem__desc{
    color: #222327;
    /* L/desktop */
    font-size: 20px;
    font-weight: 400;
    line-height: 130%; /* 26px */
    letter-spacing: -0.2px;
}

.newItem__tags{
    display: flex;

    position: absolute;
    padding: 0 32px;
    bottom: 32px;
}
.newItem__tag{
    padding: 4px 16px;

    border-radius: 360px;
    background: #F0F6FE;

    font-size: 14px;
    color: #00133A;
    line-height: 140%;
    

}
.newItem:hover .hoverAnimation{
    bottom: 254px;
}
.newsBtn{
    background: none;
    outline: none;

    cursor: pointer;

    display: block;
    margin: 0 auto;
    margin-top: 72px;
    padding: 16px 32px;

    border-radius: 8px;
    border: 1px solid #002DBF;

    color: #002DBF;
    text-align: center;
    font-size: 20px;
    font-weight: 600;
    line-height: 120%; /* 24px */
    letter-spacing: -0.2px;

    transition: all .4s;
}
.newsBtn:hover{
    background: #002DBF;
    color: white;
}
@media (max-width:1550px) {
    .newsList{

        grid-template-columns: repeat(2,minmax(320px, 1fr));
    
        
    }
}
@media (max-width:840px) {
    .newsList{

        grid-template-columns: repeat(1,minmax(320px, 1fr));
    
        
    }
}
</style>