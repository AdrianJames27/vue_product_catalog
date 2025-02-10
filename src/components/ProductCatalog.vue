<template>
    <div class="container">
        <div class="column">
            <h1>Product Catalog</h1>
            <div class="section">
                <p class="section-title">Product List</p>
                <div class="product-item" v-for="(product, index) in products" :key="index">
                    <p class="product-name">{{ product.name }}</p>
                    <p class="product-price">P{{ formatNumber(product.price.toFixed(2)) }}</p>
                    <button class="btn-add" @click="addToCart(product)">Add to Cart</button>
                </div>
            </div>
        </div>
        <div class="column">
            <div class="section">
                <p class="section-title">Cart Item List</p>
                <div class="product-item" v-for="(cartItem, index) in cartItems" :key="index">
                    <p class="product-name">{{ cartItem.name }}</p>
                    <span class="product-total">P{{ formatNumber(cartItem.price.toFixed(2)) }} x {{ cartItem.quantity }} = P{{ formatNumber(cartItem.totalPrice) }}</span>
                    <div class="quantity-controls">
                        <button class="quantity-button" @click="decreaseQuantity(cartItem)">-</button>
                        <input type="text" class="quantity-input" disabled :value="cartItem.quantity">
                        <button class="quantity-button" @click="increaseQuantity(cartItem)">+</button>
                    </div>
                    <button class="btn-remove" @click="removeToCart(index)">Remove Item</button>
                </div>
                <div class="grand-total-price">
                    <p v-if="cartItems.length === 0">No item is added yet. :)</p>
                    <p v-else>Grand Total: P{{ formatNumber(calculateGrandTotal) }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'ProductCatalog',
        data: function() {
            return {
                products: [
                    { name: 'Canned Tuna', price: 99.99 },
                    { name: 'Toilet Paper', price: 10.50 },
                    { name: 'iPhone 16 Pro Max', price: 56999.99 }
                ],
                cartItems: []
            }
        },
        computed: {
            calculateGrandTotal: function() {
                let grandTotal = 0;

                for (const cartItem of this.cartItems) {
                    grandTotal += cartItem.totalPrice;
                }

                console.log(grandTotal);

                return grandTotal;
            }
        },
        methods: {
            addToCart: function(product) {
                const cartItem = this.cartItems.find(cartItem => cartItem.name === product.name);
                
                if (cartItem) {
                    this.increaseQuantity(cartItem);
                } else {
                    const newCartItem = {...product, quantity: 1, totalPrice: 0 };
                    this.cartItems.push(newCartItem);
                    this.calculateTotalPrice(newCartItem);
                }
            },
            increaseQuantity: function(cartItem) {
                // can only add qty. up to 30
                if (cartItem.quantity < 30) {
                    cartItem.quantity++;
                    this.calculateTotalPrice(cartItem);
                }
            },
            decreaseQuantity: function(cartItem) {
                // can only remove qyt. down to 1
                if (cartItem.quantity > 1) {
                    cartItem.quantity--;
                    this.calculateTotalPrice(cartItem);
                }
            },
            calculateTotalPrice: function(cartItem) {
                cartItem.totalPrice = cartItem.price * cartItem.quantity;
            },
            removeToCart: function(index) {
                this.cartItems.splice(index, 1);
            },
            formatNumber: function(number) {
                return new Intl.NumberFormat('en-US').format(number);
            }
        }
    }
</script>

<style scoped>
    .container {
        display: flex;
        justify-content: space-between;
        gap: 20px;
    }
    .column {
        flex: 1;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }

    .btn-add {
        background: #28a745;
        color: white;
        border: none;
        padding: 8px 12px;
        border-radius: 5px;
        cursor: pointer;
        transition: background 0.3s;
    }

    .btn-add:hover {
        background: #218838;
    }

    .btn-remove {
        background: #dc3545;
        color: white;
        border: none;
        padding: 8px 12px;
        border-radius: 5px;
        cursor: pointer;
        transition: background 0.3s;
    }

    .btn-remove:hover {
        background: #c82333;
    }

    .section-title {
        font-size: 1.5em;
        margin-bottom: 10px;
    }
    .product-item {
        margin: 10px 0;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .quantity-controls {
        display: flex;
        align-items: center;
    }
    .quantity-input {
        width: 40px;
        text-align: center;
        margin: 0 5px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
    .grand-total-price {
        font-weight: bold;
        margin-top: 20px;
        font-size: 1.2em;
        text-align: center;
    }
</style>