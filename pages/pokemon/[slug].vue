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
  <div v-if="pokemon" class="max-w-lg space-y-8 mx-auto">
    <NuxtImg class="" :src="pokemon.image.url" :alt="pokemon.nom" />
    <h2 class="text-3xl text-center">{{ pokemon.nom }}</h2>
    <p class="text-justify text-red-950">{{ pokemon.description }}</p>
    <p class="text-justify text-red-950">{{ pokemon.pdv }}</p>
    <p class="text-justify text-red-950">{{ pokemon.weight }}</p>
    <p class="text-justify text-red-950">{{ pokemon.height }}</p>
    <p class="text-justify text-red-950">{{ pokemon.color }}</p>

    <!-- Check if typeA exists before accessing its properties -->
    <p>Pokemon de type :</p>
    <p v-if="pokemon.typepokemonA" class="text-justify text-red-950">
      {{ pokemon.typepokemonA.nom }}
      <img
        :src="pokemon.typepokemonA.image.url"
        :alt="pokemon.typepokemonA.image.url"
      />
    </p>
    <p v-else class="text-justify text-red-950">No TypeA available</p>

    <!-- Check if typeB exists before accessing its properties -->
    <p v-if="pokemon.typepokemonB" class="text-justify text-red-950">
      {{ pokemon.typepokemonB.nom }}
      <img
        :src="pokemon.typepokemonB.image.url"
        :alt="pokemon.typepokemonB.image.url"
      />
    </p>
    <p v-else class="text-justify text-red-950"></p>

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
