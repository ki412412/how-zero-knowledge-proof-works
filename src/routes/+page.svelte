<script lang="ts">
	import { writable } from "svelte/store";

    const a = writable(3); // secret(private key)
    const g = writable(3);
    const q = writable(200);

    const k = writable(2);
    const p = writable(5);
    const h = writable(0);
    const c = writable(3);
    const s = writable(0);

    $: {
        $h = ($g**$k) % $p;
        $s = (($a * $c) + $k) % $q;
    }
</script>

<h1 class="text-xl font-bold">How Zero-Kowldge Proof Works</h1>

<div class="mt-6">
    <div>
        g: <input type="number" bind:value={$g}>
        q: <input type="number" bind:value={$q}>
    </div>
</div>

<div class="flex gap-6 h-max items-stretch">
    
    <!-- Alice -->
    <div class="bg-gray-50 rounded-md p-6 shadow-lg border min-w-[30rem]">
        <h2 class="text-lg text-center">Alice</h2>

        <div class="h-10 mt-3">
            a(secret, private key): <input type="number" bind:value={$a}>
        </div>

        <!-- flows -->
        <div class="mt-6">
            <!-- Step 0 -->
            <div class="h-24">
                <div class="font-semibold">Step 0: Alice calculates Public Key(PK) and send it to Bob</div>
                <div class="mt-3 pl-6">PK = g^a <span class="text-gray-400"> = {$g}^{$a}</span> = {$g ** $a}</div>
            </div>
            <!-- Step 1 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 1: Alice picks a random k in range 1,...,q({$q})</div>
                <div class="mt-3 pl-6">
                    k: <input type="number" bind:value={$k} min="0" max={$q}>
                </div>
            </div>
            <!-- Step 2 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 2: Alice calculates h and send it to Bob</div>
                <div class="mt-3 pl-6">
                    <div>h = g^k mod p</div>
                    <div class="text-gray-400">{$h} = {$g}^{$k} mod {$p}</div>
                </div>
            </div>
            <!-- Step 3 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 3: Alice recieves c from Bob</div>
                <div class="mt-3 pl-6">
                    c = {$c}
                </div>
            </div>
            <!-- Step 4 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 4: Alice calculates s and send it to Bob</div>
                <div class="mt-3 pl-6">
                    <div>s = ac + k mod q</div>
                    <div class="text-gray-400">{$s} = {$a}*{$c} + {$k} mod {$p}</div>
                </div>
            </div>
            <!-- Step 5 -->
            <div class="mt-6 h-">
                
            </div>
        </div>
    </div>

    <!-- Bob -->
    <div class="bg-gray-50 rounded-md p-6 shadow-lg border min-w-[30rem] h-full">
        <h2 class="text-lg text-center">Bob</h2>

        <div class="h-10 mt-3"></div>
        
        <!-- flows -->
        <div class="mt-6">
            <!-- Step 0 -->
            <div class="h-24">
                <div class="font-semibold">Step 0: Bob recieves PK from Alice</div>
                <div class="mt-3 pl-6">PK = {$g**$a}</div>
            </div>
            <!-- Step 1 -->
            <div class="mt-6 h-24"></div>
            <!-- Step 2 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 2: Bob recieves h from Alice</div>
                <div class="mt-3 pl-6">h = {$h}</div>
            </div>
            <!-- Step 3 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 3: Bob picks a random c in range 1,...,q({$q}) and send it to Alice</div>
                <div class="mt-3 pl-6">
                    c: <input type="number" bind:value={$c} min="0" max={$q}>
                </div>
            </div>
            <!-- Step 4 -->
            <div class="mt-6 h-24">
                <div class="font-semibold">Step 4: Bob recieves s from Alice</div>
                <div class="mt-3 pl-6">s = {$s}</div>
            </div>
            <!-- Step 5 -->
            <div class="mt-6 h-30">
                <div class="font-semibold">Step  5: Bob checks that</div>
                <div class="mt-3 pl-6">
                    <div>g^s ≡ (PK)^c * h mod p</div>
                    <div class="text-gray-400">g^s ≡ (g^a)^c * h mod p</div>
                    <div class="text-gray-400">{$g}^{$s}≡({$g}^{$a})^{$c} * {$h} mod {$p}</div>
                    <div class="text-gray-400">{($g**$s)} ≡ {((($g**$a)**$c) * $h)} mod {$p}</div>
                    <div class="text-gray-400">{($g**$s) % $p} = {((($g**$a)**$c) * $h) % $p}</div>
                </div>
            </div>
        </div>

        
    </div>
</div>