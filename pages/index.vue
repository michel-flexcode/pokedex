<script setup>
const query = gql`
  query Pokemons {
    pokemons(orderBy: id_ASC) {
      createdAt
      description
      id
      nom
      publishedAt
      slug
      updatedAt
      image {
        url(
          transformation: {
            document: { output: { format: webp } }
            image: { resize: { fit: crop, height: 256, width: 256 } }
          }
        )
      }
    }
  }
`;

const pokemons = ref();
const { data } = await useAsyncQuery(query);
console.log(data.value);
pokemons.value = data.value.pokemons;
</script>

<template>
  <div class="flex justify-center items-center space-y-4">
    <ul v-if="pokemons">
      <li v-for="pokemon in pokemons" :key="pokemon.id">
        <NuxtLink :to="`/pokemon/${pokemon.slug}`">
          <NuxtImg :src="pokemon.image.url" :alt="pokemon.nom" />
          <h2 class="text-1xl text-center">{{ pokemon.nom }}</h2>
        </NuxtLink>
      </li>
    </ul>
    <ul v-else>
      <li>Loading...</li>
    </ul>
  </div>
</template>
