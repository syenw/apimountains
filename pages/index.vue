<template>
    <div>
        <p v-if="$fetchState.pending">Fetching mountains...</p>
        <p v-else-if="$fetchState.error">An error occurred :(</p>
        <div v-else>
            <h1>Nuxt API Mountains (Pages)</h1>
            <b-button @click="$fetch" variant="primary">Refresh</b-button><br />
            <sup id="url-api">https://api.nuxtjs.dev/mountains</sup>
            <div class="row">
                <div class="col-md-4" v-for=" (value, index) in mountains" :key="index">
                    <b-card :title='value.title' :img-src='value.image' :img-alt="value.title" tag="article" style="padding: 5px;" class="mb-2" img-top>
                        <sup>Updated at: {{ dateConvert(value.updatedAt) }}</sup>
                        <b-button block variant="info" v-b-modal.modal-1 @click="show(value)">Details</b-button>
                    </b-card>
                </div>      
            </div>
        </div>
        
        <!-- Modal -->
        <b-modal id="modal-1" size="lg" :title='title'>
            <table class="table table-striped">
                <tr>
                    <td style="width: 14%;">Slug</td>
                    <td style="width: 1%;">:</td>
                    <td>{{ slug }}</td>
                </tr>
                <tr>
                    <td>Height</td>
                    <td>:</td>
                    <td>{{ height }}</td>
                </tr>
                <tr>
                    <td>Contingen</td>
                    <td>:</td>
                    <td>{{ continent }}</td>
                </tr>
                <tr>
                    <td>Countries</td>
                    <td>:</td>
                    <td>
                        {{ replaceString(countries) }}
                    </td>
                </tr>
            </table>
            <table class="table table-striped">
                <tr>
                    <td style="width: 14%;">Directory</td>
                    <td style="width: 1%;">:</td>
                    <td>{{ dir }}</td>
                </tr>
                <tr>
                    <td>Path</td>
                    <td>:</td>
                    <td>{{ path }}</td>
                </tr>
                <tr>
                    <td>Description</td>
                    <td>:</td>
                    <td>{{ description }}</td>
                </tr>
            </table>
        </b-modal>
    </div>
</template>

<script>
    import Vue from 'vue'
    import Box from '../components/box.vue'

    import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

    // Import Bootstrap and BootstrapVue CSS files (order is important)
    import 'bootstrap/dist/css/bootstrap.css'
    import 'bootstrap-vue/dist/bootstrap-vue.css'

    // Make BootstrapVue available throughout your project
    Vue.use(BootstrapVue)
    // Optionally install the BootstrapVue icon components plugin
    Vue.use(IconsPlugin)

    export default {
        layout: 'index',
        components: {Box},

        data() {
            return {
                dir: '',
                slug: '',
                path: '',
                title: '',
                height: '',
                countries: '',
                continent: '',
                mountains: [],
                updated_at: '',
                description: '',
            }
        },
        methods: {
            show(data) {
                this.dir = data.dir
                this.slug = data.slug
                this.path = data.path
                this.title = data.title
                this.height = data.height
                this.countries = data.countries
                this.continent = data.continent
                this.updated_at = data.updatedAt
                this.description = data.description
            },
            replaceString(str) {
                str = str.toString()
                str = str.replace(',', ', ')
                return str
            },
            dateConvert(date) {
                return new Date(date)
            }
        },
        async fetch() {
            this.mountains = await fetch(
                'https://api.nuxtjs.dev/mountains'
            ).then(
                res => res.json(),
            )
        },
    }
</script>
<style scoped>
    .card-img-top {
        height: 260px;
    }

    sup#url-api {
        top: -1px !important;
    }
    
    .modal-header {
        border-bottom: 0px solid transparent;
    }
</style>