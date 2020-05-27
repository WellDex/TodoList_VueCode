<template>
	<div class="shadow_modal">
		<div class="modal-window">
			<div class="modal_content">
				<input
						type="text"
						placeholder="write the name of the list"
						v-model="nameList"
				>
			</div>
			<div class="modal_footer">
				<button
						class="submit_btn"
						@click="submitCreate"
				>Ok
				</button>
				<button
						class="cancel_btn"
						@click="$emit('close-create-list')"
				>Cancel
				</button>
			</div>
		</div>
	</div>
</template>
<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend({
        name: "AddTodoList",
        data() {
            return {
                nameList: ''
            }
        },
        methods: {
            submitCreate(): void {
                if (this.nameList.trim()) {
                    const newTodoList = {
                        idList: Date.now(),
                        nameList: this.nameList,
                        todos: []
                    }
                    this.$emit('add-todo-list', newTodoList)
                    this.nameList = ''
                }
            }
        },
    })
</script>
<style scoped>
	input {
		width: 15rem;
		height: 2rem;
		margin-bottom: 0.5rem;
		padding-left: 0.5rem;
	}

	button {
		width: 70px;
		height: 40px;
		border: 1px solid #000;
		border-radius: 5px;
	}

	.submit_btn {
		margin-right: 5px;
		color: #fff;
		background-color: green;
	}

	.cancel_btn {
		margin-left: 5px;
		color: #fff;
		background-color: red;
	}

	.modal-window {
		border-radius: 10px;
		padding: 16px;
		max-width: 500px;
		background: #fff;
		box-shadow: 0 0 17px 0 #e7e7e7;
		margin: 10% auto;
		z-index: 999;
	}

	.shadow_modal {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-color: rgba(0, 0, 0, 0.5);
		z-index: 988;
		align-items: center;
	}

	.modal_content {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.modal_footer {
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>