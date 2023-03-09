<template>
<div class="card" style="width: 18rem;">
    <div class="card-body">
        <h5 class="card-title">{{ prod.name}} 
            <span v-if="!prod.isAvailable" class="badge text-bg-danger">Unavailable</span>
        </h5>
        <p class="card-text">
            <ul>
                <li>Price : {{ prod.price }}.- CHF</li>
                <li>ID : {{ prod.id }}</li>
            </ul>
        </p>
        <p v-if="prod.isAvailable">
            <button @click="$emit('addToCart')" type="button" class="btn btn-info">Add to cart</button>
            <!--  -->
        </p>
    </div>
</div>
</template>
<script>
export default {
    props: ["prod"]
}

</script>