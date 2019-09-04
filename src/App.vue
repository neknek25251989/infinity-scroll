<template>
    <div id="app">
        <b-container>
            <b-navbar sticky
                type="dark"
                variant="dark">
                <b-navbar-brand href="https://github.com/neknek25251989?tab=repositories">List of Lo Whai-Jer's repositories</b-navbar-brand>
            </b-navbar>
            <b-card-group>
                <b-card v-for="(repo, index) in listOfRepos"
                    :key="index"
                    style="min-width: 400px;">
                    <b-card-body>
                        <b-card-title>{{repo.name}}</b-card-title>
                        <b-card-text>{{repo.description}}</b-card-text>
                        <b-button :href="repo.html_url"
                            variant="primary">Link</b-button>
                    </b-card-body>
                </b-card>
            </b-card-group>
        </b-container>
    </div>
</template>

<script>
    import { BNavbar, BNavbarBrand, BContainer, BCardGroup, BCard, BCardBody, BCardTitle, BCardText, BButton } from 'bootstrap-vue'
    export default {
        name: 'app',
        data() {
            return {
                listOfRepos: []
            }
        },
        components: {
            BNavbar,
            BNavbarBrand,
            BContainer,
            BCardGroup,
            BCard,
            BCardBody,
            BCardTitle,
            BCardText,
            BButton
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