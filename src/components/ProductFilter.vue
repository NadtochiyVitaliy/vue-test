<template>
	<div class="filter-wrap">
		<button class="btn" @click="$emit('emitFilter', currentFilter)">Filter</button>
		<div class="form-group sort-field">
			<select v-model="currentFilter">
				<option v-for="(filter, index) of filters" :key="index" :value="filter.value">{{ filter.name }}</option>
			</select>
		</div>
	</div>
</template>

<script>
export default {
	name: 'ProductFilter',
	data: () => ({
		currentFilter: 1,
		filters: [
			{ name: 'All', value: 1 },
			{ name: 'Price', value: 2 },
			{ name: 'Rating', value: 3 },
		]
	})
}
</script>

<style lang="scss">
.filter-wrap {
	display: flex;
	align-self: start;

	@media only screen and (min-width : 768px) {
		flex-grow: 1;
		max-width: 500px;
		margin: 0 0 20px auto;
	}
}

.filter-form {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.btn {
	font-size: 14px;
	line-height: 1;
	padding: 10px 16px;
	background-color: #fa3621;
	color: #fff;
	text-align: center;
	vertical-align: top;
	border: none;
	cursor: pointer;
	transition: all .2s;
	min-width: 150px;
	margin-right: 15px;

	&:hover {
		background-color: rgba(#fa3621, .7);
	}

}

.form-group {
	width: 100%;
}

select {
	-webkit-appearance: none;
	-webkit-border-radius: 0;
	box-sizing: border-box;
	display: block;
	width: 100%;
	border: none;
	border-bottom: 1px solid #000;
	background: transparent;
	padding: 10px 15px;
	font-size: 14px;
	line-height: 1;
	color: #000;

	&:focus {
		border-color: rgba(#000, .7);
		outline: none;
	}

	&::placeholder {
		color: #000;
	}
}

.sort-field {
	position: relative;

	&::before {
		content: '';
		width: 8px;
		height: 8px;
		border-right: 1px solid #000;
		border-bottom: 1px solid #000;
		transform: rotate(45deg) translateY(-50%);
		position: absolute;
		top: 50%;
		right: 10px;
	}

	select {
		padding-right: 30px;
	}
}
</style>