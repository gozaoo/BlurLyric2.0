<template>
<h1>搜索“<a>{{page.q}}</a>”的结果</h1>

    <div class="row-two">
        <div>
            <h2 v-if="page.track != []">歌曲列表<a v-if="page.track != []" style="font-size:0.7em;color: rgba(0,0,0,.5)">{{'  '+page.track.length}}首</a></h2>
            <div v-if="page.track != []" class="SearchTrack" style="user-select:none">
                        <div v-bind:class="'tracks ' + (item.id == this.$parent.$parent.$parent.id )"  v-for="(item,i) in page.track" :key="item.id">
            <!--显示样式-->
            <div>
                <div @click="playTheOnce(i)" class="trackIMG">
            
                    <img
                    v-lazy="item.al.picUrl +'?param=192y192'" alt="" srcset="">
                    <img
                    v-lazy="item.al.picUrl +'?param=192y192'" alt="" srcset="">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="bi bi-play" viewBox="0 0 16 16">
                        <path d="M10.804 8 5 4.633v6.734L10.804 8zm.792-.696a.802.802 0 0 1 0 1.392l-6.363 3.692C4.713 12.69 4 12.345 4 11.692V4.308c0-.653.713-.998 1.233-.696l6.363 3.692z"/>
                    </svg>
                </div>
                <div class="trackTitle">
                    <h1>{{item.name}} <a v-for="(alia,i) in item.alia" :key="i" style="color: rgba(44,62,80,0.5)">
                            {{alia}} </a></h1>
                    <h2><a v-for="(name) in item.ar" :key="name.id"> {{name.name}}</a></h2>

                </div>
            </div>

            <div class="linkbox bigger">
                <a @click="musicListMore(item)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-three-dots" viewBox="0 0 16 16">
                        <path d="M3 9.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3zm5 0a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3zm5 0a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3z"/>
                    </svg>
                </a>
                <!--a v-if="(this.$parent.$parent.$parent.data.musicListInfor.myLove.aRtrackIds.indexOf(item.id) != -1)"
                    style="color:red;user-select:none" @click="this.$parent.$parent.$parent.loveMusic(item.id)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-heart-fill" viewBox="0 0 16 16">
                        <path fill-rule="evenodd"
                            d="M8 1.314C12.438-3.248 23.534 4.735 8 15-7.534 4.736 3.562-3.248 8 1.314z" /></svg>
                </!--a>
                <a-- v-if="(this.$parent.$parent.$parent.data.musicListInfor.myLove.aRtrackIds.indexOf(item.id) == -1)"
                    style="user-select:none" @click="this.$parent.$parent.$parent.loveMusic(item.id)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-heart" viewBox="0 0 16 16">
                        <path
                            d="m8 2.748-.717-.737C5.6.281 2.514.878 1.4 3.053c-.523 1.023-.641 2.5.314 4.385.92 1.815 2.834 3.989 6.286 6.357 3.452-2.368 5.365-4.542 6.286-6.357.955-1.886.838-3.362.314-4.385C13.486.878 10.4.28 8.717 2.01L8 2.748zM8 15C-7.333 4.868 3.279-3.04 7.824 1.143c.06.055.119.112.176.171a3.12 3.12 0 0 1 .176-.17C12.72-3.042 23.333 4.867 8 15z" />
                        </svg>
                </a-->
            </div>
        </div>
            </div>
        </div>

        <div>
            <h2 v-if="page.ar.result">歌手<a style="font-size:0.7em;color: rgba(0,0,0,.5)" v-if="page.ar.result"> {{'  '+page.ar.result.artistCount}}</a></h2>
            <div class="ARtrack" v-if="page.ar.result">
                <div v-for="(item) in page.ar.result.artists" :key="item.id">
                    <div class="ARImg" v-lazy="item.picUrl + '?param=500y500)'" v-bind:alt="item.name" ></div>
                    <div class="ARTrTitle">
                        {{item.name}}
                    </div>
                </div>
                <!--div @click="this.$router.push({name:'detail',query:{id:item.id }})" v-for="(item) in page.PLtrack" :key="item.id">
                    <img v-bind:src="item.coverImgUrl" v-bind:alt="item.name" >
                    <div class="PlTrTitle">
                        <h1>{{item.name}}</h1>
                        <h2>by {{item.creator.nickname}}</h2>
                    </div>
                </div-->
            </div>
            <h2 v-if="page.PLtrack[0]">歌单<a v-if="page.PLtrack[0]" style="font-size:0.7em;color: rgba(0,0,0,.5)">{{'  '+page.playlist.result.playlistCount}}个</a></h2>
            <div v-if="page.PLtrack[0]" class="PLtrack">
                <div @click="this.$router.push({name:'detail',query:{id:item.id }})" v-for="(item) in page.PLtrack" :key="item.id">
                    <img v-lazy="item.coverImgUrl + '?param=500y500)'" v-bind:alt="item.name" >
                    <div class="PlTrTitle">
                        <h1>{{item.name}}</h1>
                        <h2>by {{item.creator.nickname}}</h2>
                    </div>
                </div>
            </div>
    
        </div>
    </div>
</template>

<script>
import reTools from '../network/getData'
export default {
    name: 'searchList',
    data() {
        return {
            page:{
                q: this.$route.query.q ,
                pic: '',
                title: '稍等',
                content: '',
                creater: '',
                trackIds:'',
                aRtrackIds:[],
                track:[],
                playlist: {},
                PLtrack:[],
                ar:{}
            }
        }
    },
    async created() {
        this.requestData()
    },
    methods: {
        setTracks(num){
            this.$parent.$parent.$parent.changeTrack({tracks: this.page.track,num : num})
        },
        playThisPage(){
            this.setTracks(0)
        },
        playTheOnce(i){
            this.setTracks(i)
        },
        requestData(){
            reTools.getData('/search',{keywords:this.$route.query.q,type:1,limit:50}).then(
            r=>{
                console.log(r,this.page.q);
                this.page.trackIds = r.result.songs
                let trackIDList = ''

                for (const num in this.page.trackIds) {
                    trackIDList += this.page.trackIds[num].id
                    this.page.aRtrackIds.push(this.page.trackIds[num].id)
                    if (num < this.page.trackIds.length - 1) {
                    trackIDList += ','
                    
                    } 
                }
                console.log(trackIDList);
                reTools.getData('/song/detail',{ids:trackIDList}).then(res=>{
                    this.page.track = res.songs
                    console.log(res);
                })
            })
            reTools.getData('/search',{type:1000,keywords:this.page.q,limit:100}).then(r=>{
                this.page.playlist = r
                this.page.PLtrack = r.result.playlists
            })
            reTools.getData('/search',{type:100,keywords:this.page.q}).then(r=>{
                this.page.ar = r
                console.log(r);
            })
        }
    },
    watch:{
        $route:{
            handler: function (newVal){
                this.page.q = newVal.query.q
                this.requestData()
                
            },
            deep: true
        }
    }
}
</script>

<style>


    .SearchTrack{
        display: flex;
        flex-direction: column;
        overflow-y: auto;
    }
    .SearchTrack> .tracks{
        max-width: calc(var(--paddingIndex-MaxWidth) / 2);
    }
    div.SearchTracks.true{
        background-image: linear-gradient(to right,#00000008,#0000ff08);
    }
    .SearchTracks> div:nth-child(1){
        display: flex;
        width: 100%;
        gap: 8px;
        overflow: hidden;
    }
    .SearchTracks> div:nth-child(1)>.trackTitle{
    }
    .SearchTracks> div:nth-child(1)>.trackTitle>*{
        white-space: nowrap;
        overflow: hidden;
        display: -webkit-box;
        text-overflow: ellipsis;
    }
    .SearchTracks> div:nth-child(1)>.trackTitle>h1>a{
        font-size: 0.8em;
    }
    .SearchTracks> div:nth-child(1)>.track-img{
    user-select: none;

    height: 42px;
    width:42px;
    min-height:42px;
    min-width: 42px;
    border-radius: 9px;
    background-position: 50% 50%;
    background-size: cover;
    background-image: url("https://p1.music.126.net/P99uc1Hqzkj2QE1UMehuHQ==/109951167197634217.jpg?param=48y48");
    }
        .SearchTracks> div:nth-child(1)>.trackTitle>h1{
        font-size: 15px;
        margin: 5px 0 0 0;
       
    }
    .SearchTracks> div:nth-child(1)>.trackTitle>h2{
        font-size: 10px;
        margin: 2px 0 0 0;
    }
    .SearchTracks> div:nth-child(1)>.trackTitle>h2>a{
        margin: 0 1em 0 0;
    }
    @media (max-width: 500px) {
        .dlTopLab-TitleLab{

        }
        .dlTopLab-TitleLab > h2 {
            font-size: 1.15em;
            display: -webkit-box;
            --webkit-box-orient: vertical;
            --webkit-line-clamp: 2;
            text-overflow: clip;
            white-space: normal;
        }
        .dlTopLab-TitleLab > h3{
            font-size: 0.9em;
        }
        div.dlTopLab > .dlTopLab-TitleLab > .linkbox.bigger > a{
            padding: 5px 10px;
        }
        .SearchTrack{
            max-height: 50vh;
            border-radius: 8px;
        }
        .SearchTracks{
            position: relative;
            background-image: linear-gradient(to right,#d1d1d1,#e5e5e5);
            border-radius: 9px;
            display: flex;
            max-width: calc(var(--paddingIndex-MaxWidth) - calc(var(--thispadding) * 2));
        }

    }
    .dlTopLab-TitleLab>.linkbox.bigger>a{
        margin-top: 3px;
        background-color: rgba(255, 255, 255, 0.8);
        
    }

    .row-two{
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 30px;
        width: var(--paddingIndex-MaxWidth);
        max-width: var(--paddingIndex-MaxWidth);
    }
    @media (max-width:650px) {
            .SearchTrack>.tracks{
          max-width: calc(var(--paddingIndex-MaxWidth) - 20px) ;            
        }
    .row-two>div>.track>div{
        max-width: calc(var(--paddingIndex-MaxWidth));
    }
    .row-two{
        display: grid;
        grid-template-columns: repeat(1, 1fr);
        gap: 30px;
        width: var(--paddingIndex-MaxWidth);
        max-width: var(--paddingIndex-MaxWidth);
    }
    div.viewBox > div > div:nth-child(1)>.track{
        height: 100vh;
        overflow-y: auto;
        border-radius: 11px;
    }

    }
    .PLtrack{
        display: grid;
        --repeat: 2;
        grid-template-columns: repeat(var(--repeat), 1fr);
        overflow: hidden;
        --gapver: 20px;
        gap: var(--gapver);
        flex-direction: row;
        font-size: 16px;
        width: calc(var(--paddingIndex-MaxWidth) / 2);
    }

    .PLtrack>div{

        width: calc(calc(var(--paddingIndex-MaxWidth) / calc( 2 * var(--repeat))) - calc(calc(var(--repeat) - 1) * var(--gapver)));
    }

    .PLtrack>div>img{
        width: 100%;
        border-radius: 1.5vw;
    }
    .PlTrTitle{
        overflow: hidden;
        text-overflow: ellipsis;
        -webkit-box-orient: vertical;
        color: rgba(50,50,50,1);
    }
    .PlTrTitle>*{
        -webkit-line-clamp: 2;
        overflow: hidden;
        text-overflow: ellipsis;
        -webkit-box-orient: vertical;
        display: -webkit-box;

    }
    .PlTrTitle>h1 {
        -webkit-line-clamp: 2;
        font-size: 1em;
        color: rgba(50,50,50,1);
        margin: 0.4em 0 0 0;
    }
    .PlTrTitle>h2{
        -webkit-line-clamp: 1;
        font-size: 0.8em;
        color: rgba(100,100,100,1);
        margin: 1px 0 0 0;

    }
    @media (max-width: 500px) {
    .PLtrack{
        width: var(--paddingIndex-MaxWidth);
    }
    .PLtrack>div{
    width: calc(calc(var(--paddingIndex-MaxWidth) /  var(--repeat)) - calc(calc(var(--repeat) - 1) * var(--gapver)));
    }
    .PLtrack>div>img{
        width: 100%;
        border-radius: 2.5vw;
    }
        }
    @media (min-width:1300px) {
    .PLtrack{
        --repeat: 3;

    }}
    @media (min-width:1650px) {
    .PLtrack{
        --repeat: 3;
    }}

    .ARtrack{
        display: flex;
        gap: 30px;
        overflow-x: auto;
        width: calc(var(--paddingIndex-MaxWidth) / 2);
        padding:  0 0 10px 0px;
        color: rgba(50,50,50,1);
    }
    .ARImg{
        height: 15vh;
        width: 15vh;
        border-radius: 50%;
        background-position: center center;
        background-size: cover;
        background-repeat: no-repeat;

    }
    .ARtrack::-webkit-scrollbar {
        height: 8px;
    }
    .ARTrTitle{
        margin-top: .8em;
        font-size: 1em;
        text-align: center;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
    }
    @media (max-width: 500px) {
    .ARtrack{
        width: var(--paddingIndex-MaxWidth);
    }}
</style>