<template>
	<form @submit.prevent="onSubmit">
		<input
				type="text"
				placeholder="write your tiitle todo"
				v-model="title"
		>
		<button type="submit">Create</button>
	</form>
</template>
<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend({
        name: "AddTodo",
        data() {
            return {
                title: ''
            }
        },
        methods: {
            onSubmit(): void {
                if (this.title.trim()) {
                    const newTodo = {
                        id: Date.now(),
                        title: this.title,
                        completed: false
                    }
                    this.$emit('add-todo', newTodo)
                    this.title = ''
                }
            }
        }
    })
</script>
<style scoped>
</style>