<template>

<div id="hidden-card">
        <div id="wrapper" class="wrapper">
            <div id="blur-background">
                <canvas id="blur-background-canvas" width="1200" height="1200"></canvas>
            </div>
            <div id="overlay"></div>
            <div id="shadow">
                <img width="100%" height="100%" src="../assets/images/shadow.png">
            </div>
            <div id="main-image">
                <canvas width="590" height="950" id="main-image-canvas"></canvas>
                <div id="content" class="text-shadow" v-if="user">
                    <div id="user">
                        <span id="user-name" v-if="user">{{user.username}}</span>
                        <svg id="arrow-down" width="25" height="25" viewBox="0 0 20 20" fill="#ffffff"
                            xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd"
                                d="M1.646 4.646a.5.5 0 01.708 0L8 10.293l5.646-5.647a.5.5 0 01.708.708l-6 6a.5.5 0 01-.708 0l-6-6a.5.5 0 010-.708z"
                                clip-rule="evenodd" fill="#a2a2a2" stroke-width="2" stroke="#a2a2a2"></path>
                            <path fill-rule="evenodd"
                                d="M1.646 4.646a.5.5 0 01.708 0L8 10.293l5.646-5.647a.5.5 0 01.708.708l-6 6a.5.5 0 01-.708 0l-6-6a.5.5 0 010-.708z"
                                clip-rule="evenodd" stroke="#ffffff" stroke-width="1"></path>
                        </svg>
                        <div id="hamburger">
                            <div class="item"></div>
                            <div class="item"></div>
                            <div class="item"></div>
                        </div>
                    </div>
                    <div id="avatar" class="text-center">
                            <img id="avatar-top" v-if="user" :src="user.profile_pic_url_hd" alt="avatar" crossorigin="anonymous">
                            <div id="icon-plus">+</div>
                            <div id="name" v-if="user"><b>{{user.full_name}}</b></div>
                    </div>
                    <div id="statistics">
                        <div class="text-center">
                            <strong id="posts" v-if="user">{{formatNumber(user.edge_owner_to_timeline_media.count)}}</strong>
                            <p class="stat-text trans-posts" id="posts-txt">Posts</p>
                        </div>
                        <div class="text-center">
                            <strong id="followers" v-if="user">{{formatNumber(user.edge_followed_by.count)}}</strong>
                            <p class="stat-text trans-followers" id="followers-txt">Followers</p>
                        </div>
                        <div class="text-center">
                            <strong id="following" v-if="user">{{formatNumber(user.edge_follow.count)}}</strong>
                            <p class="stat-text trans-following" id="following-txt">Following</p>
                        </div>
                        
                    </div>
                    
                    <div id="bottom">
                        <svg aria-label="Home" class="_8-yf5 " fill="#ffffff" height="35" viewBox="0 0 48 48"
                            width="35">
                            <path
                                d="M45.3 48H30c-.8 0-1.5-.7-1.5-1.5V34.2c0-2.6-2-4.6-4.6-4.6s-4.6 2-4.6 4.6v12.3c0 .8-.7 1.5-1.5 1.5H2.5c-.8 0-1.5-.7-1.5-1.5V23c0-.4.2-.8.4-1.1L22.9.4c.6-.6 1.5-.6 2.1 0l21.5 21.5c.4.4.6 1.1.3 1.6 0 .1-.1.1-.1.2v22.8c.1.8-.6 1.5-1.4 1.5zm-13.8-3h12.3V23.4L24 3.6l-20 20V45h12.3V34.2c0-4.3 3.3-7.6 7.6-7.6s7.6 3.3 7.6 7.6V45z">
                            </path>
                        </svg>
                        <svg aria-label="Search &amp; Explore" class="_8-yf5 " fill="#ffffff" height="35"
                            viewBox="0 0 48 48" width="35">
                            <path
                                d="M20 40C9 40 0 31 0 20S9 0 20 0s20 9 20 20-9 20-20 20zm0-37C10.6 3 3 10.6 3 20s7.6 17 17 17 17-7.6 17-17S29.4 3 20 3z">
                            </path>
                            <path
                                d="M46.6 48.1c-.4 0-.8-.1-1.1-.4L32 34.2c-.6-.6-.6-1.5 0-2.1s1.5-.6 2.1 0l13.5 13.5c.6.6.6 1.5 0 2.1-.2.3-.6.4-1 .4z">
                            </path>
                        </svg>
                        <svg aria-label="New Post" class="_8-yf5 " fill="#ffffff" height="35" viewBox="0 0 48 48"
                            width="35">
                            <path
                                d="M31.8 48H16.2c-6.6 0-9.6-1.6-12.1-4C1.6 41.4 0 38.4 0 31.8V16.2C0 9.6 1.6 6.6 4 4.1 6.6 1.6 9.6 0 16.2 0h15.6c6.6 0 9.6 1.6 12.1 4C46.4 6.6 48 9.6 48 16.2v15.6c0 6.6-1.6 9.6-4 12.1-2.6 2.5-5.6 4.1-12.2 4.1zM16.2 3C10 3 7.8 4.6 6.1 6.2 4.6 7.8 3 10 3 16.2v15.6c0 6.2 1.6 8.4 3.2 10.1 1.6 1.6 3.8 3.1 10 3.1h15.6c6.2 0 8.4-1.6 10.1-3.2 1.6-1.6 3.1-3.8 3.1-10V16.2c0-6.2-1.6-8.4-3.2-10.1C40.2 4.6 38 3 31.8 3H16.2z">
                            </path>
                            <path
                                d="M36.3 25.5H11.7c-.8 0-1.5-.7-1.5-1.5s.7-1.5 1.5-1.5h24.6c.8 0 1.5.7 1.5 1.5s-.7 1.5-1.5 1.5z">
                            </path>
                            <path
                                d="M24 37.8c-.8 0-1.5-.7-1.5-1.5V11.7c0-.8.7-1.5 1.5-1.5s1.5.7 1.5 1.5v24.6c0 .8-.7 1.5-1.5 1.5z">
                            </path>
                        </svg>
                        <svg aria-label="Activity" class="_8-yf5 " fill="#ffffff" height="35" viewBox="0 0 48 48"
                            width="35">
                            <path clip-rule="evenodd"
                                d="M34.3 3.5C27.2 3.5 24 8.8 24 8.8s-3.2-5.3-10.3-5.3C6.4 3.5.5 9.9.5 17.8s6.1 12.4 12.2 17.8c9.2 8.2 9.8 8.9 11.3 8.9s2.1-.7 11.3-8.9c6.2-5.5 12.2-10 12.2-17.8 0-7.9-5.9-14.3-13.2-14.3zm-1 29.8c-5.4 4.8-8.3 7.5-9.3 8.1-1-.7-4.6-3.9-9.3-8.1-5.5-4.9-11.2-9-11.2-15.6 0-6.2 4.6-11.3 10.2-11.3 4.1 0 6.3 2 7.9 4.2 3.6 5.1 1.2 5.1 4.8 0 1.6-2.2 3.8-4.2 7.9-4.2 5.6 0 10.2 5.1 10.2 11.3 0 6.7-5.7 10.8-11.2 15.6z"
                                fill-rule="evenodd"></path>
                        </svg>
                        <img id="avatar-bottom" width="30px" height="30px;"  v-if="user" :src="user.profile_pic_url" alt="avatar"
                            crossorigin="anonymous">
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    export default {
        name:"hidden-card",
        props: ['user'],
        methods: {
            formatNumber(e) {
              return  e < 1e3 ? e : e >= 1e3 && e < 1e6 ? +(e / 1e3).toFixed(1) + "K" : e >= 1e6 && e < 1e9 ? +(e / 1e6).toFixed(1) + "M" : e >= 1e9 && e < 1e12 ? +(e / 1e9).toFixed(1) + "B" : e >= 1e12 ? +(e / 1e12).toFixed(1) + "T" : void 0
            }
        }
    }
</script>

<style lang="less">
#hidden-card {
    position: relative;
    top: 0;
    left: 0;
    height: 0;
    margin: 0 auto;
    overflow: hidden;
}

 #wrapper {
    position: relative;
    width: 1200px;
    height: 1200px;
    overflow: hidden;
}

#blur-background {
    width: 1200px;
    height: 1200px;
    position: absolute;
    top: 0;
    left: 0;
}

#blur-background-canvas {
    position: absolute;
    top: 0;
    left: 0;
}
#shadow {
    width: 1200px;
    height: 1200px;
    position: absolute;
    top: 0;
    left: 0;
}

#overlay {
    position: absolute;
    top: 0;
    left: 0;
    background: rgba(0,0,0,.3);
    display: block;
    width: 1200px;
    height: 1200px;
}

#main-image{
    position: absolute;
    z-index: 9;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 50px;
    overflow: hidden;
    z-index: 9;
}

#main-image-canvas {
    width: 100%;
    height: 100%;
}

#content{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    font-size: 1.3rem;
    background: rgba(0,0,0,.1);
    
}

#user{
    display: flex;
    align-items: center;
    width: 100%;
    justify-content: center;
    margin-top: 2rem;
    height: 2rem;
}

#arrow-down{
    width: 20px;
    height: 20px;
    margin-left: 0.5rem;
    margin-top: 5px;
}

#hamburger {
    position: absolute;
    right: 4rem;
}

#hamburger .item {
    width: 1.5rem;
    height: 2px;
    content: '';
    display: block;
    background-color: white;
    margin-bottom: 5px;
}

#statistics{
    height: 100px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding-left: 180px;
    padding-right: 30px;
}

#avatar {
    position: absolute;
    top: 85px;
    left: 75px;
    // width: px;
}

#icon-plus {
    position: absolute;
    left: 40px;
    top: 2.5rem;
    border: 1px solid white;
    border-radius: 50%;
    width: 14px;
    height: 14px;
    font-size: 10px;
    line-height: 10px;
    text-align: center;
    font-weight: 600;
    background: rgb(50, 156, 255);
    
}

#avatar #avatar-top {
    border-radius: 50%;
    width: 3.5rem;
    height: 3.5rem;
    object-fit: cover;
    margin-bottom: 0.5rem;
}

#avatar #name {
    display: block;
    font-size: 20px;
    position: absolute;
    top: 60px;
    left: 50%;
    transform: translate(-50%,0);
    white-space: nowrap;
    max-width: 300px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: none;
}

#bottom {
    display: flex;
    justify-content: space-around;
    position: absolute;
    width: 100%;
    bottom: 2rem;
    width: 85%;
    left: 50%;
    transform: translateX(-50%);
}

#avatar-bottom {
    width: 2.5rem;
    height: 2.5rem;
    border-radius: 50%;
    border: 2px solid white;
    object-fit: cover;
}
</style>