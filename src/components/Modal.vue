<template>
    <div class="modal fade" :id="'modalDetail'+ num" tabindex="-1" aria-labelledby="modalDetailLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="modalDetailLabel">{{ upperFirstLetter }}</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="d-flex justify-content-center">
                        <img :src="currentImg" alt="Pokemon">
                    </div>
                    Name
                    <h3>{{upperFirstLetter}}</h3>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    props: {
        num: Number,
        name: String
    },
    computed: {
        upperFirstLetter: function(){
            let newName = this.name[0].toUpperCase() + this.name.slice(1);
            return newName;
        }
    },
}
</script>