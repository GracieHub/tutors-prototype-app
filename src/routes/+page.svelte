<script>
    import { initFirebase } from '$lib/firebase-utils';
    import { child, get, getDatabase, onValue, ref, off } from 'firebase/database';
    import { onMount } from 'svelte';
    import { getKeys } from '../environment';
    import Card from '$lib/StudentCard.svelte';
    import FaPhone from 'svelte-icons/fa/FaPhone.svelte';
    import TopicHuddleCard from '$lib/TopicHuddleCard.svelte';

    /**
     * @type {any[]}
     */
    let courseEvents = [];
    let reload = false;
    // A Map of all students seen so far student name) ==> StudentEvent)
    let studentRecords = new Map();
    // A Map of (topic name) => (a map of (student Name) => StudentEvent)
    let topics = new Map();
    onMount(async () => {
        initFirebase(getKeys().firebase);
        let statusRef = ref(getDatabase(), 'StudentInfo');
        onValue(statusRef, async (snapshot) => {
            if (snapshot) {
                // a new event has been seen
                const studentEvent = snapshot.val();
                console.log('student info received');
                console.log(studentEvent);

                topics.forEach((studentMap, topicName) => {
                    studentMap.delete(studentEvent.name);

                });
                // Now, see if we have seen topic before
                const studentMapForTopic = topics.get(studentEvent.topic);

                if (!studentMapForTopic) {
                    // Nope, this is a new topic. Lets create a new studentMap for this topic
                    const studentMap = new Map();
                    //  put the student into this map
                    studentMap.set(studentEvent.name, studentEvent);

                    // put the map into the topics mao
                    topics.set(studentEvent.topic, studentMap);
                } else {
                    // existiing topic, put the student in
                    studentMapForTopic.set(studentEvent.name, studentEvent);

                    // if the student was already in aother topic, we have removed it (see earlier)
                }

                // we have changed the topic map, reload this part of the page
                reload = !reload;
                // touch all sutdent records, so that if any have changed they will be refreshed in screen
                courseEvents = [...courseEvents];
            }
        });
    });
</script>
<svelte:head>
  <title>Tutors Hive</title>
</svelte:head>
<div class="sticky top-0 z-40 mb-5">
</div>
<div class="container mx-auto p-8 space-y-8">
    <h1>Tutors Hive</h1>
    {#key reload}
        <section class=" space-x-2">
            {#each [...topics] as [topicName, topicMap]}  
                <hr />
                <h3 class="p-2">Topic: {topicName} 
                    <TopicHuddleCard {topicMap} />
                </h3> 
                <hr />
                <h5 class="p-2">Students</h5>
                <div class="flex justify-center">  
                    {#each [...topicMap] as [studentName, studentRecord]}
                        <Card courseEvent={studentRecord} />
                    {/each}
                </div>
            {/each}
        </section>
    {/key}
</div>