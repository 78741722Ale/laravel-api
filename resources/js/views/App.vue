<template>


        <div>
            <!-- Componente Work in progress -->
            <work-in-progress></work-in-progress>
            <!-- Section relativa ai posts -->
            <div class="posts">
                <div class="container ">
                    <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
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
            </div>
        </div>


</template>


<script>
import WorkInProgress from '../components/WorkInProgress';
export default {
    name: 'App',
    /* Componente */
    components: { WorkInProgress },
    /* Data */
    data() {
        return {
            posts: '',
            postsResponse : ''
        }
    },
    methods: {
        /* Metodo per impaginazione */
        getAllPosts() {
            /* La nostra richiesta */
            axios
            .get('/api/posts') // Quello da ricavare
            .then((response) => {
                /* Verifica del responso */
                console.log(response);
                /* Prendiamo i nostri post */
                this.posts = response.data.data
                this.postsResponse = response.data

            }).catch(e => {
                console.log(e);
            })
        }
    },
    /* Metodo */
    mounted() {
        /* Verifica del mounted */
        console.log('mounted');
        this.getAllPosts()
    },
}
</script>
