<script>
  import { currentUser } from '../../stores/users';

  export let editMode = false;
  export let loading = false;
  export let onSubmit;

  let formData = {
    companyName: '',
    vatNumber: ''
  };

  $: {
    if ($currentUser && !editMode) {
      formData = {
        companyName: $currentUser.companyName || '',
        vatNumber: $currentUser.vatNumber || ''
      };
    }
  }

  function handleSubmit(event) {
    event.preventDefault();
    onSubmit(formData);
  }
</script>

<div class="bg-white rounded-lg shadow-md p-6">
  <h2 class="text-lg font-semibold text-gray-900 mb-4">Company Details</h2>
  
  {#if editMode}
    <form on:submit={handleSubmit} class="space-y-4">
      <div>
        <label for="companyName" class="block text-sm font-medium text-gray-700">Company Name</label>
        <input
          id="companyName"
          type="text"
          bind:value={formData.companyName}
          required
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
        />
      </div>

      <div>
        <label for="vatNumber" class="block text-sm font-medium text-gray-700">VAT Number</label>
        <input
          id="vatNumber"
          type="text"
          bind:value={formData.vatNumber}
          required
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
        />
      </div>

      <button
        type="submit"
        disabled={loading}
        class="w-full bg-green-600 text-white py-2 px-4 rounded-lg hover:bg-green-700 disabled:opacity-50"
      >
        {loading ? 'Saving...' : 'Save Company Details'}
      </button>
    </form>
  {:else}
    <div class="space-y-4">
      <div>
        <span class="text-sm font-medium text-gray-700">Company Name</span>
        <p class="mt-1 text-gray-900">{formData.companyName || 'Not set'}</p>
      </div>
      <div>
        <span class="text-sm font-medium text-gray-700">VAT Number</span>
        <p class="mt-1 text-gray-900">{formData.vatNumber || 'Not set'}</p>
      </div>
    </div>
  {/if}
</div>