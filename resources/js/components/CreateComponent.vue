// CreatePost.vue

<template>
    <div class="container">
        <div class="col-md-8 ml-auto mr-auto">
            <div class="form">
                <h3 class="text-center">Add new Content</h3>
                <form method="post" @submit.prevent="addPost">
                    <div class="form-group">
                        <label for="title">Title</label>
                        <input type="text" :class="['form-control', allerros.title ? 'is-invalid' : '']" name="title" id="title"
                               placeholder="Enter the title of the article" v-model="post.title" autofocus>
                        <div v-if="allerros.title" :class="['invalid-feedback']">
                            {{ allerros.title[0] }}
                        </div>
                    </div>

                    <div class="form-group">
                        <label for="description">Description</label>
                        <textarea :class="['form-control', allerros.body ? 'is-invalid' : '']" name="description" id="description"
                                  placeholder="Enter the description of the article" v-model="post.body" rows="5"></textarea>
                        <div v-if="allerros.body" :class="['invalid-feedback']">
                            {{ allerros.body[0] }}
                        </div>
                    </div>

                    <button type="submit" class="btn btn-primary btn-lg btn-block">Add</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                post: {},
                allerros: [],
                success: false,
            }
        },
        methods: {
            addPost() {
                // console.log(this.post);
                let uri = 'http://127.0.0.1:8000/api/post/create';
                this.axios.post(uri, this.post).then((response) => {

                    event.preventDefault();
                    Swal.fire({
                        title: 'Are you sure you want to register information ?',
                        type: 'warning',
                        focusConfirm: false,
                        showCancelButton: true,
                        confirmButtonColor: '#3085d6',
                        cancelButtonColor: '#d33',
                        confirmButtonText: 'Yes',
                        cancelButtonText: 'No',

                    }).then((result) => {
                        if (result.value) {
                            // alert('added');
                            this.$router.push({name: 'posts'});
                        }
                    })

                }).catch((error) => {
                    this.allerros = error.response.data.errors;
                    this.success = false;
                });
            },
        },
    }
</script>