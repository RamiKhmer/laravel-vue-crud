<template>
    <div class="product-list">
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Name</th>
                    <th scope="col">Description</th>
                    <th scope="col">Price</th>
                    <th scope="col">Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(product,index) in products" :key="product.id">
                    <td>{{ index+1 }}</td>
                    <td>{{ product.name }}</td>
                    <td>{{ product.description }}</td>
                    <td>{{ product.price }}</td>
                    <td>
                        <div class="row gap-3">
                            <router-link :to="`/products/${product.id}`"
                                class="p-2 col border btn btn-sm btn-primary">View</router-link>
                            <router-link :to="`/products/${product.id}/edit`"
                                class="p-2 col border btn btn-sm btn-success">Edit</router-link>
                            <button @click="deleteProduct(product.id)" type="button"
                                class="p-2 col border btn btn-sm btn-danger">Delete</button>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            products: []
        }
    },
    async created() {
        try {
            const response = await axios.get('/api/products');
            this.products = response.data;
        } catch (error) {
            console.error(error);
        }
    },
    methods: {
        async deleteProduct(id) {
            try {

                Swal.fire({
                    title: 'ត្រូវការលុបទិន្នន័យ?',
                    text: "សូមបញ្ជាក់ថាអ្នកមិនអាចកែប្រែវិញបានទេ!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, Delete it!'
                }).then((result) => {
                    if (result.isConfirmed) {

                         axios.delete(`/api/products/${id}`);
                this.products = this.products.filter(product => product.id !== id);
                        Swal.fire(
                            'បានលុប!',
                            'ទិន្នន័យត្រូវបានលុប.',
                            'ប្រតិបត្តិការជោគជ័យ'
                        )
                    }
                })


                
            } catch (error) {
                console.error(error);
            }
        }
    }
}
</script>