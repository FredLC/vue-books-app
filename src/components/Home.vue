<template>
    <div class="home">
        <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
            <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                    <a
                        href="#"
                        class="link-margin"
                        @click.prevent="selectCategory('all')"
                        >All</a
                    >
                    <a
                        href="#"
                        class="link-margin"
                        @click.prevent="selectCategory('featured')"
                        >Featured</a
                    >
                    <a
                        href="#"
                        v-for="category of data.categories"
                        :key="category.id"
                        class="link-margin"
                        @click.prevent="selectCategory(category.id)"
                    >
                        {{ category.id }}. {{ category.name }}
                    </a>
                </div>
            </template>
        </ApolloQuery>

        <div v-if="selectedCategory === 'all'">
            <ApolloQuery :query="query">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                    <div v-if="isLoading">Loading...</div>
                    <div v-else>
                        <div href="#" v-for="book of data.books" :key="book.id">
                            {{ book.id }}. {{ book.title }}
                        </div>
                    </div>
                </template>
            </ApolloQuery>
        </div>

        <div v-else-if="selectedCategory === 'featured'">
            <ApolloQuery :query="query" :variables="{ featured: true }">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                    <div v-if="isLoading">Loading...</div>
                    <div v-else>
                        <div
                            href="#"
                            v-for="book of data.booksFeatured"
                            :key="book.id"
                        >
                            {{ book.id }}. {{ book.title }}
                        </div>
                    </div>
                </template>
            </ApolloQuery>
        </div>

        <div v-else>
            <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
                <template slot-scope="{ result: { data, loading }, isLoading }">
                    <!-- Some content -->
                    <div v-if="isLoading">Loading...</div>
                    <div v-else>
                        <div
                            href="#"
                            v-for="book of data.category.books"
                            :key="book.id"
                        >
                            {{ book.id }}. {{ book.title }}
                        </div>
                    </div>
                </template>
            </ApolloQuery>
        </div>
    </div>
</template>

<script>
import categoryQuery from '@/graphql/queries/Category.gql';
import booksQuery from '@/graphql/queries/Books.gql';
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql';
import categoriesQuery from '@/graphql/queries/Categories.gql';
export default {
    name: 'Home',
    data() {
        return {
            categoryQuery,
            categoriesQuery,
            booksQuery,
            booksFeaturedQuery,
            selectedCategory: 'all',
            query: booksQuery,
            categories: [],
        };
    },
    methods: {
        selectCategory(category) {
            if (category === 'all') {
                this.query = booksQuery;
            } else if (category === 'featured') {
                this.query = booksFeaturedQuery;
            } else {
                this.query = categoryQuery;
            }
            this.selectedCategory = category;
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.link-margin {
    margin-right: 24px;
}
</style>
