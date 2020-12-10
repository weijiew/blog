<template>
  <div class="author">
    <g-image
      alt="Author image"
      class="author__image"
      src="~/assets/images/author.jpg"
      width="180"
      height="180"
      blur="5"
    />

    <h1 v-if="showTitle" class="author__site-title">
      {{ $static.metadata.siteName }}
    </h1>

    <p class="author__intro">
      每一个不曾起舞的日子都是对声明的浪费！
    </p>

    <p class="author__links">
      <g-link href="//weijiew.com" target="_blank"
        ><font-awesome :icon="['fas', 'id-badge']"
      /></g-link>

      <g-link to="/archives" style="color: var(--title-color);"
        ><font-awesome :icon="['fas', 'archive']"/></g-link
      >

      <g-link to="/friends" style="color: #6c5ce7;"
        ><font-awesome :icon="['fab', 'fly']"/></g-link>

      <g-link to="//www.kaggle.com/w836678589" style="color: #0984e3;"
        ><font-awesome :icon="['fab', 'kaggle']"/></g-link
      >

      <a
        href="//github.com/weijiew"
        target="_blank"
        style="color: var(--title-color);"
        ><font-awesome :icon="['fab', 'github']"
      /></a>
      
      <a
        href="//www.zhihu.com/people/wei-jie-66-92"
        target="_blank"
        style="color: var(--title-color);"
        ><font-awesome :icon="['fab', 'zhihu']"
      /></a>


    </p>
  </div>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
export default {
  props: ['showTitle'],
}
</script>

<style lang="scss">
.author {
  margin: 0 auto;
  max-width: 500px;
  text-align: center;
  padding: calc(var(--space) / 2) 0;

  &__image {
    border-radius: 100%;
    width: 90px;
    height: 90px;
    margin-bottom: 1em;
  }

  &__intro {
    opacity: 0.8;
  }

  &__site-title {
    font-size: 1.5em;
  }

  &__links {
    margin-top: -0.5em;
    font-size: 1em;
    a {
      color: var(--link-color);
      margin: 0 0.5em;
    }
  }
}
</style>
