<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())
if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found', fatal: true })
}

useSeoMeta({
  titleTemplate: '',
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})
</script>

<template>
  <div v-if="page">
    <!-- Hero Section -->
    <ULandingHero
      :title="page.hero.title"
      :description="page.hero.description"
      :links="page.hero.links"
    >
      <div class="absolute inset-0 landing-grid z-[-1] [mask-image:radial-gradient(100%_100%_at_top_right,white,transparent)]" />

      <template #headline>
        <UBadge
          v-if="page.hero.headline"
          variant="subtle"
          size="lg"
          class="relative rounded-full font-semibold"
        >
          <NuxtLink
            :to="page.hero.headline.to"
            target="_blank"
            class="focus:outline-none"
            tabindex="-1"
          >
            <span
              class="absolute inset-0"
              aria-hidden="true"
            />
          </NuxtLink>

          {{ page.hero.headline.label }}

          <UIcon
            v-if="page.hero.headline.icon"
            :name="page.hero.headline.icon"
            class="ml-1 w-4 h-4 pointer-events-none"
          />
        </UBadge>
      </template>

      <!-- Formulário de Cadastro -->
      <template #extra>
        <div class="auth-buttons">
          <button @click="signInWithApple" class="auth-button">
            <UIcon name="i-mdi-apple" class="w-5 h-5 mr-2" />
            Apple
          </button>
          <button @click="signInWithGoogle" class="auth-button">
            <UIcon name="i-mdi-google" class="w-5 h-5 mr-2" />
            Google
          </button>
          <span class="text-center my-2">Ou</span>
          <form @submit.prevent="signUp" class="auth-form">
            <input type="text" placeholder="Nome" required class="auth-input" />
            <input type="email" placeholder="Email" required class="auth-input" />
            <input type="password" placeholder="Senha" required class="auth-input" />
            <button type="submit" class="auth-submit-button">
              Cadastre-se agora
            </button>
          </form>
          <p class="text-sm text-center mt-2">
            Ao clicar em "Cadastre-se agora", você concorda com nossos
            <a href="#" class="text-primary-500 hover:underline">Termos de Uso</a> e
            <a href="#" class="text-primary-500 hover:underline">Política de Privacidade</a>.
          </p>
          <p class="text-sm text-center mt-2">
            Já possui uma conta? <a href="#" class="text-primary-500 hover:underline">Faça Login</a>
          </p>
        </div>
      </template>
    </ULandingHero>

    <!-- Destaques -->
    <ULandingSection class="!pt-0">
      <div class="stats">
        <div v-for="stat in page.stats" :key="stat.label">
          <span>{{ stat.value }}</span>
          <span>{{ stat.label }}</span>
        </div>
      </div>
      <button @click="startFree" class="start-free-button">
        Comece na HEBREUS gratuitamente
      </button>
    </ULandingSection>

    <!-- Benefícios -->
    <ULandingSection
      :title="page.benefits.title"
      :description="page.benefits.description"
    >
      <UPageGrid>
        <ULandingCard
          v-for="(benefit, index) in page.benefits.items"
          :key="index"
          v-bind="benefit"
        />
      </UPageGrid>
    </ULandingSection>

    <!-- Funcionalidades -->
    <ULandingSection
      :title="page.features.title"
      :description="page.features.description"
    >
      <UPageGrid>
        <ULandingCard
          v-for="(feature, index) in page.features.items"
          :key="index"
          v-bind="feature"
        />
      </UPageGrid>
    </ULandingSection>

    <!-- Integrações -->
    <ULandingSection
      :title="page.integrations.title"
      :description="page.integrations.description"
    >
      <div class="platforms">
        <img
          v-for="platform in page.integrations.platforms"
          :key="platform.name"
          :src="platform.logo"
          :alt="platform.name"
        />
      </div>
    </ULandingSection>

    <!-- Ferramentas -->
    <ULandingSection
      :title="page.tools.title"
      :description="page.tools.description"
    >
      <UPageGrid>
        <ULandingCard
          v-for="(tool, index) in page.tools.items"
          :key="index"
          v-bind="tool"
        />
      </UPageGrid>
    </ULandingSection>

    <!-- Preços -->
    <ULandingSection
      :title="page.pricing.title"
      :description="page.pricing.description"
    >
      <div class="price-details">
        <span>{{ page.pricing.details }}</span>
      </div>
      <button @click="createAccount" class="create-account-button">
        Criar conta
      </button>
    </ULandingSection>

    <!-- Depoimentos -->
    <ULandingSection
      :title="page.testimonials.title"
      :description="page.testimonials.description"
    >
      <UPageColumns class="xl:columns-4">
        <div
          v-for="(testimonial, index) in page.testimonials.items"
          :key="index"
          class="break-inside-avoid"
        >
          <ULandingTestimonial
            v-bind="testimonial"
            class="bg-gray-100/50 dark:bg-gray-800/50"
          />
        </div>
      </UPageColumns>
    </ULandingSection>

    <!-- Perguntas Frequentes -->
    <ULandingSection
      :title="page.faq.title"
      :description="page.faq.description"
    >
      <div class="faq-list">
        <div v-for="(faq, index) in page.faq.items" :key="index">
          <h3>{{ faq.question }}</h3>
          <p>{{ faq.answer }}</p>
        </div>
      </div>
    </ULandingSection>

    <!-- Rodapé -->
    <footer>
      <a href="#">Termos de Uso</a>
      <a href="#">Política de Privacidade</a>
      <a href="#">Central de Ajuda</a>
    </footer>
  </div>
</template>

<style scoped>
.landing-grid {
  background-size: 100px 100px;
  background-image:
    linear-gradient(to right, rgb(var(--color-gray-200)) 1px, transparent 1px),
    linear-gradient(to bottom, rgb(var(--color-gray-200)) 1px, transparent 1px);
}
.dark {
  .landing-grid {
    background-image:
      linear-gradient(to right, rgb(var(--color-gray-800)) 1px, transparent 1px),
      linear-gradient(to bottom, rgb(var(--color-gray-800)) 1px, transparent 1px);
  }
}

.auth-buttons {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 400px;
  margin: 0 auto;
}

.auth-button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.75rem;
  border: 1px solid rgb(var(--color-gray-200));
  border-radius: 0.5rem;
  background-color: white;
  color: rgb(var(--color-gray-800));
  font-weight: 500;
  transition: background-color 0.2s;
}

.auth-button:hover {
  background-color: rgb(var(--color-gray-50));
}

.auth-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.auth-input {
  padding: 0.75rem;
  border: 1px solid rgb(var(--color-gray-200));
  border-radius: 0.5rem;
  background-color: white;
  color: rgb(var(--color-gray-800));
  font-size: 0.875rem;
}

.auth-submit-button {
  padding: 0.75rem;
  border: none;
  border-radius: 0.5rem;
  background-color: rgb(var(--color-primary-500));
  color: white;
  font-weight: 500;
  transition: background-color 0.2s;
}

.auth-submit-button:hover {
  background-color: rgb(var(--color-primary-600));
}

.stats {
  display: flex;
  justify-content: space-around;
  padding: 2rem;
  background: #f9f9f9;
}

.platforms {
  display: flex;
  justify-content: center;
  gap: 1rem;
  padding: 2rem;
}

.faq-list {
  padding: 2rem;
  text-align: left;
}

footer {
  text-align: center;
  padding: 1rem;
  background: #333;
  color: white;
}
</style>