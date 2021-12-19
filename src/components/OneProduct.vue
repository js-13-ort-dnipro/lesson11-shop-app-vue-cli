<template>
    
    <div class="card p-3">
        <img :src="product.image" class="card-img-top" alt="">
        <div class="card-body">
            <h5 class="card-title">
                {{product.title}}
            </h5>
            <p class="card-text">
                {{product.description}}
            </p>
            <h3 class='text-end'>{{product.price.toFixed(2)}}$</h3>
            <button 
                @click="$emit('add-to-cart', product.id)"
                :disabled="product.qt"
                class="w-100 btn btn-success"
            >
                <span v-if="!product.qt">Add To Cart</span>
                <span v-else>Already In Cart</span>
            </button>
        </div>
    </div>
        
</template>

<script>

    export default{
        name: 'OneProduct',
        props: ['product']
    }

</script>

<style scoped>

    img {
        max-height: 180px;
        object-fit: contain;
    }

</style>