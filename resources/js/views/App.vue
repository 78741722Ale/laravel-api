<template>


        <div>
            <!-- Componente Work in progress -->
            <work-in-progress></work-in-progress>
            <!-- Section relativa ai posts -->
            <div class="posts">

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
            posts: ''
        }
    },
    /* Metodo */
    mounted() {
        /* Verifica del mounted */
        console.log('mounted');
        /* La nostra richiesta */
        axios
        .get('/api/posts') // Quello da ricavare
        .then((response) => {
            console.log(response);
        }).catch(e => {
            console.log(e);
        })
    },
}
</script>
