<template>
  <div class="link-shortener">
    <input v-model='link' class="link-shortener_input" type="text" placeholder="Paste a link to shorten it.">
    <button @click="shortenLink" class="link-shortener_button">shorten</button>
    <div v-if="error" class="link-shortener_error">
      There was an error shortening your link.
    </div>
  </div>

</template>

<script>
import EventBus from './EventBus.vue';

export default {
  name: 'LinkShortener',
  data: function() {
    return {
      link: '',
      error: false
    }
  },
  methods: {
    shortenLink() {
      this.$parent.bitlySDK().shorten(this.link).then((result) => {
        // success
        EventBus.$emit('add-link', result);
        this.link = '';
        this.error = false;
      }, () => {
        // failure
        this.error = true;
      });
    }
  }
}
</script>

<style lang="scss">
$orange: #ee6123;
$dark-grey: #444;
$light-grey: #9b9b9b;

.link-shortener {
  display: flex;
  flex-wrap: wrap;
  width: 1000px;
  height: auto;
  margin: 60px auto 40px;
}

.link-shortener_input {
  flex-grow: 1;
  height: 80px;
  box-sizing: border-box;
  box-shadow: 0;
  border: 0;
  border-radius: 4px 0 0 4px;
  padding-left: 34px;

  color: $light-grey;
  font-size: 22px;
  vertical-align: middle;

  &:focus {
    border: 2px solid $orange;
    padding-left: 32px;
    outline: none;
  }

  &::-webkit-input-placeholder { /* Chrome/Opera/Safari */
    color: $light-grey;
  }
  &::-moz-placeholder { /* Firefox 19+ */
    color: $light-grey;
  }
  &:-ms-input-placeholder { /* IE 10+ */
    color: $light-grey;
  }
  &:-moz-placeholder { /* Firefox 18- */
    color: $light-grey;
  }
}

.link-shortener_button {
  height: 80px;
  box-sizing: border-box;
  box-shadow: none;
  border: none;
  border-radius: 0 3px 3px 0;
  padding: 0 45px;
  
  background-color: $orange;
  color: #fff;
  font-size: 18px;
  text-transform: uppercase;
  vertical-align: middle;

  &:focus {
    outline: 0;
  }
}

.link-shortener_error {
  width: 100%;
  margin-top: 10px;
  border-radius: 4px;
  padding: 5px;

  background-color: #fff;
  color: #bb0000;
  font-size: 16px;
  line-height: 22px;
  text-transform: none;

  &:before {
    content: '!';
    width: 22px;
    height: 22px;
    display: inline-block;
    margin-right: 4px;
    border-radius: 50%;

    background-color: #bb0000;
    color: white;
    text-align: center;
  }
}


@media only screen and (max-width: 1140px) {
  .link-shortener {
    width: auto;
    margin-right: 20px;
    margin-left: 20px;
  }
}

@media only screen and (max-width: 490px) {
  .link-shortener_input, 
  .link-shortener_button {
    height: 60px;

    font-size: 18px;
  }

  .link-shortener_input {
    padding-left: 15px;

    &:focus {
      padding-left: 13px;
    }
  }

  .link-shortener_button {
    padding-right: 20px;
    padding-left: 20px;
  }
}

@media only screen and (max-width: 430px) {
  .link-shortener {
    margin-top: 40px;
    margin-bottom: 20px;
  }

  .link-shortener_button, 
  .link-shortener_input {
    height: 40px;

    font-size: 16px;
  }
}
</style>
