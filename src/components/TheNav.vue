<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top" id="mainNav">
        <div class="container">
            <router-link class="navbar-brand" to="/">Projecty Web</router-link>
            <button aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation"
                    class="navbar-toggler"
                    data-target="#navbarResponsive" data-toggle="collapse" type="button">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarResponsive">
                <ul class="navbar-nav ml-auto">
                    <template v-if="!isAuthenticatedUser()">
                        <li class="nav-item" v-if="this.$route.path !== '/login'">
                            <router-link class="nav-link" to="/login">
                                Sign in
                            </router-link>
                        </li>
                        <li class="nav-item" v-if="this.$route.path !== '/register'">
                            <router-link class="nav-link" to="/register">
                                Sign up
                            </router-link>
                        </li>
                    </template>
                    <template v-else>
                        <li class="nav-item">
                            <router-link class="nav-link" to="/message/receivedMessages">
                                <template v-if="unreadMessages>0">
                                    <span class="text-warning">({{unreadMessages}})</span>
                                </template>
                                Messages
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link class="nav-link" to="/myProjects">
                                Projects
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link class="nav-link" to="/team/myTeams">
                                Teams
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <router-link class="nav-link" to="/settings">
                                <span>{{username}}</span>
                            </router-link>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link js-scroll-trigger" style="cursor: pointer" v-on:click="logout">Log
                                out</a>
                        </li>
                    </template>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    import {userService} from "@/services";

    export default {
        name: "TheNav",
        data() {
            return {
                username: '',
                unreadMessages: 0
            }
        },
        mounted() {
            if (userService.isAuthenticatedUser()) {
                const user = JSON.parse(localStorage.getItem('user'));
                if (user) {
                    this.username = user.username;
                }
                userService.makeRequestToAPI("/message/getUnreadMessageCount")
                    .then(unreadMessages => {
                        this.unreadMessages = unreadMessages;
                    });
            }
        },
        methods: {
            isAuthenticatedUser() {
                return userService.isAuthenticatedUser();
            },
            logout() {
                userService.logout();
                location.reload();
            }
        }
    }
</script>

<style scoped>

</style>
