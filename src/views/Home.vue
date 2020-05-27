<template>
	<div id="Home">
		<div class="content flex-root">
			<TodoList
					v-for="TodoList of TodoLists"
					:key="TodoList.idList"
					:TodoList="TodoList"
					:editMode="editMode"
					@change-edit-mode="changeEditMode"
					@remove-todo-list="removeTodoList"
			/>
		</div>
		<p v-if="!TodoLists.length">No To-Do List!</p>
	</div>
</template>
<script lang="ts">
    import Vue from 'vue'
    import TodoList from "@/components/TodoList.vue";

    export default Vue.extend({
        name: "Home",
        components: {
            TodoList
        },
        props: {
            TodoLists: {
                type: Array
            },
            editMode: Boolean
        },
        methods: {
            changeEditMode(idList: number) {
                this.$emit('change-edit-mode', idList)
            },
            removeTodoList(idList: number): void {
                this.$emit('remove-todo-list', idList)
            }
        },
    })
</script>
<style scoped>
	P{
		font-weight: bold;
		font-size: 20px;
	}
	.flex-root {
		display: flex;
		justify-content: flex-start;
		flex-wrap: wrap;
		margin: 0 -1rem;
	}

	.content {
		max-width: 1800px;
		margin: 1rem auto 0 auto;
		padding-left: 15px;
		padding-right: 15px;
	}
</style>