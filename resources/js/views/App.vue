<template>
        <div>
            <!-- Componente Work in progress -->
            <banner-component></banner-component>
            <!-- Section relativa ai posts -->
            <section class="posts">
                <div class="container">
                    <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 gy-4">
                        <!-- Singola colonna da replicare con ciclo v-for -->
                        <div class="col" v-for="post in postsResponse.data" :key="post.id">
                            <div class="product card">
                                <!-- Immagine -->
                                <img :src="post.cover" :alt="post.title">
                                <!-- Corpo del prodotto -->
                                <div class="card-body">
                                    <!-- Titolo -->
                                    <h3>{{post.title}}</h3>
                                    <!-- Paragrafo -->
                                    <p>{{post.content}}</p>
                                </div>
                                <!-- Card footer (metadati) -->
                                <div class="card-footer">
                                    <div class="row">
                                        <!-- Utente -->
                                        <div class="col">
                                            <div class="author" v-if="post.user">
                                                <h5> Author </h5>
                                                <p>{{post.user.name}}</p>
                                            </div>
                                        </div>
                                        <!-- Categoria -->
                                        <div class="col">
                                            <span v-if="post.category"><strong>Category :</strong> {{post.category.name}} </span>
                                            <!-- Lista di tag -->
                                            <div class="tags" v-if="post.tags.length > 0">
                                                <!-- Titolo -->
                                                <strong>Tags :</strong>
                                                <!-- lista non ordinata -->
                                                <ul>
                                                    <li v-for="tag in post.tags" :key="tag.id">{{tag.name}}</li>
                                                </ul>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <!-- Navbar Sotto per cambio pagina -->
                <nav class="py-3" aria-label="Page navigation">
                    <ul class="pagination justify-content-center">
                        <!-- Pagina Precedente -->
                        <li class="page-item" v-if="postsResponse.current_page > 1">
                            <a class="page-link" href="#" aria-label="Previous" @click.prevent="getAllPosts(postsResponse.current_page - 1)">
                                <span aria-hidden="true">&laquo;</span>
                                <span class="visually-hidden">Previous</span>
                            </a>
                        </li>
                        <!-- Link alla pagina dinamico -->
                        <li :class="{'page-item' : true, 'active': page == postsResponse.current_page}" v-for="page in postsResponse.last_page" :key="page.id">
                            <a class="page-link" href="#" @click.prevent="getAllPosts(page)">{{page}}</a>
                        </li>
                        <!-- Pagina successiva -->
                        <li class="page-item" v-if="postsResponse.current_page < postsResponse.last_page">
                            <a class="page-link" href="#" aria-label="Next" @click.prevent="getAllPosts(postsResponse.current_page + 1)">
                                <span aria-hidden="true">&raquo;</span>
                                <span class="visually-hidden">Succ</span>
                            </a>
                        </li>
                    </ul>
                </nav>
            </section>
            <!-- Barra laterale con Categorie -->
            <aside>
                <div class="categories">
                    <ul>
                        <!-- List Item dinamici -->
                        <li v-for="category in categories" :key="category.id">{{category.name}}</li>
                    </ul>
                </div>
            </aside>
        </div>
</template>


<script>
import BannerComponent from '../components/BannerComponent';
export default {
    name: 'App',
    /* Componente */
    components: { BannerComponent },
    /* Data */
    data() {
        return {
            categories: '',
            postsResponse : ''
        }
    },
    methods: {
        /* Metodo per impaginazione */
        getAllPosts(postPage) {
            /* Verifica della current page */
            console.log(postPage);
            /* La nostra richiesta */
            axios
            .get('/api/posts', {
                /* Gestione delle pagine */
                params: {
                    page: postPage
                }
            })
            // Quello da ricavare
            .then((response) => {
                /* Verifica del responso */
                console.log(response);
                /* Prendiamo i nostri post */
                /* this.posts = response.data.data */
                this.postsResponse = response.data
            })
            .catch(e => {
                console.log(e);
            })
        },
        getAllCategories() {
            axios
            .get('/api/categories')
            .then(response => {
                /* Verifica del responso */
                console.log(response);
                this.categories = response.data;
            })
            .catch(e => {
                console.log(e);
            })
        }
    },
    /* Metodo */
    mounted() {
        /* Verifica del mounted */
        console.log('mounted');
        this.getAllPosts(1)
        this.getAllCategories(1)
    },
}
</script>
