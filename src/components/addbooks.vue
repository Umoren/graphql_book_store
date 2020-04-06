<template name="addbooks">
    <div class="form_input mx-auto col-md-6">
        <form class="form-group" @submit.prevent>
            <div>
                <label for="title"> Book Title</label>
                <input 
                    type="text" 
                    class="form-control" 
                    placeholder="Book Title"
                    v-model="newTitle"
                />
            </div>
       
            <!-- <div class="text-left">
               <label> Have you completed this book? </label>
               <el-select v-model="newStatus" placeholder="Select">
                    <el-option
                    v-for="(status, key) in data.insert_books.returning"
                    :key="key"
                    :label="status.title"
                    :value="status.id">
                    </el-option>
                </el-select> 
            </div> -->
            <div>
                <label for="description"> Book Description</label>
                <textarea    type="text" 
                          class="form-control"
                          placeholder="Book description"
                          v-model="newDesc" />
               
            </div>

            <button class="btn btn-primary mt-2" @click="addBook"  > Apply</button>
        </form>
    </div>
</template>

<script>

    import gql from "graphql-tag";
    import { ALL_BOOKS } from './allbooks.vue';

     const ADD_BOOKS = gql`
        mutation ($title: String!, $description: String!) {
        insert_books(objects: {title: $title, description: $description}) {
            affected_rows
            returning {
                id
                title
                description
                created_at
            }
        }
    }`;

   

    export default {
    
        data() {
            return {
                newTitle: '',
                newDesc: ''
            }
        },
        methods:{
            addBook() {
                const { newTitle, newDesc } = this.$data;
                const title = this.newTitle && this.newTitle.trim();
                const description= this.newDesc && this.newDesc.trim() ;
                this.$apollo.mutate({
                    mutation: ADD_BOOKS,
                    variables: {    
                        description,
                        title
                    },
                    update: (cache, {data: { insert_books}}) => {
                        // Read data from cache for this query

                        try {
                            const insertedBook = insert_books.returning;
                            console.log(insertedBook)
                            cache.writeQuery({
                                query: ALL_BOOKS
                            })
                        }
                        catch (err) {
                            console.error(err)
                        }

                    }
                }).then((data) => {
                    this.$router.push({path: '/'})
                }).catch(err => console.log(err));
                this.newTitle = '';
                this.newDesc = '';
            },

        
        }
    }
</script>