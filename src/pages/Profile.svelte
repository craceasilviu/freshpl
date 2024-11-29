<script>
  import { currentUser } from '../stores/users';
  import CompanyDetails from '../components/profile/CompanyDetails.svelte';
  import AddressForm from '../components/profile/AddressForm.svelte';
  import Certifications from '../components/profile/Certifications.svelte';
  import AuthorizedProducts from '../components/profile/AuthorizedProducts.svelte';
  import { showError } from '../lib/toast';
  import { updateUserProfile } from '../services/users';

  let editMode = false;
  let loading = false;

  async function handleSubmit(formData) {
    if (!$currentUser?.id || loading) return;

    loading = true;
    try {
      const success = await updateUserProfile($currentUser.id, formData);
      if (success) {
        editMode = false;
      }
    } catch (error) {
      console.error('Error updating profile:', error);
      showError(error.message || 'Failed to update profile');
    } finally {
      loading = false;
    }
  }
</script>

<div class="container mx-auto px-4 py-8">
  <div class="flex justify-between items-center mb-6">
    <h1 class="text-2xl font-bold text-gray-900">Producer Profile</h1>
    <button
      on:click={() => editMode = !editMode}
      class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700"
    >
      {editMode ? 'Cancel Edit' : 'Edit Profile'}
    </button>
  </div>

  <div class="grid md:grid-cols-2 gap-8">
    <div class="space-y-6">
      <CompanyDetails {editMode} {loading} onSubmit={handleSubmit} />
      <AddressForm {editMode} {loading} onSubmit={handleSubmit} />
    </div>
    <div class="space-y-6">
      <Certifications {editMode} {loading} onSubmit={handleSubmit} />
      <AuthorizedProducts />
    </div>
  </div>
</div>