<script>
    import { initFirebase } from '$lib/firebase-utils';
    import { child, get, getDatabase, onValue, ref, off } from 'firebase/database';
    import { onMount } from 'svelte';
    import { getKeys } from '../environment';
  
	onMount(async () => {
        initFirebase(getKeys().firebase);
        let statusRef = ref(getDatabase(), 'StudentInfo');
        onValue(statusRef, async () => {
            console.log('student info received');
        });
    });
</script>
<div class="container mx-auto p-8 space-y-8">
    <h1>Tutors Live Prototype</h1>
    <p>Students Info</p>
    <section class="flex space-x-2">
        <div class="flex justify-center" />
    </section>
</div>
