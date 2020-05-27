<template>
	<div>
		<div class="content flex-content">
			<TodoList
					:TodoList="TodoList"
					:editMode="editMode"
					@add-todo="changeVisibilityCreateTodo"
					@change-todo-completed="changeTodoCompleted"
					@remove-todo="removeTodo"
					@change-name-list="changeNameList"
					@change-name-todo="changeNameTodo"
			/>
		</div>
		<CreateTodo
				v-if="isVisibilityCreateTodo"
				@add-todo="createNewTodo"
				@close-create-todo="changeVisibilityCreateTodo"
		/>
	</div>
</template>
<script lang="ts">
    import Vue from 'vue'
    import TodoList from "@/components/TodoList.vue"
    import CreateTodo from "@/components/ModalWindow/CreateTodo.vue"

    export default Vue.extend({
        name: "ChangeTodo",
        components: {
            TodoList, CreateTodo
        },
        props: {
            TodoList: {
                type: Object
            },
            editMode: Boolean
        },
        data() {
            return {
                isVisibilityCreateTodo: false,
                idListForAddTodo: null as number | null
            }
        },
        methods: {
            changeVisibilityCreateTodo(): void {
                this.isVisibilityCreateTodo = !this.isVisibilityCreateTodo
            },
            changeTodoCompleted(idTodo: number): void {
                this.$emit('change-todo-completed', this.TodoList.idList, idTodo)
            },
            createNewTodo(newTodo: { idTodo: number; title: string; completed: boolean }): void {
                this.$emit('add-todo', newTodo, this.TodoList.idList)
                this.changeVisibilityCreateTodo()
            },
            removeTodo(idTodo: number): void {
                this.$emit('remove-todo', this.TodoList.idList, idTodo)
            },
            changeNameList(newName: string): void {
                this.$emit('change-name-list', this.TodoList.idList, newName)
            },
            changeNameTodo(idTodo: number, newName: string): void {
                this.$emit('change-name-todo', this.TodoList.idList, idTodo, newName)
            }
        },
    })
</script>
<style scoped>
	.flex-content {
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
		margin: 0 -1rem;
	}

	.content {
		max-width: 1800px;
		margin: 1rem auto 0 auto;
		padding-left: 15px;
		padding-right: 15px;
	}

	a {
		text-align: center;
		width: 50px;
		color: #000;
		border: 1px solid #000;
		border-radius: 5px;
		background-color: #ccc;
	}
</style>