<script>
	import {onMount} from 'svelte';
	import DDay from './components/DDay.svelte'
	import {holidays} from './constants/holidays'
	import Space from './components/Space.svelte'
	import Bulb from './components/Bulbs.svelte'
	import Tree from './components/Tree.svelte'
	import makeItSnow from './components/Snow.js'
	import Btn from './components/Button.svelte'

	let nextHolidayIdx;
	let nextHoliday;
	let dateString;
	let hasPrev = false;
	let hasNext = false;

	$: nextHoliday = nextHolidayIdx >= 0 ? holidays[nextHolidayIdx] : null;
	$: {
		if(nextHoliday){
			let m = nextHoliday['date'].split('-')[1]
			if(m == '01' || m == '02' || m == '12'){
				document.body.style.backgroundColor = '#95caffab'
			} else if (m == '03' || m == '04' || m == '05'){
				document.body.style.backgroundColor = '#94e572ab'
			}else if (m == '06' || m == '07' || m == '08'){
				document.body.style.backgroundColor = '#3197ffab'
			}else if (m == '09' || m == '10' || m == '11'){
				document.body.style.backgroundColor = '#ed861bab'
			}
		}
	}
	$: hasPrev = nextHolidayIdx > 0 ? false: true;
	$: hasNext = nextHolidayIdx < holidays.length - 1 ? false: true;
	
	onMount(()=> {
		getNextHoliday();
	})

	function getNextHoliday() {
		let today = new Date();

		let year = today.getFullYear();
		let month = ('0' + (today.getMonth() + 1)).slice(-2);
		let day = ('0' + today.getDate()).slice(-2);

		dateString = year + '-' + month  + '-' + day;
		for (let i = 0; i < holidays.length; i++){
			nextHolidayIdx = i
			nextHoliday = holidays[nextHolidayIdx]
			if(dateString < holidays[i]['date'] ){
				break;
			}
		} 
	}

	function goPrevHoliday(){
		nextHolidayIdx--;
	}

	function goNextHoliday(){
		nextHolidayIdx++;
	}


</script>


<main use:makeItSnow>
	{#if nextHoliday && nextHoliday['name'] == '크리스마스🎄'}
	<Bulb></Bulb>
	{/if}

	<Space h_value=1></Space>
	<h1>빨간 날 카운터</h1>
	<div class="message" >오늘은 {dateString} 입니다.</div>
	
	{#if nextHoliday}
	<div class="content-wrap">
		<Btn disabled={hasPrev} value='◁' event={goPrevHoliday}></Btn>
		<div class="count-wrap">
			<p class="message" >다음 빨간 날은 <red>{nextHoliday['name']}</red> ({nextHoliday['date']}) 입니다.</p>

			<DDay value='{nextHoliday['date']}'></DDay> 

			<p class="message" >남았습니다.</p>
			<Space h_value=2></Space>

			{#if nextHoliday['isWeekend']}
				<p class="message" >하지만 그날은 주말이군요😥</p>
				{#if nextHoliday['substitution']}
					<p class="message" >우리에겐 대체휴일이 있어요!🤩</p>
				{:else}
					<p class="message" >대체휴일도 없네요...😂</p>
				{/if}
			{/if}
		</div>
		<Btn disabled={hasNext} value='▷' event={goNextHoliday}></Btn>
	</div>
	{/if}

	{#if nextHoliday && nextHoliday['name'] == '크리스마스🎄'}
	<div style="margin-top: 10rem">
		<Tree></Tree>
	</div>
	{/if}
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

	.count-wrap {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.content-wrap {
		display: flex;
		align-items: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>