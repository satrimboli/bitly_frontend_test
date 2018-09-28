<template>
  <div class="link-list" v-show="links.length">
    <ul>
      <li v-for="link in links" class="link-list_item">
        <div class="item_title">{{link.title}}</div>
        <a :href="link.long_url" class="item_long-url">{{link.long_url}}</a>
        <a :href="link.short_url" class="item_short-url">bit.ly/<span class="item_hash">{{link.hash}}</span></a>
        <div class="item_clicks"> {{link.clicks}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
import EventBus from './EventBus.vue';

export default {
  name: 'LinkList',
  data: function() {
    return {
      links: []
    }
  },
  mounted() {
    EventBus.$on('add-link', this.addLink);
  },
  methods: {
    addLink(link) {
      var newLink = {
        long_url: link.long_url,
        short_url: link.url,
        title: link.title,
        hash: link.hash
      }

      this.$parent.bitlySDK().info(newLink.short_url).then((result) => {
        // success
        newLink.title = result.title;
      }, () => {
        // failure
        newLink.title = newLink.long_url;
      });

      this.$parent.bitlySDK().clicks([newLink.short_url]).then((result) => {
        newLink.clicks = result[0].global_clicks;
      });

      this.links.push(newLink);
    }
  }
}
</script>

<style lang="scss">
$orange: #ee6123;
$dark-grey: #444;
$light-grey: #9b9b9b;

.link-list {
  width: 1000px;
  box-sizing: border-box;
  margin: 0 auto 40px;
  border-radius: 3px;
  padding: 40px 35px;

  background-color: #fff;
  color: $dark-grey;
  font-size: 18px;
  text-transform: none;

  ul {
    margin: 0;
    padding: 0;
  }
}

.link-list_item {
  list-style: none;

  & + .link-list_item {
    margin-top: 40px;
    border-top: 1px solid $light-grey;
    padding-top: 40px;
  }
}

.item_title {
  margin-bottom: 8px;
}

.item_long-url, 
.item_clicks {
  color: $light-grey;
}

.item_title, 
.item_long-url, 
.item_clicks {
  &:hover {
    text-decoration: underline;
  }
}

.item_long-url {
  display: block;
  margin-bottom: 40px;

  text-decoration: none;
}

.item_short-url {
  display: inline-block;
  
  color: $orange;
  text-decoration: none;
}

.item_hash {
  font-weight: 600;
}

.item_clicks {
  float: right;
  margin-top: 8px;
  
  font-size: 16px;

  &:after {
    content: '';
    position: relative;
    top: 2px;
    width: 24px;
    height: 17px;
    display: inline-block;
    margin-left: 8px;
    
    background-image: url("../assets/svgs/click-icon.svg");
  }
}

@media only screen and (max-width: 1140px) {
  .link-list {
    width: auto;
    margin-right: 20px;
    margin-left: 20px;
  }
}
</style>
