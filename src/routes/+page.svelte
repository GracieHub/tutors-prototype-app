<script>
    import { initFirebase } from '$lib/firebase-utils';
    import { child, get, getDatabase, onValue, ref, off } from 'firebase/database';
    import { onMount } from 'svelte';
    import { getKeys } from '../environment';
    import StudentCard from '$lib/StudentCard.svelte';
    /**
     * @type {any[]}
     */
    let courseEvents = [];
    // A Map of all students seen so far (name) ==> Student Data)
    let studentRecords = new Map();
    onMount(async () => {
        initFirebase(getKeys().firebase);
        let statusRef = ref(getDatabase(), 'StudentInfo');
        onValue(statusRef, async (snapshot) => {
            if (snapshot) {
                // a new event has been seen
                const studentEvent = snapshot.val();
                console.log('student info received');
                console.log(studentEvent);
                // Have we seen this student before?
                const studentRecord = studentRecords.get(studentEvent.name);
                if (!studentRecord) {
                    // Nope, this is a new student. Lets store the student in our map
                    studentRecords.set(studentEvent.name, studentEvent);
                    // .. and lets push onto the aray of live student cards we are displaying
                    courseEvents.push(studentEvent);
                } else {
                    // we have already seen this student, so lets update the course info...
                    studentRecord.course = studentEvent.course;
                    // .. and lets also update the topic
                    studentRecord.topic = studentEvent.topic;
                }
                // touch all sutdent records, so that if any have changed they will be refreshed in screen
                courseEvents = [...courseEvents];
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
            <StudentCard {courseEvent} />
        {/each}
    </section>
</div>