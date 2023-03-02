<script>
    import { initFirebase } from '$lib/firebase-utils';
    import { child, get, getDatabase, onValue, ref, off } from 'firebase/database';
    import { onMount } from 'svelte';
    import { getKeys } from '../environment';
	import { Avatar } from "@skeletonlabs/skeleton";
	import FaPhone from "svelte-icons/fa/FaPhone.svelte";
	import FaFacebookMessenger from "svelte-icons/fa/FaFacebookMessenger.svelte";
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
				<div class="card w-60 h-[24rem] overflow-x-hidden m-1 border-y-8 border-primary-500">
					<div class="flex">
						<header class="card-header inline-flex items-center">
							<div class="inline-flex w-full">
								<Avatar src="/student1.jpg" alt={courseEvent.name} class="mr-2" />
								<h6>
									{courseEvent.name}
									<div>
									<button class="btn variant-filled-secondary btn-base ring-2 ring-primary-500 ring-inset text-filled-500">
										<div class="icon" style="color: orange; width: 20px; height:16px">
											<FaFacebookMessenger />
										</div>
									</button>
									<button class="btn variant-filled-secondary btn-base ring-2 ring-primary-500 ring-inset text-filled-500">
										<div class="icon" style="color: green; width: 20px; height:16px">
											<FaPhone />
										</div>
									</button>
								</div>
								</h6>
							</div>
						</header>
					</div>
					<figure class="flex justify-center object-scale-down mt-4 p-1 h-40">
						<img src="/topic.jpg" alt="topic Image here" />
					</figure>
					<div class="card-body text-center">
						Studying Lab: {@html courseEvent.course}
					</div>
					<footer class="card-footer text-center">
						<p class="mt-2 italic">
							{@html courseEvent.topic}
						</p>
					</footer>
				</div>
            {/each}
    </section>
</div>