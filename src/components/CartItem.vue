<template>
    <div class="card" style="width: 18rem;">
    
    <div class="card-body">
        <h5 class="card-title">{{ cartItem[0].name }}
        </h5>
        <p class="card-text">
            <ul>
                <li>Price : {{ cartItem[0].price }}.- CHF</li>
                <li>Quantity : {{ cartItem[1] }}</li>
                <li>Total : {{ cartItem[1] * cartItem[0].price }}.- CHF</li>
            </ul>
        </p>
        <p>
            <button type="button" @click="$emit('removeFromCart')" class="btn btn-danger">Remove from cart</button>
        </p>
    </div>
</div>
</template>
<script>
export default {
    props: ["cartItem"]
}

</script>