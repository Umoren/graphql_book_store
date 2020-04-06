<template name="component-name">
    <div>
        <ApolloQuery 
            :query=" gql => gql`
            query allBooks{
                books{
                id
                title
                description
                is_completed
                author{
                    id
                    name
                }
                }
            }
            `">
            <template v-slot="{result: {loading, error, data}}">
                <div v-if="loading"> Loading ...</div>
                <h2 v-if="error"> Please try again</h2>
                <div v-if="data">
                    <div v-for="book in data.books" :key="book.id"> 
                    <h5 >{{book.title}}</h5>
                    <button class="btn btn-sm btn-danger" @click="removeBook(book)"> delete book</button>
                    <small> {{book.is_completed}}</small>

                    <p> {{book.description}} </p>
                    </div>
                </div>       
            </template>
        </ApolloQuery>
    </div>
</template>

<script>
import gql from 'graphql-tag';
import allbooks from '../components/allbooks';

const REMOVE_BOOK = gql `
  mutation delete_books($id: Int!){
  delete_books(where: {id: {_eq: $id}}){
    affected_rows
  }
}
  
`;

export const ALL_BOOKS = gql `
    query allBooks{
        books {
            id
            title
            description
            is_completed
            author{
            id
            name
            }
        }
}`;


export default {
  name: 'AllBooks',
 
  methods: {
 
    removeBook(book){
    this.$apollo.mutate({
        mutation: REMOVE_BOOK,
        variables: {
          id: book.id
        },
        refetchQueries: [
          {
            query: ALL_BOOKS
          }       
          
        ]
      })
    }
  }
 
}
</script>
