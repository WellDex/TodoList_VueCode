<template>
	<form @submit.prevent="onSubmit">
		<input
				type="text"
				:class="{'position_input--todo': !isTodoList, 'position_input--list': isTodoList}"
				v-model="newName"
		>
		<button
				type="submit"
				:class="{'position_btn-access--todo': !isTodoList, 'position_btn-access--list': isTodoList}"
		>
			<i class="material-icons">done</i>
		</button>
		<button
				type="button"
				:class="{'position_btn-cancel--todo': !isTodoList, 'position_btn-cancel--list': isTodoList}"
				@click="$emit('close-form')"
		>
			<i class="material-icons">cancel_presentation</i>
		</button>
	</form>
</template>
<script lang="ts">
    import Vue from "vue"

    export default Vue.extend({
        name: "ChangeNameForm",
        props: {
            newName: String,
            isTodoList: Boolean
        },
        methods: {
            onSubmit(): void {
                this.$emit('change-name', this.newName)
            }
        }
    })
</script>
<style scoped>
	form {
		align-items: center;
	}

	form button {
		margin-top: .5rem;
		background: transparent;
		color: #000;
		border: 1px solid;
		border-radius: 5px;
		width: 35px;
		height: 27px;
	}

	form input {
		padding-left: 10px;
		width: 13.5rem;
		height: 1.8rem;
		border-radius: 5px;
	}

	.position_input--todo {
		position: absolute;
		top: -.5rem;
		left: .5rem;
	}

	.position_input--list {
		position: absolute;
		top: .5rem;
		left: 4.5rem;
	}

	.position_btn-access--todo {
		position: absolute;
		top: -1rem;
		left: 14.3rem;
	}

	.position_btn-access--list {
		position: absolute;
		top: 0.1rem;
		left: 18.5rem;
	}

	.position_btn-cancel--todo {
		position: absolute;
		top: -1rem;
		left: 16.6rem;
	}

	.position_btn-cancel--list {
		position: absolute;
		top: 0.1rem;
		left: 20.8rem;
	}
</style>