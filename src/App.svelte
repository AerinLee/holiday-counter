<script>
	import {onMount} from 'svelte';
	import DDay from './components/DDay.svelte'
	import {holidays} from './constants/holidays'
	import Space from './components/Space.svelte'
	import Bulb from './components/Bulbs.svelte'
	import Tree from './components/Tree.svelte'
	import makeItSnow from './components/Snow.js'

	let nextHoliday;
	let dateString;
	
	onMount(()=> {
		getNextHoliday();
	})

	function getNextHoliday() {
		let today = new Date();

		let year = today.getFullYear();
		let month = ('0' + (today.getMonth() + 1)).slice(-2);
		let day = ('0' + today.getDate()).slice(-2);

		dateString = year + '-' + month  + '-' + day;

		for (let holiday in holidays){
			nextHoliday = holiday
			if(dateString < holiday ){
				break;
			}
		} 
	}

</script>


<main use:makeItSnow>

	<Bulb></Bulb>
	<Space h_value=1></Space>
	<h1>빨간 날 카운터</h1>
	<p class="message" >오늘은 {dateString} 입니다.</p>
	{#if nextHoliday}
		<p class="message" >다음 빨간 날은 <red>{holidays[nextHoliday]['name']}</red> 입니다.</p>
		<Space h_value=2></Space>
		<DDay value='{nextHoliday}'></DDay> 
		<Space h_value=2></Space>
		<p class="message" >남았습니다.</p>
		<Space h_value=2></Space>
		{#if holidays[nextHoliday]['isWeekend']}
			<p class="message" >하지만 그날은 주말이군요😥</p>
			{#if holidays[nextHoliday]['substitution']}
				<p class="message" >우리에겐 대체휴일이 있어요!🤩</p>
			{:else}
				<p class="message" >대체휴일도 없네요...😂</p>
			{/if}
		{/if}
	{/if}
	<div style="margin-top: 10rem">
		<Tree></Tree>
	</div>
</main>


<style>
	
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: auto;
		height: inherit;
	}

	h1 {
		color: #d11a1abb;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}

	.message {
		font-size: 1.6rem;
		margin: 0;
	}

	red {
		color: red;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>