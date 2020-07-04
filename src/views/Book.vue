<template>
    <div class="book">
        <ApolloQuery
            :query="require('@/graphql/queries/Book.gql')"
            :variables="{ id: $route.params.id }"
        >
            <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                    <div>
                        <div>{{ data.book.title }}</div>
                        <div>{{ data.book.author }}</div>
                        <img :src="data.book.image" alt="book cover" />
                    </div>
                </div>
            </template>
        </ApolloQuery>
    </div>
</template>
