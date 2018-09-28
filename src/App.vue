<template>
  <div id="app" class="shorten-page">
    <nav class="shorten-page_navigation">
      <img class="shorten-page_logo" src="./assets/svgs/logo.svg" alt="logo">
      <a href="" class="shorten-page_link">Tour</a>
      <a href="" class="shorten-page_link">Enterprise</a>
      <a href="" class="shorten-page_link">Resources</a>
      <a href="" class="shorten-page_link">About</a>
    </nav>

    <div class="shorten-page_cta">shorten. share. measure.</div>
    <div class="shorten-page_subtitle">Join Bitly, the world's leading link management platform.</div>
    
    <LinkShortener />
    <LinkList />
  </div>
</template>

<script>
import LinkShortener from './components/LinkShortener.vue'
import LinkList from './components/LinkList.vue'
import BitlySDK from './sdk.js';

export default {
  name: 'App',
  components: {
    LinkShortener,
    LinkList
  },
  data: function() {
    return {
      bitlySDK: function() {
        var b = new BitlySDK({
          accessToken: "f2c70570b2aebede898c69ceefd806a75c86e7b3"
        });

        return b;
      },
    }
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
}

.shorten-page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;

  background-image: url("./assets/svgs/background-image.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: auto 100%;
  color: #fff;
  font-family: Helvetica, Arial, sans-serif;
  text-transform: uppercase;
  text-decoration: none;
}

.shorten-page_navigation {
  display: flex;
  align-items: center;
  margin: 25px 160px 165px;
}

.shorten-page_logo {
  width: 135px;
  margin-right: 80px;
}

.shorten-page_link {
  margin-top: 10px;

  color: #fff;
  font-size: 18px;
  text-decoration: none;

  & + .shorten-page_link {
    margin-left: 45px;
  }
}

.shorten-page_cta{
  margin: 0 auto 25px;

  font-size: 60px;
}

.shorten-page_subtitle {
  margin: 0 auto;

  font-size: 26px;
  text-transform: none;
}

@media only screen and (max-width: 1300px) {
  .shorten-page_navigation {
    width: auto;
    margin-left: calc((100% - 1000px)/2);
    margin-right: calc((100% - 1000px)/2);
  }
}

@media only screen and (max-width: 1140px) {
  .shorten-page_navigation,
  .shorten-page_cta, 
  .shorten-page_subtitle {
    margin-left: 20px;
    margin-right: 20px;
  }

  .shorten-page_cta, 
  .shorten-page_subtitle {
    text-align: center;
  }
}

@media only screen and (max-width: 760px) {
  .shorten-page_navigation {
    margin-bottom: 60px;
  }

  .shorten-page_link {
    display: none;
  }
}

@media only screen and (max-width: 430px) {
  .shorten-page_navigation {
    margin-bottom: 20px;
  }

  .shorten-page_cta {
    font-size: 40px;
    margin-bottom: 15px;
  }

  .shorten-page_subtitle {
    font-size: 18px
  }
}
</style>
