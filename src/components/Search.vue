<template>
    <div class="input-group flex-nowrap">
        <!-- <span class="input-group-text" id="addon-wrapping">Search</span> -->
        <input v-model="searchQuery" type="text" class="form-control" placeholder="Search Pokemon">
    </div>
</template>

<script>
export default {
    props: [
        'pokemons'
    ],

}
</script>