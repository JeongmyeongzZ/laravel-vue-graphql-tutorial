<template>
    <ApolloMutation :mutation="$options.mutation" :variables="{productId: id}">
        <button slot-scope="{mutate, loading}" :disabled="loading" @click="mutate()">
            <h3>{{ name }}</h3>
            <p class="price-label">Price: ${{ price }}</p>
            <p>You save: ${{ savings }}</p>
        </button>
    </ApolloMutation>
</template>

<script>
    import gql from 'graphql-tag';
    import ShoppingCartTotals from './ShoppingCartTotals';
    import ShoppingCartProductsList from './ShoppingCartProductsList';

    export default {
        props: {
            id: String,
            name: String,
            price: Number,
            savings: Number,
        },

        fragment: gql`
            fragment ProductButton on Product {
                id
                name
                price
                savings
            }
        `,

        mutation: gql`
            mutation productButton($productId: ID!) {
                addProductToShoppingCart(productId: $productId) {
                    shoppingCart {
                        id
                        ...ShoppingCartProductsList
                        ...ShoppingCartTotals
                    }
                }
            }

            ${ShoppingCartProductsList.fragment}
            ${ShoppingCartTotals.fragment}
        `,
    }
</script>

<style scoped>
    button {
        width: 110%;
        background-color: #b3c5e1;
        color: white;
        border: none;
        line-height: 14px;
        padding-top: 20px; 
        margin-top: 6px;
        cursor: pointer;
    }

    h3 {
        line-height: 24px;
        font-weight: bold;
        height: 60px;
    }

    .price-label {
        margin-bottom: 3px;
        margin-top: 20px;
        font-size: 1.1em;
        font-weight: bold;
    }
</style>
