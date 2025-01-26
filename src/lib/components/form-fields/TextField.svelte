<script lang="ts">
	import { Schema, string, ValidationError } from 'yup';

	interface Props {
		label: string;
		name: string;
		value: string;
		isRequired?: boolean;
		type?: 'text' | 'textarea' | 'email' | 'tel' | 'url';
		helpText?: string;
		placeholder?: string;
		validators?: Schema[];
	}

	let {
		label,
		name,
		value = $bindable(''),
		type = 'text',
		isRequired = false,
		helpText = '',
		placeholder = '',
		validators = []
	}: Props = $props();

	if (isRequired) {
		validators.push(string().required('This field is required.'));
	}

	if (type === 'email') {
		validators.push(string().email('Please enter a valid email address.'));
	}

	if (type === 'url') {
		validators.push(string().url('Please enter a correct URL.'));
	}

	// STATE
	let unValidatedValue = $state<string>(value);
	let isFocused = $state(false);
	let fieldValidationErrors = $state<string[]>([]);
	/**
	 * - indeterminate - when the field hasn't been touched
	 * - valid - when the entered value is valid
	 * - invalid - when the entered input is invalid
	 */
	let validationStatus = $state<'indeterminate' | 'valid' | 'invalid'>('indeterminate');

	// EVENT HANDLERS
	function handleFocus() {
		isFocused = true;
	}

	function handleBlur() {
		isFocused = false;
	}

	async function handleInput() {
		try {
			for (const validator of validators) {
				await validator.validate(value);
			}

			validationStatus = 'valid';
			fieldValidationErrors = [];
		} catch (err) {
			if (err instanceof ValidationError) {
				validationStatus = 'invalid';
				fieldValidationErrors = [...err.errors];
			}
		}
	}
</script>

<div class="field">
	<label for={name} class="field__label" class:invalid={validationStatus === 'invalid'}
		>{label}</label
	>

	{#if helpText}
		<p class="field__helptext" class:invalid={validationStatus === 'invalid'}>{helpText}</p>
	{/if}

	<div
		class="field__input-container"
		class:invalid={validationStatus === 'invalid'}
		class:focused={isFocused}
	>
		{#if type === 'textarea'}
			<textarea
				{name}
				{placeholder}
				bind:value
				id={name}
				cols="30"
				rows="6"
				onfocus={handleFocus}
				onblur={handleBlur}
				oninput={handleInput}
				class:invalid={validationStatus === 'invalid'}
				class="input"
			></textarea>
		{:else}
			<input
				{type}
				{placeholder}
				bind:value
				onfocus={handleFocus}
				onblur={handleBlur}
				oninput={handleInput}
				class:invalid={validationStatus === 'invalid'}
				class="input"
			/>
		{/if}
		<div class="validation-status-indicator">
			{#if validationStatus === 'indeterminate'}
				<span></span>
			{:else if validationStatus === 'invalid'}
				<span class="invalid">
					<i class="ri-close-circle-fill"></i>
				</span>
			{:else if validationStatus === 'valid'}
				<span class="valid">
					<i class="ri-checkbox-circle-fill"></i>
				</span>
			{/if}
		</div>
	</div>

	<ul class="field__errors">
		{#each fieldValidationErrors as validationError}
			<li>{validationError}</li>
		{/each}
	</ul>
</div>
