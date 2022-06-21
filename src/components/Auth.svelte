<script lang="ts">
	import { supabase } from '$lib/supabaseClient';

	let loading = false;
	let email: string;
	let password: string;
	let firstName: string;
	let lastName: string;

	const handleSignUp = async () => {
		try {
			loading = true;
			const { user, error } = await supabase.auth.signIn({ provider: 'google' });
			if (error) throw error;
			console.log('user', user);
		} catch (error) {
			alert(error.error_description || error.message);
		} finally {
			loading = false;
		}
	};
</script>

<form class="row flex flex-center" on:submit|preventDefault={handleSignUp}>
	<div class="col-6 form-widget">
		<h1 class="header">Supabase + Svelte</h1>
		<p class="description">Sign in via magic link with your email below</p>
		<!-- <div>
			<input class="inputField" type="email" placeholder="Your email" bind:value={email} />
			<input class="inputField" type="password" placeholder="Your password" bind:value={password} />
			<input class="inputField" type="text" placeholder="First Name" bind:value={firstName} />
			<input class="inputField" type="text" placeholder="Last Name" bind:value={lastName} />
		</div> -->
		<div>
			<input
				type="submit"
				class="button block"
				value={loading ? 'Loading' : 'Sign in with Google'}
				disabled={loading}
			/>
		</div>
	</div>
</form>
