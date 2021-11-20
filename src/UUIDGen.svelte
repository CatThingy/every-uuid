<script lang="ts">
    // Document variables
    let documentHeight : number = Math.max(800, document.documentElement.scrollHeight);
    let initialHeight : number;
    let currentScroll : number;

    let currentID : bigint = 0n;

    let list : Element;

    function uuidFromBigInt (n : bigint) : string {
        let padded_hex : string = "00000000000000000000000000000000";
        padded_hex = (padded_hex + n.toString(16)).slice(-32);

        return padded_hex.substring(0, 8) + "-" 
             + padded_hex.substring(8, 12) + "-"
             + padded_hex.substring(12, 16) + "-"
             + padded_hex.substring(16, 20) + "-"
             + padded_hex.substring(20, 32)
    }

    function scrollHandler () : void {
        while (documentHeight <= currentScroll + initialHeight) {
            let el : Element = document.createElement("li");
            el.textContent = uuidFromBigInt(currentID);
            list.appendChild(el);
            currentID += 1n;
            documentHeight = document.documentElement.scrollHeight;
        }
    }

    function loadHandler () : void {
        list = document.querySelector("#uuids")
        while (document.documentElement.scrollHeight <= documentHeight * 5) {
            let el : Element = document.createElement("li");
            el.textContent = uuidFromBigInt(currentID);
            list.appendChild(el);
            currentID += 1n;
        }
        initialHeight = document.documentElement.scrollHeight;
    }
</script>

<svelte:window bind:scrollY={currentScroll} on:scroll={scrollHandler} on:load={loadHandler}/>

<ul id="uuids" class="text-center text-2xl p-10 font-mono dark:text-white"/>

<style lang="postcss" global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>
