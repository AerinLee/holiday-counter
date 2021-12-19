<script>
    import {onMount} from 'svelte'

    function flipTo(digit, n){
        let current = digit.getAttribute('data-num');
        digit.getAttribute('data-num', n);
        digit.querySelector('.front').getAttribute('data-content', current);
        digit.querySelector('.back, .under').getAttribute('data-content', n);
        digit.querySelector('.flap').style.display = 'block'
        setTimeout(function(){
            digit.querySelector('.base').textContent = n;
            digit.querySelector('.flap').style.display = 'none'
        }, 350);
    }

    function jumpTo(digit, n){
    digit.getAttribute('data-num', n);
    digit.querySelectorAll('.base').textContent = n;
    }

    function updateGroup(group, n, flip){
        let digit1 = document.getElementsByClassName('ten'+group)[0];
        let digit2 = document.getElementsByClassName(group)[0];
        n = String(n);
        if(n.length == 1) n = '0'+n;
        let num1 = n.substr(0, 1);
        let num2 = n.substr(1, 1);
        if(digit1.getAttribute('data-num') != num1){
            if(flip) flipTo(digit1, num1);
            else jumpTo(digit1, num1);
        }
        if(digit2.getAttribute('data-num') != num2){
            if(flip) flipTo(digit2, num2);
            else jumpTo(digit2, num2);
        }
    }

    function setTime(flip){
        var t = new Date();
        updateGroup('hour', t.getHours(), flip);
        updateGroup('min', t.getMinutes(), flip);
        updateGroup('sec', t.getSeconds(), flip);
        }

    onMount(()=> {
        setTime(false);
            setInterval(function(){
                setTime(true);
        }, 1000);
    })


</script>




<div class="clock">

  <div class="digit tenhour">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>

  <div class="digit hour">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>
  
  <div class="digit tenmin">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>

  <div class="digit min">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>
  
  <div class="digit tensec">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>

  <div class="digit sec">
    <span class="base"></span>
    <div class="flap over front"></div>
    <div class="flap over back"></div>
    <div class="flap under"></div>
  </div>
  
</div>


<style lang="scss">

    $flipColour: #fff;
    $flipColourDark: darken($flipColour, 35%);
    $textColour: #333;
    $textColourDark: darken($textColour, 35%);

    html {
    height: 100%;
    }

    body {
    height: 100%;
    background: #85D8CE;
    background: linear-gradient(135deg, #085078, #85D8CE);
    }

    .digit {
    position: relative;
    float: left;
    width: 10vw;
    height: 15vw;
    background-color: $flipColour;
    border-radius: 1vw;
    text-align: center;
    font-family: Oswald, sans-serif;
    font-size: 11vw;
    }

    .base {
    display: block;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: $textColour;
    }
    
    .flap {
    display: none;
    position: absolute;
    width: 100%;
    height: 50%;
    background-color: $flipColour;
    left: 0;
    top: 0;
    border-radius: 1vw 1vw 0 0;
    transform-origin: 50% 100%;
    backface-visibility: hidden;
    overflow: hidden;
    
    &::before {
        content: attr(data-content);
        position: absolute;
        left: 50%;
    }
    
    &.front::before,
    &.under::before {
        top: 100%;
        transform: translate(-50%, -50%);
    }
    
    &.back {
        transform: rotateY(180deg);
        &::before {
        top: 100%;
        transform:  translate(-50%, -50%) rotateZ(180deg);
        }
    }

    &.over {
        z-index: 2;
    }
    
    &.under {
        z-index: 1;
    }
    
    &.front {
        animation: flip-down-front 300ms ease-in both;
    }
    
    &.back {
        animation: flip-down-back 300ms ease-in both;
    }
    
    &.under {
        animation: fade-under 300ms ease-in both;
    }
    
    }

    @keyframes flip-down-front {
    0% {
        transform: rotateX(0deg);
        background-color: $flipColour;
        color: $textColour;
    }
    100% {
        transform: rotateX(-180deg);
        background-color: $flipColourDark;
        color: $textColourDark;
    }
    }

    @keyframes flip-down-back {
    0% {
        transform: rotateY(180deg) rotateX(0deg);
        background-color: $flipColourDark;
        color: $textColourDark;
    }
    100% {
        transform: rotateY(180deg) rotateX(180deg);
        background-color: $flipColour;
        color: $textColour;
    }
    }

    @keyframes fade-under {
    0% {
        background-color: $flipColourDark;
        color: $textColourDark;
    }
    100% {
        background-color: $flipColour;
        color: $textColour;
    }
    }

    .clock {
    position: absolute;
    width: 70vw;
    top: 50%;
    left: 15vw;
    transform: translateY(-50%);
    perspective: 100vw;
    perspective-origin: 50% 50%;
    
    .digit {
        margin-right: 1vw;
        &:nth-child(2n+2) { margin-right: 3.5vw; }
        &:last-child { margin-right: 0; }
    }
    
    }  
</style>