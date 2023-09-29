<script lang="ts">
	import { writable } from "svelte/store";


    // Step
    const a = writable(3); // secret(private key)
    const g = writable(3);
    const q = writable(200);

    const k = writable(2);
    const p = writable(5);
    const h = writable(0);
    const c = writable(3);
    const s = writable(0);

    // mutator
    $: {
        $h = ($g**$k) % $p;
        $s = (($a * $c) + $k) % $q;
    }
</script>

<h1 class="text-lg font-bold">Public Key - Private Key</h1>

<div class="mt-6">
    <div>
        g: <input type="number" bind:value={$g}>
        q: <input type="number" bind:value={$q}>
    </div>
</div>

<div class="flex gap-6 h-screen">
    
    <!-- Alice -->
    <div class="bg-gray-50 rounded-md p-3 shadow-lg border">
        <h2 class="text-lg text-center">Alice</h2>

        <div class="h-10">
            a(secret, private key): <input type="number" bind:value={$a}>
        </div>

        <!-- flows -->
        <div class="mt-6">
            <!-- Step0 -->
            <div class="h-24">
                <div>Step0: Alice calculates Public Key(PK) and send it to Bob</div>
                <div>PK = g^a <span class="text-gray-400"> = {$g}^{$a}</span> = {$g ** $a}</div>
            </div>
            <!-- Step1 -->
            <div class="mt-6 h-24">
                <div>Step1: Alice picks a random k in range 1,...,q({$q})</div>
                k: <input type="number" bind:value={$k} min="0" max={$q}>
            </div>
            <!-- Step2 -->
            <div class="mt-6 h-24">
                <div>Step2: Alice calculates h and send it to Bob</div>
                <div>h = g^k mod p</div>
                <div class="text-gray-400">{$h} = {$g}^{$k} mod {$p}</div>
            </div>
            <!-- Step3 -->
            <div class="mt-6 h-24">
                <div>Step3: Alice recieves c from Bob</div>
                c = {$c}
            </div>
            <!-- Step4 -->
            <div class="mt-6 h-24">
                <div>Step4: Alice calculates s and send it to Bob</div>
                <div>s = ac + k mod q</div>
                <div class="text-gray-400">{$s} = {$a}*{$c} + {$k} mod {$p}</div>
            </div>
            <!-- Step5 -->
            <div class="mt-6 h-24">
                
            </div>
        </div>
    </div>

    <!-- Bob -->
    <div class="bg-gray-50 rounded-md p-3  shadow-lg border">
        <h2 class="text-lg text-center">Bob</h2>

        <div class="h-10"></div>
        
        <!-- flows -->
        <div class="mt-6">
            <!-- Step0 -->
            <div class="h-24">
                <div>Step0: Bob recieves PK from Alice</div>
                <div>PK = {$g**$a}</div>
            </div>
            <!-- Step1 -->
            <div class="mt-6 h-24"></div>
            <!-- Step2 -->
            <div class="mt-6 h-24">
                <div>Step2: Bob recieves h from Alice</div>
                <div>h = {$h}</div>
            </div>
            <!-- Step3 -->
            <div class="mt-6 h-24">
                <div>Step3: Bob picks a random c in range 1,...,q({$q}) and send it to Alice</div>
                c: <input type="number" bind:value={$c} min="0" max={$q}>
            </div>
            <!-- Step4 -->
            <div class="mt-6 h-24">
                
            </div>
            <!-- Step5 -->
            <div class="mt-6 h-24">
               <div>Step 5: Bob checks that</div>
                <div>g^s ≡ (PK)^c * h mod p</div>
                <div class="text-gray-400">g^s ≡ (g^a)^c * h mod p</div>
                <div class="text-gray-400">{$g} ^ {$s} % {$p} ≡ ({$g} ^ {$a}) ^ {$c} * {$h} mod {$p}</div>
                <div class="text-gray-400">{($g**$s)} ≡ {((($g**$a)**$c) * $h)} mod {$p}</div>
                <div class="text-gray-400">{($g**$s) % $p} = {((($g**$a)**$c) * $h) % $p}</div>
            </div>
        </div>

        
    </div>
</div>