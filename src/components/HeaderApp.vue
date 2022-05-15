<template>
    <div>
        <header class="data">
            <div class="left">
                <img src="@/assets/img/logo.svg" alt="logo" class="logo">
                <img src="@/assets/img/logo-small.svg" alt="logo-small" class="logo-small">
                <nav>
                    <ul>
                        <li><a href="#">Home</a></li>
                        <li><a href="#">Serie TV</a></li>
                        <li><a href="#">Film</a></li>
                        <li><a href="#">Nuovi e popolari</a></li>
                        <li><a href="#">La mia lista</a></li>
                    </ul>
                </nav>
            </div>
            <div class="rigth">
                <input type="text" v-model="inputText" @keyup.enter="searchModel">
                <button @keyup.enter="searchModel"><font-awesome-icon :icon="['fas', 'magnifying-glass']"/></button>
                <font-awesome-icon :icon="['fas', 'bell']" class="bell"/>
                <img src="@/assets/img/user.jpg" alt="user" class="user">
            </div>
        </header>
    </div>
</template>

<script>
export default {
    name: "HeaderApp",
    data() {
        return {
            inputText: '',
        }
    },
    methods: {
        searchModel(){
            this.$emit('search', this.inputText)
            this.inputText = ''
        }
    }
}
</script>

<style lang="scss" scoped>
    .data {
        display: flex;
        position: fixed;
        top: 0;
        width: 100%;
        z-index: 1000;
        justify-content: space-between;
        align-items: center;
        background-color: #141414;

        .left,
        .rigth {
            display: flex;
            align-items: center;
            color: white;

            .logo {
                width: 10em;
                cursor: pointer;
            }

            .logo-small {
                width: 2em;
                padding: 0.2em;
                margin-left: 1em;
                display: none;
                cursor: pointer;
            }

            ul {
                display: flex;
                list-style-type: none;

                li {
                    padding: 0.3em;
                    a {
                        text-decoration: none;
                        color: white;
                        font-size: 0.9em;
                    }
                }
            }

            .user {
                width: 2em;
                border-radius: 5px;
                margin: 0 1em;
                cursor: pointer;
            }

            input {
                padding: 10px;
                margin: 0 5px;
                border: none;
                border-radius: 10px;
            }

            button {
                padding: 10px;
                background-color: #141414;
                border: none;
                margin-right: 1em;
                cursor: pointer;
                color: white;
                cursor: pointer;
            }
        }
    }
    @media screen and (max-width: 768px){
        .data .left ul,
        .data .rigth .bell,
        .data .rigth .user {
            display: none;
        }
    }
    @media screen and (max-width: 375px){
        .data {
            padding: 1em 0;
        }
        .data .left .logo {
            display: none;
        }
        .data .left .logo-small {
            display: block;
        }
    }
</style>
