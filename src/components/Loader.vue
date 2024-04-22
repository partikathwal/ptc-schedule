<template>
    <div class="absolute top-0 left-0 w-screen h-screen bg-black bg-opacity-25 grid place-items-center">
		<div id="loader" class="w-24 h-24 border-8 border-white rounded-full"></div>
	</div>
</template>

<script setup></script>

<style>
#loader {
	border-top-color: green;
	animation-name: spin;
	animation-duration: 750ms;
	animation-iteration-count: infinite;
	animation-timing-function: linear; 
}

@keyframes spin {
    from {
        transform:rotate(0deg);
    }
    to {
        transform:rotate(360deg);
    }
}
</style>