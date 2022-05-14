<template>
    <div class="info">
        <img v-if="item.poster_path !== null" :src="`https://image.tmdb.org/t/p/w342${item.poster_path}`" :alt="item.title ? item.title : item.name" class="cover">
        <img v-else src="@/assets/img/poster.jpg" alt="" class="cover">

        <ul class="card_description">
            <!-- TITOLO -->
            <li class="title">
                <span>{{title}}</span> 
            </li>
            <!-- BANDIERA -->
            <li class="language" v-if="flag.includes(item.original_language)">
                <strong>Lingua:</strong>
                <img class="flag" :src="require(`@/assets/img/${item.original_language}.png`)" :alt="item.original_language">
            </li>
            <li v-else class="language">
                <strong>Lingua:</strong>
                <img class="flag" src="@/assets/img/missing.png" :alt="item.original_language">
            </li>
            <!-- VOTO -->
            <li class="voto">
                <strong>Voto:</strong>
                <span v-for="index in 5" :key="index">
                    <font-awesome-icon :icon="['fas', 'star']" v-if="index <= Math.round(item.vote_average / 2)"/>
                    <font-awesome-icon :icon="['far', 'star']" v-else/>
                </span>
            </li>
            <!-- RIASSUNTO -->
            <li class="overview">
                <strong>Overview: </strong>
                <span>{{item.overview}}</span>
            </li>
        </ul>
    </div>
</template>

<script>

export default {
    name: "CardComponent",
    props: ['item'],
    data(){
        return {
            movieCast: [],
            seriesCast: [],
            flag: ['it', 'en', 'es', 'fr'],
        }
    },
    computed: {
        title(){
            return this.item.title ? this.item.title : this.item.name
        },
    },
}
</script>

<style lang="scss" scoped>

        .info {
            width: calc((100% / 6) - 1em);
            position: relative;
            margin: 2em 0.3em;
            overflow: hidden;
            border-radius: 5px;

            &:hover {
                transform: scale(1.4);
                z-index: 1;
                transition: ease-in-out 0.3s;

                .cover {
                    transform: scale(0.72);
                }
    
                .card_description {
                    opacity: 1;
                }
            }

            .cover {
                width: 100%;
                overflow: hidden;
                border-radius: 5px;
            }

            .card_description {
                list-style-type: none;
                position: absolute;
                top: 0;
                left: 0;
                background-color: rgba(20, 20, 20, 0.8);
                opacity: 0;
                height: 100%;
                border-radius: 5px;
                box-shadow: 0px 0px 10px 5px #000000;
                display: flex;
                flex-flow: column nowrap;
                overflow-y: auto;
                text-align: center;

                &::-webkit-scrollbar {
                    width: 0;
                }

                li {
                    color: white;
                    padding: 1em 1em;
                }

                .title {
                    font-size: 1.5em;
                }

                .overview {
                    margin: 1em 0;
                }

                .language {
                    display: flex;
                    justify-content: center;

                    .flag {
                        width: 1.5em;
                    }
                }
            }
        }

        @media screen and (max-width: 1440px){
            .info {
                width: calc((100% / 4) - 2em);
            }
        }
        @media screen and (max-width: 768px){
            .container .info {
                width: calc((100% / 2) - 2em);
            }
            .data .left ul,
            .data .rigth .bell,
            .data .rigth .user {
                display: none;
            }
        }
        @media screen and (max-width: 375px){
            .container .info {
                width: 100%;
                margin: 3em;
            }
        }
</style>