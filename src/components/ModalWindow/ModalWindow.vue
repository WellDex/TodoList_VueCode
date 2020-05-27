<template>
	<div class="shadow_modal">
		<div class="modal-window">
			<div class="modal_content">
				<slot>
				</slot>
			</div>
			<div class="modal_footer">
				<button
						class="submit_btn"
						@click="submitModal"
				>Ok
				</button>
				<button
						class="cancel_btn"
						@click="$emit('close-modal')"
				>Cancel
				</button>
			</div>
		</div>
	</div>
</template>
<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend({
        name: "ModalWindow",
        props: {
            modalInfo: {
                type: Object,
                required: true
            }
        },
        data() {
            return {
                idTodo: this.modalInfo.idTodo as number | undefined,
                idList: this.modalInfo.idList as number,
                action: this.modalInfo.action as string,
                newName: this.modalInfo.newName as string | undefined
            }
        },
        methods: {
            submitModal(): void {
                const action = {
                    type: this.action,
                    idList: this.idList,
                    idTodo: this.idTodo,
                    newName: this.newName
                }
                this.$emit('submit-modal', action)
            }
        }
    })
</script>
<style scoped>
	.modal-window {
		border-radius: 10px;
		padding: 16px;
		max-width: 500px;
		background: #fff;
		box-shadow: 0 0 17px 0 #e7e7e7;
		margin: 10% auto;
		z-index: 999;
	}

	.modal_footer {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.modal_content {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	button {
		width: 70px;
		height: 40px;
		border: 1px solid #000;
		border-radius: 5px;
	}

	.modal_footer .submit_btn {
		margin-right: 5px;
		color: #fff;
	}

	.modal_footer .cancel_btn {
		margin-left: 5px;
		color: #fff;
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
</style>