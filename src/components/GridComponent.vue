<template>
    <section class="category">
        <loader-component v-if="loading"/>
        <div v-else>
            <h2>{{categoria}}</h2>
            <div class="container">
                <card-component :item="item" v-for="(item, index) in list" :key="index"/>
            </div>
        </div>
    </section>
</template>

<script>
import LoaderComponent from "./LoaderComponent.vue"
import CardComponent from "./CardComponent.vue"

export default {
    name: "GridComponent",
    components: { 
      LoaderComponent,
      CardComponent 
    },
    props:{
        list: Array,
        categoria: String,
        loading: Boolean,
        research: String,
    },
    
}
</script>

<style lang="scss" scoped>
    .category {
        margin: 10em 0;

        h2 {
            padding: 0 7vw;
            color: white;
        }

        .container {
            width: 90vw;
            margin: 0 auto;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }
    }

    @media screen and (max-width: 375px){
        .category h2 {
            padding: 0;
            text-align: center;
        }
    }

</style>