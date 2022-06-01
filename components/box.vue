<template>
    <div>
        <b-modal id="modal-1" size="lg" :title='title'>
            <table class="table table-striped">
                <tr>
                    <td style="width: 14%;">Slug</td>
                    <td style="width: 1%;">:</td>
                    <td>{{ slug }}</td>
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
                    <td style="width: 14%;">Description</td>
                    <td style="width: 1%;">:</td>
                    <td>{{ description }}</td>
                </tr>
                
            </table>
        </b-modal>
    </div>
</template>

<script>
    export default {
        data () {
            
            return {
                slug: '',
                title: '',
                countries: '',
                description: '',
            }
        },
        methods: {
            replaceString(str) {
                str = str.toString()
                str = str.replace(',', ', ')
                return str
            },
        },
        mounted() { 
            this.$nuxt.$on('example', (data) => {
                this.slug = data.slug
                this.title = data.title
                this.countries = data.countries
                this.description = data.description
            })
        },
    }
</script>