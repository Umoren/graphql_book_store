<template name="component-name">
    <div class="allBooks">
        <router-link to="/addbook" class="btn btn-primary btn-sm mb-3 " > Add Book  </router-link>
        <ApolloQuery 
            :query=" gql => gql`
            query allBooks{
                books{
                id
                title
                description
                
                }
            }
            `">
          
            <template slot-scope="{ result: { loading, error, data } }">
                <div v-if="loading" class="loading apollo"> Loading ...</div>
                <h2 v-if="error"> Oh yikes :(  </h2>

                
                <div v-if="data" >
                
                    <div class="row"  > 
                        <div v-for="book in data.books" class="col-md-4 col-sm-12 col-lg-3 col-xl-3 " :key="book.id"> 
                            <div class="card shadow-lg  bg-danger mb-3">
                                <div class="card-body ">
                                    
                                    <h5 class="card-title" >{{book.title}}</h5>
                                        <p class="card-text"> {{book.description }} </p>
                                
                                    <div class="text-right"> 
                                        <button class="btn btn-sm btn-dark" @click="removeBook(book)"> delete book</button>
                                </div>
                            </div>
                            
                            <!-- <small> {{book.is_completed}}</small> -->


                            </div>
                        </div>
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
  


   filters: {
       readMore (text, length, suffix){
           return text.substring(0, length) + suffix
       }
   },
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

<style scoped>
    .allBooks{
        margin-top: 2em;
    }
</style>