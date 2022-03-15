<template>
    <div class="dashboard" style="overflow:auto">
        <v-row align="center" class="top-bar" justify="center">
            <!-- <div class="top-bar"> -->
            <v-col cols="12" md="12" class="blue center rounded-bl-xl rounded-tr-xl">
                <v-card-text class="white--text">
                    <h2 class="text-center">Welcome To Parkinglot</h2>
                    <v-btn outlined x-small color="red">
                        <v-icon title="log out" @click.native="logoutUser">logout</v-icon>
                    </v-btn>
                </v-card-text>

            </v-col>
            <Content class="mainContent" />
        </v-row>

    </div>
</template>

<script>
    import Content from "./content.vue";
    export default {
        data: () => {
            return {
                mobileView: true,
                showNav: false
            };
        },
        methods: {
            handleView() {
                this.mobileView = window.innerWidth <= 990;
            },
            logoutUser() {
                this.$router.push({
                    name: 'Home'
                });
            }
        },
        components: {
            Content
        },
        created() {
            this.handleView();
            window.addEventListener('resize', this.handleView);
        }
    };
</script>

<style lang="scss">
    @import url("https://use.fontawesome.com/releases/v5.9.0/css/all.css");

    * {
        font-size: 1rem;
    }

    body {
        width: 100%;
        height: 100vh;
        margin: 0;
        padding: 0;
        font-family: "Segoe UI", Tahoma;
        background-color: #7ca971;
    }

    .dashboard {
        position: relative;
        width: calc(100% - 20px);
        height: calc(100vh - 20px);
        padding: 10px;
        color: #333;
        overflow: hidden;
    }

    .top-bar {
        display: flex;
        width: 100%;
    }

    #navigation-icon {
        padding: 10px 10px 20px;
        margin-right: 10px;
        cursor: pointer;

        i {
            font-size: 2rem;
        }
    }

    .mainContent {
        display: flex;
        width: 100%;
        height: 100%;
        // overflow: auto;

    }

    .content {
        position: absolute;
        top: 10px;
        width: calc(100% - 60px);
        height: calc(100vh - 60px);
        overflow: auto;
        padding: 20px;
        background-color: #fff;
        border-radius: 30px;
        box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
        transition: 1s transform cubic-bezier(0, .12, .14, 1);
    }

    .open {
        transform: translateX(300px);
    }
</style>