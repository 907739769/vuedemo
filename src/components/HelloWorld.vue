<template>
    <div class="hello">
        <ul v-for="(imageList,index) in imageLists" :key="index">
            <li v-for="(item,id) in imageList" :key="id">
                <a v-bind:href="item.url">
                    <img alt="baby" class="simg" mode="aspectFit" v-bind:src="item.thumbUrl"/>
                </a>
            </li>
        </ul>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'HelloWorld',
        data() {
            return {
                interval: 0,
                imageLists: [],
                pageNum: 1,
                pageSize: 24,
            }
        },
        mounted() {
            document.addEventListener("scroll", this.onscroll);
            this.interval = setInterval(this.onFull, 500);
        },
        beforeDestroy() {
            clearInterval(this.interval);
            document.removeEventListener("scroll", this.loadMore);
        },
        methods: {
            loadMore: function () {
                axios.get("http://127.0.0.1:8101/image/get", {
                    params: {
                        "pageNum": this.pageNum,
                        "pageSize": this.pageSize,
                    }
                }).then(res => {
                    this.imageLists.push(res.data.records);
                })
                this.pageNum++;
            },
            onscroll: function () {
                const doc = document;
                /** 滚动的高度 */
                let scrollTop = doc.documentElement.scrollTop === 0 ? doc.body.scrollTop : doc.documentElement.scrollTop;
                /** 滚动条高度 */
                let scrollHeight = doc.documentElement.scrollTop === 0 ? doc.body.scrollHeight : doc.documentElement.scrollHeight;
                if (scrollHeight - scrollTop  <= window.innerHeight) {
                    this.loadMore();
                }
            },
            onFull: function () {
                const doc = document;
                /** 页面高度 */
                let offsetHeight = doc.documentElement.scrollTop === 0 ? doc.body.offsetHeight : doc.documentElement.offsetHeight;
                if (offsetHeight <= window.innerHeight) {
                    this.loadMore();
                }
            },

        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 0;
    }

    a {
        color: #42b983;
        margin: 0.3125rem;
    }

    .simg {
        width: auto;
        /* height: 20rem */
    }
</style>
