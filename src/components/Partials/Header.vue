<script>
export default {
    name: 'Header',
}
</script>

<template>
	<div class="d-flex">
		<img class="me-auto" src="../../assets/img/logo-boolflix.png" alt="logo netflix" />
		<input
			placeholder="Cosa vuoi guardare?"
			class="me-5 form-control w-25"
			type="text"
			name="search"
			id="searchInput"
		/>
		<select
			aria-label="example"
			class="form-select w-25 me-3"
			id="genreSelect"
			name="genreSelect"
		>
			<option>Tutti i generi</option>
			<option>Horror</option>
		</select>
	</div>
</template>

<style lang="scss" scoped>
	
</style>