<template>
    <header class="section-header fixed-top">
        <section class="header-main border-bottom">
            <div class="container-fluid">
                <div class="row align-items-center">
                    <div class="col-lg-3 col-sm-4 col-md-4 col-5">
                        <nuxt-link to="/" class="brand-wrap" data-abc="true">
                            <span class="logo">NUXT-STORE</span>
                        </nuxt-link>
                    </div>
                    <div class="col-lg-5 mx-auto">
                        <div class="search-wrap">
                            <div class="input-group w-100">
                                <input type="text" class="form-control search-form" v-model="search"
                                    @keypress.enter="searchData" style="width:55%;"
                                    placeholder="mau belanja apa hari ini ?">
                                <div class="input-group-append">
                                    <button @click="searchData" class="btn btn-primary search-button"> <i
                                            class="fa fa-search"></i> </button>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-5 col-xl-4 col-sm-8 col-md-4 col-7">
                        <div class="d-flex justify-content-end">
                            <nuxt-link :to="{
                                name: 'cart'
                            }" class="btn search-button btn-md d-md-block ml-4"><i class="fa fa-shopping-cart"></i>
                                <span class="ml-2">{{ cartTotal }}</span> | Rp. {{
                                        formatPrice(cartPrice)
                                }}</nuxt-link>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <nav class="navbar navbar-expand-md navbar-main border-bottom p-2">
            <div class="container-fluid">
                <button class="navbar-toggler collapsed" type="button" data-toggle="collapse" data-target="#dropdown6"
                    aria-expanded="false"> <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse" id="dropdown6">
                    <ul class="navbar-nav">
                        <li class="nav-item dropdown"> <a class="nav-link dropdown-toggle" href="#"
                                data-toggle="dropdown" data-abc="true" aria-expanded="false"><i
                                    class="fa fa-list-ul"></i> KATEGORI</a>
                            <div class="dropdown-menu">
                                <nuxt-link :to="{ name: 'categories-slug', params: { slug: category.slug } }"
                                    class="dropdown-item" v-for="category in categories" :key="category.id">
                                    <img :src="category.image" width="50" height="50"> {{ category.name }}
                                </nuxt-link>
                                <div class="dropdown-divider"></div>

                                <!-- Semua Category -->
                                <nuxt-link :to="{ name: 'categories' }" class="dropdown-item active text-center" href=""
                                    data-abc="true">
                                    LIHAT SEMUA KATEGORI <i class="fa fa-long-arrow-alt-right"></i>
                                </nuxt-link>
                            </div>
                        </li>
                        <li class="nav-item"> <a href="#" class="nav-link" data-abc="true"><i
                                    class="fa fa-shopping-bag"></i> SEMUA PRODUK</a> </li>
                        <li class="nav-item"> <a href="#" class="nav-link" data-abc="true"><i
                                    class="fa fa-info-circle"></i> TENTANG</a> </li>
                        <li class="nav-item"> <a href="#" class="nav-link" data-abc="true"><i
                                    class="fa fa-comments"></i> KONTAK</a> </li>
                    </ul>
                    <ul class="navbar-nav ml-auto">
                        <li class="nav-item dropdown" v-if="!$auth.loggedIn">
                            <nuxt-link :to="{ name: 'customer-login' }" class="nav-link" href="#" role="button"
                                aria-expanded="false"> <i class="fa fa-user-circle"></i>
                                ACCOUNT
                            </nuxt-link>
                        </li>
                        <li class="nav-item dropdown" v-if="$auth.loggedIn">
                            <nuxt-link :to="{ name: 'customer-dashboard' }" class="nav-link" href="#" role="button"
                                aria-expanded="false"> <i class="fa fa-tachometer-alt"></i>
                                DASHBOARD</nuxt-link>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
</template>
<script>

export default {

    data() {
        return {
            search: '',
        }
    },
    async fetch() {
        //fething sliders on Rest API
        await this.$store.dispatch('web/category/getCategoryHeader')

        if (this.$auth.loggedIn && this.$auth.strategy.name == 'customer') {
            //fething carts on Rest API
            await this.$store.dispatch('web/cart/getCartsData')
            await this.$store.dispatch('web/cart/getCartPrice')

        }
    },
    computed: {
        categories() {
            return this.$store.state.web.category.categoryHeader
        },
        //cartPrice
        cartPrice() {
            return this.$store.state.web.cart.cartPrice
        },

        //cartTotal
        cartTotal() {
            return this.$store.state.web.cart.carts.length
        },
    },

    methods: {
        searchData() {
            this.$router.push({
                name: 'search',
                query: {
                    q: this.search
                }
            });
        }
    }
}
</script>
  
<style scope
.btn {
    font-size: initial;
}
}












































</style>