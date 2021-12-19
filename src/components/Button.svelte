<script>
    export let value;
    export let disabled;
    export let event;
</script>

<button class="btn" id="sidebarToggle" {disabled} on:click="{event}">
    {value}
</button>

<style>
.btn {
    display: inline-block;
    font-weight: bolder;
    color: #172abb;
    text-align: center;
    vertical-align: middle;
    border: 1px solid transparent;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    border-radius: 50%;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

@media (prefers-reduced-motion: reduce) {
    .btn {
        transition: none;
    }
}

.btn:hover {
color: #858796;
    text-decoration: none;
}

.btn:hover {
outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(78, 115, 223, 0.25);
}

.btn:disabled {
    opacity: 0.65;
}

.btn:not(:disabled):not(.disabled) {
    cursor: pointer;
}

.btn:disabled {
    pointer-events: none;
}

#sidebarToggle {
  width: 2.5rem;
  height: 2.5rem;
  text-align: center;
  margin-bottom: 1rem;
  cursor: pointer;
  background-color: #eaecf4;
}

</style>