<script>
    export let benefit;
    export let index;
    export let isVisible;
    export let ref;
</script>

<div
    bind:this={ref}
    class="flex gap-6 sm:gap-8 
        {benefit.name === 'Full-Stack Capabilities' ? 'slide-in-right' : 'slide-in'} 
        {isVisible ? 'visible' : ''}"
    style="transition-delay: {index * 150}ms"
>
    <p
        class="poppins text-4xl sm:text-5xl md:text-6xl text-slate-500 font-semibold"
    >
        0{index + 1}
    </p>
    <div class="flex flex-col gap-6 sm:gap-8">
        <h3 class="text-2xl sm:text-3xl md:text-5xl">
            {benefit.name}
        </h3>
        <p>{benefit.description}</p>
    </div>
</div>