<script lang="ts">
	import { page } from '$app/stores';
	import FaFacebookMessenger from 'svelte-icons/fa/FaFacebookMessenger.svelte';
    import FaPhone from 'svelte-icons/fa/FaPhone.svelte';

    export let courseEvent: any;

</script>


<a href="{courseEvent.topichuddle}" target="_blank">
    <button
        class="btn variant-filled-secondary btn-base ring-2 ring-primary-500 ring-inset text-filled-500"
    >
        <div class="icon" style="color: green; width: 20px; height:16px">
            <FaPhone />
        </div>
    </button>
    </a>
