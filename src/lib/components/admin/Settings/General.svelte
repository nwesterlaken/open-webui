<script lang="ts">
	import {
		getDefaultUserRole,
		getSignUpEnabledStatus,
		toggleSignUpEnabledStatus,
		updateDefaultUserRole
	} from '$lib/apis/auths';
	import { onMount } from 'svelte';

	export let saveHandler: Function;
	let signUpEnabled = true;
	let defaultUserRole = 'pending';

	const toggleSignUpEnabled = async () => {
		signUpEnabled = await toggleSignUpEnabledStatus(localStorage.token);
	};

	const updateDefaultUserRoleHandler = async (role) => {
		defaultUserRole = await updateDefaultUserRole(localStorage.token, role);
	};

	onMount(async () => {
		signUpEnabled = await getSignUpEnabledStatus(localStorage.token);
		defaultUserRole = await getDefaultUserRole(localStorage.token);
	});
</script>

<form
	class="flex flex-col h-full justify-between space-y-3 text-sm"
	on:submit|preventDefault={() => {
		// console.log('submit');
		saveHandler();
	}}
>
	<div class=" space-y-3 pr-1.5 overflow-y-scroll max-h-80">
		<div>
			<div class=" mb-2 text-sm font-medium">General Settings</div>

			<div class="  flex w-full justify-between">
				<div class=" self-center text-xs font-medium">Enable New Sign Ups</div>

				<button
					class="p-1 px-3 text-xs flex rounded transition"
					on:click={() => {
						toggleSignUpEnabled();
					}}
					type="button"
				>
					{#if signUpEnabled}
						<svg
							xmlns="http://www.w3.org/2000/svg"
							viewBox="0 0 16 16"
							fill="currentColor"
							class="w-4 h-4"
						>
							<path
								d="M11.5 1A3.5 3.5 0 0 0 8 4.5V7H2.5A1.5 1.5 0 0 0 1 8.5v5A1.5 1.5 0 0 0 2.5 15h7a1.5 1.5 0 0 0 1.5-1.5v-5A1.5 1.5 0 0 0 9.5 7V4.5a2 2 0 1 1 4 0v1.75a.75.75 0 0 0 1.5 0V4.5A3.5 3.5 0 0 0 11.5 1Z"
							/>
						</svg>
						<span class="ml-2 self-center">Enabled</span>
					{:else}
						<svg
							xmlns="http://www.w3.org/2000/svg"
							viewBox="0 0 16 16"
							fill="currentColor"
							class="w-4 h-4"
						>
							<path
								fill-rule="evenodd"
								d="M8 1a3.5 3.5 0 0 0-3.5 3.5V7A1.5 1.5 0 0 0 3 8.5v5A1.5 1.5 0 0 0 4.5 15h7a1.5 1.5 0 0 0 1.5-1.5v-5A1.5 1.5 0 0 0 11.5 7V4.5A3.5 3.5 0 0 0 8 1Zm2 6V4.5a2 2 0 1 0-4 0V7h4Z"
								clip-rule="evenodd"
							/>
						</svg>

						<span class="ml-2 self-center">Disabled</span>
					{/if}
				</button>
			</div>

			<div class=" flex w-full justify-between">
				<div class=" self-center text-xs font-medium">Default User Role</div>
				<div class="flex items-center relative">
					<select
						class="w-fit pr-8 rounded py-2 px-2 text-xs bg-transparent outline-none text-right"
						bind:value={defaultUserRole}
						placeholder="Select a theme"
						on:change={(e) => {
							updateDefaultUserRoleHandler(e.target.value);
						}}
					>
						<option value="pending">Pending</option>
						<option value="user">User</option>
						<option value="admin">Admin</option>
					</select>
				</div>
			</div>
		</div>
	</div>

	<div class="flex justify-end pt-3 text-sm font-medium">
		<button
			class=" px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-gray-100 transition rounded"
			type="submit"
		>
			Save
		</button>
	</div>
</form>
