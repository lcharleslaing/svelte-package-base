# svelte-auto-clock

A simple Analog and Digital Clock display with Date.  This was built with tailwindcss and may update it to standard css in the future.

## Install

```bash
pnpm i svelte-auto-clock
```

```bash
yarn svelte-auto-clock
```

```bash
npm i svelte-auto-clock
```


## Use

```js
<script>
    import Clock from "svelte-auto-clock";
</script>


<div class="absolute left-2 bottom-2">
    <Clock 
        width={150} 
        card={true} 
        cardColor="bg-blue-100" 
        textSize="16" />
</div>
```


