<template>
    <div id="app">
        <h1>list of repositories</h1>
        <ul>
            <li v-for="(repo, index) in listOfRepos"
                :key="index">
                <h2>{{repo.name}}</h2>
                <h4>{{repo.description}}</h4>
                <a :href="repo.html_url">Link</a>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                listOfRepos: []
            }
        },

        mounted() {
            this.fetch();
        },

        created() {
            window.addEventListener('scroll', this.getScrollButtom)
        },
        destroyed() {
            window.removeEventListener('scroll', this.getScrollButtom)
        },
        computed: {

        },
        methods: {
            fetch: function() {
                var apiUrl = 'https://api.github.com/users/neknek25251989/repos';
                var xhr = new XMLHttpRequest();
                var self = this;
                xhr.open('GET', apiUrl);
                xhr.onload = function() {
                    var response = JSON.parse(xhr.responseText);
                    self.listOfRepos = self.listOfRepos.concat(response);
                }
                xhr.send();
            },
            getScrollButtom: function(e) {
                let offsetHeight = Math.max(document.body.scrollHeight, document.body.offsetHeight); // 内容高度
                let clientHeight = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight || 0; //视窗高度
                let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0; //滚动条滚动高度
                console.log(offsetHeight - clientHeight - scrollTop);
                let trigger = offsetHeight - clientHeight - scrollTop;

                if (trigger <= 0) {
                    this.fetch();
                }

            }
        },
    }
</script>

<style lang="scss">

</style>