<template>
	<div id="TodoList">
		<div class="header_background todoList_header">
			<h1
					v-if="!isChangeMode"
					@dblclick="changeChangeMode"
			>
				{{TodoList.nameList}}
			</h1>
			<UpdateForm
					v-if="isChangeMode"
					:newName="this.newName"
					:isTodoList="true"
					@change-name="changeName"
					@close-form="changeChangeMode"
			/>
			<button
					class="btn_edit"
					v-if="editMode"
					@click="changeEditMode"
			>
				<i class="material-icons">edit</i>
			</button>
			<button
					class="btn_edit"
					v-else
					@click="$emit('add-todo')"
			>
				<i class="material-icons">add_box</i>
			</button>
			<button
					class="btn_remove"
					v-if="editMode"
					@click="removeTodoList"
			>
				<i class="material-icons">cancel</i>
			</button>
		</div>
		<div class="conteiner">
			<ul v-if="TodoList.todos.length">
				<TodoItem
						v-for="todo of TodoList.todos"
						:todo="todo"
						:editMode="editMode"
						:key="todo.idTodo"
						@remove-todo="removeTodo"
						@change-todo-completed="changeTodoCompleted"
						@change-name-todo="changeNameTodo"
				/>
			</ul>
			<p v-else>No To-Do!</p>
		</div>
	</div>
</template>
<script lang="ts">
    import Vue from "vue"
    import TodoItem from "@/components/TodoItem.vue"
    import UpdateForm from "@/components/ChangeNameForm.vue"

    export default Vue.extend({
        name: "TodoList",
        components: {
            TodoItem, UpdateForm
        },
        props: {
            TodoList: {
                type: Object
            },
            editMode: Boolean,
        },
        data() {
            return {
                isChangeMode: false,
                newName: this.TodoList.nameList
            }
        },
        methods: {
            changeTodoCompleted(idTodo: number): void {
                this.$emit('change-todo-completed', idTodo)
            },
            changeEditMode(): void {
                this.$emit('change-edit-mode', this.TodoList.idList)
            },
            changeChangeMode(): void {
                if (this.editMode == false) {
                    this.isChangeMode = !this.isChangeMode
                }
            },
            removeTodoList(): void {
                this.$emit('remove-todo-list', this.TodoList.idList)
            },
            removeTodo(idTodo: number): void {
                this.$emit('remove-todo', idTodo)
            },
            changeName(newName: string): void {
                this.$emit('change-name-list', newName)
                this.changeChangeMode()
                this.newName = newName
            },
            changeNameTodo(idTodo: number, newName: string): void {
                this.$emit('change-name-todo', idTodo, newName)
            }
        }
    })
</script>
<style scoped>
	p{
		font-weight: bold;
	}
	ul {
		height: 18rem;
		list-style: none;
		margin: 0;
		padding: 0;
		overflow-y: auto;
	}

	.conteiner {
		background-color: #E6E6E6;
		width: 410px;
		height: 300px;
		border: 1px solid black;
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px;
		margin: 0 1rem 1rem 1rem;
		padding-top: .5rem;
	}

	.todoList_header {
		margin: 0 1rem;
		height: 3rem;
		width: 410px;
		border-top-left-radius: 10px;
		border-top-right-radius: 10px;
		position: relative;
	}

	.btn_remove {
		position: absolute;
		top: .5rem;
		right: .5rem;
	}

	.btn_edit {
		position: absolute;
		top: .5rem;
		left: 1rem;
	}
</style>