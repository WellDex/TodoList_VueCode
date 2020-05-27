<template>
	<div id="app">
		<header class="header_background">
			<button
					v-if="isVisibilityComponentHome"
					class="btn_createNewList"
					@click="changeVisibilityCreateList"
			>
				<i class="material-icons">add_box</i>
			</button>
			<button
					v-else
					class="btn_backToHome"
					@click="changeVisibilityComponentHome"
			>
				<i class="material-icons">reply</i>
			</button>
			<h1>My To-Do Lists</h1>
		</header>
		<Home
				v-if="isVisibilityComponentHome"
				:TodoLists="TodoLists"
				:editMode="isVisibilityComponentHome"
				@remove-todo-list="removeTodoList"
				@change-edit-mode="changeEditMode"
		/>
		<ChangeTodo
				v-else
				:TodoList="TodoListForChange"
				:editMode="isVisibilityComponentHome"
				@add-todo="createNewTodo"
				@remove-todo="removeTodo"
				@change-todo-completed="changeTodoCompleted"
				@change-name-list="changeNameList"
				@change-name-todo="changeNameTodo"
		/>
		<Modal-window
				v-if="isVisibilityModal"
				:modalInfo="this.modalInfo"
				@submit-modal="submitModal"
				@close-modal="changeVisibilityModal"
		>
			<slot>
				<p>Вы действительно хотите {{textForModal}} todo?</p>
			</slot>
		</Modal-window>
		<Create-todo-list
				v-if="isVisibilityCreateList"
				@add-todo-list="createNewList"
				@close-create-list="changeVisibilityCreateList"
				@remove-list-todo="removeTodoList"
		/>
	</div>
</template>
<script lang="ts">
    import {Vue} from "vue-property-decorator"
    import Home from "@/views/Home.vue"
    import ChangeTodo from "@/views/ChangeTodo.vue"
    import ModalWindow from "@/components/ModalWindow/ModalWindow.vue"
    import CreateTodoList from "@/components/ModalWindow/CreateTodoList.vue"

    export default Vue.extend({
        name: "app",
        components: {
            Home, ChangeTodo, ModalWindow, CreateTodoList
        },
        data() {
            return {
                TodoLists: [{
                    idList: null as number | null,
                    nameList: null as string | null,
                    todos: [{
                        idTodo: null as number | null,
                        title: null as string | null,
                        completed: null as boolean | null
                    }]
                }] as Array<{
                    idList: number | null;
                    nameList: string | null;
                    todos: Array<{
                        idTodo: number | null;
                        title: string | null;
                        completed: boolean | null;
                    }>;
                }>,
                isVisibilityComponentHome: true,
                isVisibilityModal: false,
                isVisibilityCreateList: false,
                modalInfo: {
                    idTodo: null as number | null,
                    idList: null as number | null,
                    action: null as string | null
                },
                textForModal: '',
                TodoListForChange: {}
            }
        },
        methods: {
            submitModal(action: { type: string; idList: number }): void {
                switch (action.type) {
                    case 'DELETE_LIST': {
                        this.TodoLists = this.TodoLists.filter(t => t.idList !== action.idList)
                        this.saveInLocalStorage()
                        break
                    }
                }
                this.changeVisibilityModal()
            },
            changeTodoCompleted(idList: number, idTodo: number): void {
                for (const TodoList of this.TodoLists) {
                    if (idList === TodoList.idList) {
                        for (const todo of TodoList.todos) {
                            if (idTodo === todo.idTodo) {
                                todo.completed = !todo.completed
                                this.saveInLocalStorage()
                                break
                            }
                        }
                    }
                }
            },
            changeVisibilityModal(): void {
                this.isVisibilityModal = !this.isVisibilityModal
            },
            changeVisibilityCreateList(): void {
                this.isVisibilityCreateList = !this.isVisibilityCreateList
            },
            changeVisibilityComponentHome(): void {
                this.isVisibilityComponentHome = !this.isVisibilityComponentHome
            },
            changeEditMode(idList: number): void {
                for (const TodoList of this.TodoLists) {
                    if (TodoList.idList === idList) {
                        this.TodoListForChange = TodoList
                        this.saveInLocalStorage()
                        break
                    }
                }
                this.changeVisibilityComponentHome()
            },
            createNewList(newTodoList: { idList: number; nameList: string; todos: [] }): void {
                this.TodoLists.push(newTodoList)
                this.saveInLocalStorage()
                this.changeVisibilityCreateList()
            },
            createNewTodo(newTodo: { idTodo: number; title: string; completed: boolean }, idList: number): void {
                for (const TodoList of this.TodoLists) {
                    if (TodoList.idList === idList) {
                        TodoList.todos.push(newTodo)
                        this.saveInLocalStorage()
                        break
                    }
                }
            },
            removeTodoList(idList: number): void {
                this.textForModal = 'удалить список'
                this.modalInfo.action = 'DELETE_LIST'
                this.modalInfo.idList = idList
                this.saveInLocalStorage()
                this.changeVisibilityModal()
            },
            removeTodo(idList: number, idTodo: number): void {
                for (const TodoList of this.TodoLists) {
                    if (TodoList.idList === idList) {
                        TodoList.todos = TodoList.todos.filter(t => t.idTodo !== idTodo)
                        this.saveInLocalStorage()
                    }
                }
            },
            changeNameList(idList: number, newName: string): void {
                for (const TodoList of this.TodoLists) {
                    if (TodoList.idList === idList) {
                        TodoList.nameList = newName
                        this.saveInLocalStorage()
                        break
                    }
                }
            },
            changeNameTodo(idList: number, idTodo: number, newName: string): void {
                for (const TodoList of this.TodoLists) {
                    if (TodoList.idList === idList) {
                        for (const todo of TodoList.todos) {
                            if (todo.idTodo === idTodo) {
                                todo.title = newName
                                this.saveInLocalStorage()
                                break
                            }
                        }
                    }
                }
            },
            saveInLocalStorage(): void {
                localStorage.setItem('todoLists', JSON.stringify(this.TodoLists))
            }
        },
        created(): void {
            const data = localStorage.getItem('todoLists')
            if (data !== null) {
                this.TodoLists = JSON.parse(data)
            } else {
                this.TodoLists = []
            }
        }
    })
</script>
<style>
	* {
		box-sizing: border-box;
	}

	header {
		height: 4rem;
		padding: 0.6rem 0;
	}

	button {
		outline: none;
		width: 2rem;
		height: 1.7rem;
		padding: 0;
		background: linear-gradient(to bottom, #cd9c52 -20%, #50260c 100%);
		color: #D4AD73;
		border: 1px solid #7C3D14;
	}

	button:hover, input:hover {
		cursor: pointer;
	}

	.header_background {
		background: linear-gradient(to bottom, #cd9c52 -10%, #50260c 100%);
		color: #F0DDBF;
		position: relative;
	}

	.btn_createNewList {
		position: absolute;
		left: 1rem;
		top: 1rem;
	}

	.btn_backToHome {
		position: absolute;
		right: 1rem;
		top: 1rem;
	}

	h1 {
		margin: 0;
	}

	body {
		margin: 0
	}

	a {
		text-decoration: none;
	}

	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
	}
</style>
