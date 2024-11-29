<script>
  import { currentUser } from '../../stores/users';
  import { products } from '../../stores/products';
  import { authorizations } from '../../stores/authorizations';

  $: authorizedProductsList = $products.filter(product => 
    $authorizations.some(auth => 
      auth.userId === $currentUser?.id && 
      auth.productId === product.id
    )
  );
</script>

<div class="bg-white rounded-lg shadow-md p-6">
  <h2 class="text-lg font-semibold text-gray-900 mb-4">Authorized Products</h2>

  {#if authorizedProductsList.length === 0}
    <p class="text-gray-600">No authorized products yet.</p>
  {:else}
    <div class="grid gap-4">
      {#each authorizedProductsList as product}
        <div class="border rounded-lg p-4">
          <h3 class="font-medium text-gray-900">{product.name}</h3>
          <p class="text-sm text-gray-600">
            Box Size: {product.boxSize} {product.unit}s/box
          </p>
        </div>
      {/each}
    </div>
  {/if}
</div>