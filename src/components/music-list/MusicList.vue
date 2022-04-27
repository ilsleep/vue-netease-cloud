<template>
    <h2>
        推荐歌单
        <button>查看更多></button>
    </h2>
    <div class="swiper-container swiper-music-list">
        <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="item in musicList">
                <router-link :to="{path:'/list-view',query:{id:item.id}}">
                    <!--  通过路由向下一个组件传参-->
                    <img :src="item.picUrl" alt="">
                    <span>
                        {{item.playCount>100000000?(item.playCount/100000000).toFixed(2)+'亿':
                        (item.playCount/10000).toFixed(2)+'万'}}
                    </span>
                    <p>{{item.name}}</p>
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
    import 'swiper/css/swiper.min.css'
    import Swiper from 'swiper'
    import {getMusicList} from "@/api";

    export default {
        name: "MusicList",
        data() {
            return {
                musicList: []
            }
        },
        created() {
            this.getMusicListData()//调用methods里的异步方法
            /*
            data里默认为空数据
            created时候，向后台要到数据，更新data
            更新完data之后，用新数据渲染页面（vue实例做的）
            监测到数据被修改后，updated里，操作dom
             */
        },
        updated() {
            const mySwiper = new Swiper('.swiper-music-list', {
                slidesPerView: 3,
                spaceBetween: 20,
            })
            this.getMusicListData()//调用methods里的异步方法
        },
        methods: {
            async getMusicListData() {
                const res = await getMusicList()//使用封装好的api方法去后台获取歌单列表的数据
                this.musicList = res.data.result
                console.log(this.musicList)
            }
        }
    }
</script>

<style scoped lang="less">
    button {
        float: right;
        width: 80px;
        height: 30px;
        background: #fff;
        border-radius: 15px;
        border: 1px solid #ebebeb;
    }

    .swiper-slide {
        width: 30vw;
        /*height: 35vw;*/
        /*text-align: center;*/
        font-size: 12px;

        a {
            text-decoration: none;
            color: black;
            font-size: 10px;
        }

        img {
            width: 100%;
        }

        span {
            position: absolute;
            top: 0;
            right: 10px;
            color: #fff;
        }

        p {
            margin: 0;
        }
    }
</style>