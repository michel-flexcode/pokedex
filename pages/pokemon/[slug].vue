<script setup>
const query = gql`
  query Pokemon($slug: String!) {
    pokemon(where: { slug: $slug }) {
      attaques {
        degat
        nom
        description
        image {
          url(transformation: {})
        }
        typepokemon {
          image {
            url(transformation: {})
          }
        }
      }

      description
      height
      weight
      color
      id
      image {
        url(transformation: {})
      }
      typepokemonA {
        color
        nom
        image {
          url(transformation: {})
        }
      }
      slug
      pdv
      nom
      typepokemonB {
        color
        nom
        image {
          url(transformation: {})
        }
      }
    }
  }
`;

const pokemon = ref();
const route = useRoute();
const { data } = await useAsyncQuery(query, {
  slug: route.params.slug,
});
console.log(data.value);
console.log(data.value.pokemon.pokemonTypeA);
pokemon.value = data.value.pokemon;
</script>

<template>
  <Head v-if="pokemon">
    <Title>{{ pokemon.nom }} - Détails du Pokémon</Title>
    <Meta
      name="description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta
      property="og:title"
      :content="`${pokemon.nom} - Détails du Pokémon`"
    />
    <Meta
      property="og:description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta property="og:image" :content="pokemon.image.url" />
    <Meta property="og:type" content="website" />
    <Meta property="og:locale" content="fr_FR" />
    <Meta name="twitter:card" content="summary_large_image" />
    <Meta
      name="twitter:title"
      :content="`${pokemon.nom} - Détails du Pokémon`"
    />
    <Meta
      name="twitter:description"
      :content="`Découvrez des détails sur ${pokemon.nom}: ${pokemon.description}`"
    />
    <Meta name="twitter:image" :content="pokemon.image.url" />
  </Head>

  <!-- commentaire : ici est l'affichage réel -->
  <div
    v-if="pokemon"
    :style="{ backgroundColor: pokemon.color }"
    class="max-w-lg mx-auto p-8 rounded-lg space-y-8 border-8 border-yellow-500"
  >
    <div class="flex justify-between">
      <h2 class="text-3xl text-left">{{ pokemon.nom }}</h2>
      <div class="flex justify-between rounded-full h-8">
        <p class="text-justify text-black-950">{{ pokemon.pdv }} HP</p>
        <img
          :src="pokemon.typepokemonA.image.url"
          :alt="pokemon.typepokemonA.image.url"
          class="rounded-full w-8 h-8"
        />
        <img
          v-if="pokemon.typepokemonB"
          :src="pokemon.typepokemonB.image.url"
          :alt="pokemon.typepokemonB.image.url"
          class="rounded-full w-8 h-8"
        />
      </div>
    </div>
    <NuxtImg
      class="border-8 border-yellow-500 rounded-lg"
      :src="pokemon.image.url"
      :alt="pokemon.nom"
    />

    <div class="flex justify-between items-center bg-white rounded-2xl p-2 m-4">
      <p class="text-justify text-black-950 mx-4">{{ pokemon.weight }} kg</p>
      <p class="text-justify text-black-950">{{ pokemon.height }} cm</p>
      <p class="text-justify text-black-950 mx-4">{{ pokemon.color }}</p>
    </div>

    <p class="text-justify text-black-950">{{ pokemon.description }}</p>

    <p>Capacités :</p>
    <div v-for="attaque in pokemon.attaques" :key="attaque.nom">
      <p>Attaque :</p>
      {{ attaque.nom }}
      <p>Dégats :</p>
      {{ attaque.degat }}
      <p>Description :</p>
      {{ attaque.description }}
      <p>Image de l'attaque :</p>
      <!-- {{ attaque.image.url }} -->
      <img :src="attaque.image.url" :alt="attaque.nom" />
      <p>Type de l'attaque :</p>
      <!-- {{ attaque.image.url }} -->
      <img :src="attaque.typepokemon.image.url" :alt="attaque.nom" />
      <!-- <img :src="attaque.typepokemon.image" :alt="attaque.nom" /> -->
    </div>
  </div>
  <div v-else>
    <li>Loading...</li>
  </div>
</template>
