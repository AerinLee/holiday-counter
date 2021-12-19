<script>
import { onMount } from "svelte";
import Number from 'svelte-digital-alarm-number'

export let value;

let day;
let hours;
let minutes;
let seconds;


let innerText;
const getDDay = () => {
	const setDate = new Date(value+"T00:00:00+0900");
	const setDateYear = setDate.getFullYear();
	const setDateMonth = setDate.getMonth() + 1;
	const setDateDay = setDate.getDate();

	let now = new Date();

	const distance = setDate.getTime() - now.getTime();
	
	day = Math.floor(distance/(1000*60*60*24));
	hours = Math.floor((distance % (1000*60*60*24))/(1000*60*60));
	minutes = Math.floor((distance % (1000*60*60))/(1000*60));
	seconds = Math.floor((distance % (1000*60))/1000);

}

const init = () => {
    getDDay();
    setInterval(getDDay, 1000);
}

    onMount(() => {
        init();
    })
</script>

<div class="dday-wrap">
    <Number min=2 value='{day}'></Number> <span>일</span>
    <Number min=2 value='{hours}'></Number> <span>시</span><span>간</span>
    <Number min=2 value='{minutes}'></Number> <span>분</span>
    <Number min=2 value='{seconds}'></Number> <span>초</span>
</div>

<style>
    .dday-wrap {
        display : inline-flex;
        width : 30%;
        font-size : 1.6rem;
        align-items: flex-end;
        margin: 2rem 0;
    }
</style>