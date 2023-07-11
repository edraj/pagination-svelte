<script lang="ts">
	import Pagination from '$lib';
	let page = 1;
</script>

<p>THE SELECTED PAGE IS: {page}</p>

<Pagination
	propNumberOfPages={100}
	maxPageDisplay={5}
	bind:propActivePage={page}
	on:eventChange={(parEvent) => console.log(parEvent.detail.numberActivePage)}
/>
