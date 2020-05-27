<template>
	<li>
		<span
				:class="{completed: todo.completed}"
				class="todo_flex"
		>
			<div class="todo_input">
				<input
						:class="{showInput: editMode}"
						type="checkbox"
						@change="changeTodoCompleted"
						:checked="todo.completed"
				>
			</div>
			<div class="todo_title">
				<span
						v-if="!isChangeMode"
						@dblclick="changeChangeMode"
				>
					{{todo.title | uppercase}}
				</span>
				<UpdateForm
						v-if="isChangeMode"
						:isTodoList="false"
						:newName="this.newName"
						@change-name="changeName"
						@close-form="changeChangeMode"
				/>
			</div>
		</span>
		<button
				@click="removeTodo"
				v-if="!editMode"
		>
			<i class="material-icons">delete_forever</i>
		</button>
	</li>
</template>
<script lang="ts">
    import Vue from "vue"
    import UpdateForm from "@/components/ChangeNameForm.vue"

    export default Vue.extend({
        name: "TodoItem",
        components: {
            UpdateForm
        },
        props: {
            todo: {
                type: Object,
                required: true
            },
            editMode: Boolean,
        },
        data() {
            return {
                isChangeMode: false,
                newName: this.todo.title
            }
        },
        filters: {
            uppercase(title: string): string {
                return title.toUpperCase()
            }
        },
        methods: {
            removeTodo(): void {
                this.$emit('remove-todo', this.todo.idTodo)
            },
            changeTodoCompleted(): void {
                this.$emit('change-todo-completed', this.todo.idTodo)
                console.log('todo: ' + this.todo.idTodo)
            },
            changeChangeMode(): void {
                if (this.editMode == false) {
                    this.isChangeMode = !this.isChangeMode
                }
            },
            changeName(newName: string): void {
                this.$emit('change-name-todo', this.todo.idTodo, newName)
                this.changeChangeMode()
                this.newName = newName
            }
        }
    })
</script>
<style scoped>
	li {
		background-color: transparent;
		border-top: 2px solid #D3D5D7;
		color: #564F4D;
		font-weight: bold;
		display: flex;
		justify-content: space-between;
		padding: 0 1rem;
		align-items: center;
	}

	.showInput {
		visibility: hidden;
	}

	button {
		background: transparent;
		border: none;
		color: #7C3D14;
	}

	input {
		width: 25px;
		height: 25px;
		margin-right: 0.3em;
	}

	span {
		vertical-align: super;
	}

	.todo_flex {
		display: flex;
		align-items: center;
		justify-content: flex-start;
	}

	.todo_title {
		position: relative;
		padding: .3rem 0 .3rem .5rem;
	}

	.todo_input {
		border-right: 1px solid red;
		padding: .3rem .5rem .3rem 0;
	}

	.completed {
		text-decoration: line-through;
		color: #AFADAB;
	}
</style>