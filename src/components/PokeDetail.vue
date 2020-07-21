<template>
  <div class="detail">
      <transition
        enter-active-class="animate__animated animate__bounceIn"
      >
        <div class="detail-view" v-if="show">
            <div class="detail-view__img mb-2" v-if="pokemon">
                <img :src="imgUrl + pokemon.id + '.png'" class="card-img-top" alt="">
            </div>
            <div class="detail-view__data text-center" v-if="pokemon">
                <span class="text-center">#{{ pokemon.id }}</span>
                <h3 class="mb-5">{{ pokemon.name }}</h3>
                <div class="property mb-3">
                    <div>Base experience:</div> 
                    <div class="font-weight-bold">{{ pokemon.base_experience }} XP</div>
                </div>
                <div class="property mb-3">
                    <div>Height:</div> 
                    <div class="font-weight-bold">{{ pokemon.height * 10 }} cm</div>
                </div>
                <div class="property mb-3">
                    <div>Weight:</div> 
                    <div class="font-weight-bold">{{ pokemon.weight / 10 }} kg</div>
                </div>
                <div class="types text-left">
                    <span class="type">Types:</span>
                    <ul class="types-list mt-2">
                        <li 
                            :key="value + index"
                            v-for="(value, index) in pokemon.types"
                        >
                            {{ value.type.name }}
                        </li>
                    </ul>
                </div>
                <div class="types text-left">
                    <span class="type">Abilities:</span>
                    <ul class="types-list abilities mt-2">
                        <li 
                            :key="value + index"
                            v-for="(value, index) in pokemon.abilities"
                        >
                            {{ value.ability.name }}
                        </li>
                    </ul>
                </div>
            </div>
            <div v-if="!pokemon">
                <h2>Pokemon not found :(</h2>
            </div>
            <button class="btn btn-dark mb-3" @click="closeDetail">Close</button>
        </div>
      </transition>
  </div>
</template>

<script>
export default {
    props: [
        'pokeUrl',
        'imgUrl',
    ],
    data() {
        return {
            pokemon: {},
            show: false,
        }
    },
    methods: {
        fetchData() {
            this.$http
                .get(this.pokeUrl)
                 .then((resp) => {
                    if(resp.status === 200)
                    this.pokemon = resp.data;
                    this.show = true;
                })
                .catch((error) => {
                    // Error
                    if (error.response.status == 404) {
                        this.pokemon = false
                        this.show = true
                    }
                })
        },
        closeDetail() {
            this.$emit('closeDetail')
        }
    },
    created() {
        this.fetchData()
    }
}
</script>

<style lang="scss">

    h2 {
        background: #ffffff;
        padding: 20px;
    }

    .detail {
        display: flex;
        justify-content: center;
        align-items: flex-start;
        position: fixed;
        top: 0;
        left: 0;
        padding: 90px 10px 10px;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.7);

        &-view {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            position: relative;
            width: 100%;
            max-width: 510px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 15px 30px rgba(0,0,0,.2),
                        0 10px 10px rgba(0,0,0,.2);


            &__img {
                width: 80px;
                height: 80px;

                padding: 10px;
                background: lightcoral;
                border-radius: 50%;
                position: relative;
                margin-top: -25px;
            }

            &__data {
                width: 100%;
                padding: 0 25px;

                h3 {
                    text-transform: capitalize;
                    font-size: 1.5em;
                }
            }
        }
    }

    .property {
        display: flex;
        justify-content: space-between;
        border-bottom: 1px dotted lightgray;
        font-size: 1em;
        width: 100%;
    }

    .type {
        display: flex;
        justify-content: space-between;
        border-bottom: 1px dotted lightgray;
        font-size: 1em;
        width: 100%;
    }

    .types-list {
        display: flex;
        justify-content: flex-start;
        padding: 0;
    }

    .types-list li {
        text-transform: capitalize;
        list-style-type: none;
        background: lightcoral;
        font-size: 0.8em;
        padding: 5px 20px;
        border-radius: 20px;
        display: inline-block;
        color: #ffffff;
        margin: 0 5px;
    }

    .abilities li {
        background: lightslategray;
    }

    .fade-enter {
        opacity: 0;
    }

    .fade-enter-active {
        transition: opacity 1s;
    }

    .fade-leave {
        /*opacity: 1;*/
    }

    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

</style>