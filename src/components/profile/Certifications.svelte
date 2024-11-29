<script>
  import { currentUser } from '../../stores/users';
  import { format } from 'date-fns';

  export let editMode = false;
  export let loading = false;
  export let onSubmit;

  let formData = {
    certifications: {
      globalGap: { number: '', validUntil: '', status: 'valid' },
      grasp: { number: '', validUntil: '', status: 'valid' },
      eco: { number: '', validUntil: '', status: 'valid' }
    }
  };

  $: {
    if ($currentUser && !editMode) {
      formData = {
        certifications: {
          globalGap: {
            number: $currentUser.certifications?.globalGap?.number || '',
            validUntil: $currentUser.certifications?.globalGap?.validUntil || '',
            status: $currentUser.certifications?.globalGap?.status || 'valid'
          },
          grasp: {
            number: $currentUser.certifications?.grasp?.number || '',
            validUntil: $currentUser.certifications?.grasp?.validUntil || '',
            status: $currentUser.certifications?.grasp?.status || 'valid'
          },
          eco: {
            number: $currentUser.certifications?.eco?.number || '',
            validUntil: $currentUser.certifications?.eco?.validUntil || '',
            status: $currentUser.certifications?.eco?.status || 'valid'
          }
        }
      };
    }
  }

  function handleSubmit(event) {
    event.preventDefault();
    onSubmit(formData);
  }

  function formatDate(date) {
    if (!date) return 'Not set';
    return format(new Date(date), 'MMM d, yyyy');
  }
</script>

<div class="bg-white rounded-lg shadow-md p-6">
  <h2 class="text-lg font-semibold text-gray-900 mb-4">Certifications</h2>

  {#if editMode}
    <form on:submit={handleSubmit} class="space-y-6">
      {#each Object.entries(formData.certifications) as [certType, cert]}
        <div class="border-t pt-4 first:border-t-0 first:pt-0">
          <h3 class="font-medium text-gray-900 mb-3">{certType.toUpperCase()}</h3>
          <div class="space-y-4">
            <div>
              <label class="block text-sm font-medium text-gray-700">Certificate Number</label>
              <input
                type="text"
                bind:value={cert.number}
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
              />
            </div>

            <div>
              <label class="block text-sm font-medium text-gray-700">Valid Until</label>
              <input
                type="date"
                bind:value={cert.validUntil}
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
              />
            </div>

            <div>
              <label class="block text-sm font-medium text-gray-700">Status</label>
              <select
                bind:value={cert.status}
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
              >
                <option value="valid">Valid</option>
                <option value="pending">Pending</option>
                <option value="expired">Expired</option>
              </select>
            </div>
          </div>
        </div>
      {/each}

      <button
        type="submit"
        disabled={loading}
        class="w-full bg-green-600 text-white py-2 px-4 rounded-lg hover:bg-green-700 disabled:opacity-50"
      >
        {loading ? 'Saving...' : 'Save Certifications'}
      </button>
    </form>
  {:else}
    <div class="space-y-6">
      {#each Object.entries(formData.certifications) as [certType, cert]}
        <div class="border-t pt-4 first:border-t-0 first:pt-0">
          <h3 class="font-medium text-gray-900 mb-2">{certType.toUpperCase()}</h3>
          <div class="space-y-2">
            <p class="text-sm">
              <span class="text-gray-600">Number:</span>
              <span class="ml-2">{cert.number || 'Not set'}</span>
            </p>
            <p class="text-sm">
              <span class="text-gray-600">Valid Until:</span>
              <span class="ml-2">{formatDate(cert.validUntil)}</span>
            </p>
            <p class="text-sm">
              <span class="text-gray-600">Status:</span>
              <span class="ml-2 capitalize">{cert.status}</span>
            </p>
          </div>
        </div>
      {/each}
    </div>
  {/if}
</div>