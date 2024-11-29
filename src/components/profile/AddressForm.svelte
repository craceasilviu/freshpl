<script>
  import { currentUser } from '../../stores/users';

  export let editMode = false;
  export let loading = false;
  export let onSubmit;

  let formData = {
    address: {
      street: '',
      city: '',
      state: '',
      country: '',
      postalCode: '',
      lat: null,
      lng: null
    }
  };

  $: {
    if ($currentUser && !editMode) {
      formData = {
        address: {
          street: $currentUser.address?.street || '',
          city: $currentUser.address?.city || '',
          state: $currentUser.address?.state || '',
          country: $currentUser.address?.country || '',
          postalCode: $currentUser.address?.postalCode || '',
          lat: $currentUser.address?.lat || null,
          lng: $currentUser.address?.lng || null
        }
      };
    }
  }

  function handleSubmit(event) {
    event.preventDefault();
    onSubmit(formData);
  }
</script>

<div class="bg-white rounded-lg shadow-md p-6">
  <h2 class="text-lg font-semibold text-gray-900 mb-4">Address</h2>

  {#if editMode}
    <form on:submit={handleSubmit} class="space-y-4">
      <div>
        <label for="street" class="block text-sm font-medium text-gray-700">Street</label>
        <input
          id="street"
          type="text"
          bind:value={formData.address.street}
          required
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
        />
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="city" class="block text-sm font-medium text-gray-700">City</label>
          <input
            id="city"
            type="text"
            bind:value={formData.address.city}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>

        <div>
          <label for="state" class="block text-sm font-medium text-gray-700">State</label>
          <input
            id="state"
            type="text"
            bind:value={formData.address.state}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="country" class="block text-sm font-medium text-gray-700">Country</label>
          <input
            id="country"
            type="text"
            bind:value={formData.address.country}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>

        <div>
          <label for="postalCode" class="block text-sm font-medium text-gray-700">Postal Code</label>
          <input
            id="postalCode"
            type="text"
            bind:value={formData.address.postalCode}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>
      </div>

      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="lat" class="block text-sm font-medium text-gray-700">Latitude</label>
          <input
            id="lat"
            type="number"
            step="any"
            bind:value={formData.address.lat}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>

        <div>
          <label for="lng" class="block text-sm font-medium text-gray-700">Longitude</label>
          <input
            id="lng"
            type="number"
            step="any"
            bind:value={formData.address.lng}
            required
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500"
          />
        </div>
      </div>

      <button
        type="submit"
        disabled={loading}
        class="w-full bg-green-600 text-white py-2 px-4 rounded-lg hover:bg-green-700 disabled:opacity-50"
      >
        {loading ? 'Saving...' : 'Save Address'}
      </button>
    </form>
  {:else}
    <div class="space-y-2">
      <p class="text-gray-900">{formData.address.street || 'No street address'}</p>
      <p class="text-gray-900">
        {[
          formData.address.city,
          formData.address.state,
          formData.address.postalCode
        ].filter(Boolean).join(', ') || 'No city/state information'}
      </p>
      <p class="text-gray-900">{formData.address.country || 'No country'}</p>
      {#if formData.address.lat && formData.address.lng}
        <p class="text-sm text-gray-500">
          Coordinates: {formData.address.lat}, {formData.address.lng}
        </p>
      {/if}
    </div>
  {/if}
</div>