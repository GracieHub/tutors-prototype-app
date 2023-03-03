<script>
    import { initFirebase } from '$lib/firebase-utils';
    import { child, get, getDatabase, onValue, ref, off } from 'firebase/database';
    import { onMount } from 'svelte';
    import { getKeys } from '../environment';
    import Card from '$lib/StudentCard.svelte';
    /**
     * @type {any[]}
     */
    let courseEvents = [];
    onMount(async () => {
        initFirebase(getKeys().firebase);
        let statusRef = ref(getDatabase(), 'StudentInfo');
        onValue(statusRef, async (snapshot) => {
            console.log('student info received');
            if (snapshot) {
                const data = snapshot.val();
                courseEvents.push(data);
                courseEvents = [...courseEvents];
                console.log(data);
            }
        });
    });
</script>
<div class="container mx-auto p-8 space-y-8">
    <h1>Tutors Live Prototype</h1>
    <p>Students Info</p>
    <section class="flex space-x-2">
        <div class="flex justify-center" />
        {#each courseEvents as courseEvent}
            <Card {courseEvent} />
        {/each}
    </section>
</div>