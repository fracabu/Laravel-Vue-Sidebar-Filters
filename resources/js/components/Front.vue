<template>
    <div class="container" :class="{'loading': loading}">
        <div class="row">
            <div class="col-lg-3 mb-4">
                <h1 class="mt-4">Filters</h1>

                

                <h3 class="mt-2">Categories</h3>
                <div class="form-check" v-for="(category, index) in categories">
                    <input class="form-check-input" type="checkbox" :value="category.id" :id="'category'+index" v-model="selected.categories">
                    <label class="form-check-label" :for="'category' + index">
                        {{ category.name }} ({{ category.products_count }})
                    </label>
                </div>

                
            </div>
            
        </div>
    </div>
</template>



<script>
    export default {
        data: function () {
            return {
                categories: [],
                loading: true,
                selected: {
                    categories: [],
                
                }
            }
        },
        mounted() {
            this.loadCategories();
        },
        watch: {
            selected: {
                handler: function () {
                    this.loadCategories();
                },
                deep: true
            }
        },
        methods: {
            loadCategories: function () {
                axios.get('/api/categories', {
                        params: _.omit(this.selected, 'categories')
                    })
                    .then((response) => {
                        this.categories = response.data.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            }
        }
    
</script>
