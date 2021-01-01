<template>
  <div>
    <h1 class="friends-title text-center space-bottom">阅读 & 思考</h1>
    <div class="posts content-box">

<!-- >  <div class="read-container text-center space-bottom">
<-->
    该页面还未做好，初步打算将该页面的内容放在此处。https://blog.weijiew.com/2020/01/1-read-book/ 
    </div>
  </div>
</template>

<script>

export default {
  metaInfo: {
    title: 'Friends',
  },
  data() {
    return {
    }
  },
}

</script>

<style lang="scss">

.read-container {
  max-width: var(--content-width);
  margin-left: auto;
  margin-right: auto;

  .btn {
    margin: 0.3em 0.6em 0.3em 0;
    text-decoration: none;
    padding: 0.5em;
    border-radius: var(--radius);
    display: inline-block;
    overflow: hidden;
    position: relative;

    .avatar {
      position: absolute;
      top: 0;
      left: 0;
      bottom: 0;
      width: 48px;
      height: 48px;
      border-radius: var(--radius);
      background-color: #fff;
      transform: rotate(20deg) translate(-15%, 0%);
    }

    span {
      margin-left: 2.6em;
    }
  }

  .note {
    margin: calc(var(--space) / 2) 0;
    font-size: 0.9em;
    opacity: 0.8;
  }
}

</style>
