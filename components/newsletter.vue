<template>
  <section
    id="newsletter"
    class="bg-blue-dark px-4 py-16"
  >
    <section-heading
      white
      heading="Stay Informed"
      subheading="Get upates on current projects, events, and resources"
      class="w-full"
    />
    <form
      v-if="!subscribed"
      class="pb-2"
      novalidate
      @submit.prevent="subscribe"
    >
      <div class="text-center mt-4">
        <input
          id="EMAIL"
          v-model="email"
          name="EMAIL"
          type="text"
          class="
            outline-none focus:shadow-outline border-grey-dark rounded-full
            p-4 w-3/4 md:max-w-sm text-sm
          "
          placeholder="Email address"
          aria-label="Email address"
        >
        <button
          class="
            outline-none bg-white border border-blue-dark rounded-full
            text-blue-dark p-4 -ml-16 uppercase font-bold text-sm
            hover:text-blue-dark focus:outline-none focus:shadow-outline
          "
          type="submit"
        >Subscribe</button>
      </div>
      <div
        v-if="message"
        class="w-3/4 md:max-w-sm mx-auto"
      >
        <div class="up-arrow mt-1 ml-4"/>
        <div
          class="bg-blue-light text-blue-dark font-bold px-4 py-3 mx-auto"
          role="alert"
        >
          <p
            class="text-sm"
            v-html="message"
          />
        </div>
      </div>
    </form>
    <div
      v-if="subscribed"
      class="w-3/4 md:max-w-sm mx-auto"
    >
      <div
        class="
          bg-blue-light text-blue-dark text-center font-bold px-4 py-3
          mx-auto
        "
        role="alert"
      >
        <p
          class="text-sm"
          v-html="message"
        />
      </div>
    </div>
  </section>
</template>

<script>
import sectionHeading from '~/components/section-heading'

export default {
  components: {
    sectionHeading
  },
  data() {
    return {
      email: '',
      subscribed: false,
      message: ''
    }
  },
  methods: {
    subscribe() {
      const url =
        'https://codes.us19.list-manage.com/subscribe/post-json' +
        '?u=284c94c0d64272db7f56f4c6d&amp;id=f13ffe3703&c?'
      this.$jsonp(url, { EMAIL: this.email, callbackQuery: 'c' })
        .then(response => {
          if (response.result === 'error') {
            this.subscribed = false
            if (/^[\d ]+-/.test(response.msg)) {
              this.message = response.msg.slice(response.msg.indexOf('-') + 1)
            } else {
              this.message = response.msg || 'Oh no! Something went wrong.'
            }
          } else {
            // success handler
            this.subscribed = true
            this.message = response.msg
          }
        })
        .catch(() => {
          this.subscribed = false
          this.message = 'Oh no! Something went wrong.'
        })
    }
  }
}
</script>

<style lang="scss" scoped>
.up-arrow {
  border-color: config('colors.blue-light') transparent;
  border-style: solid;
  border-width: 0px 10px 10px;
  height: 0px;
  width: 0px;
}
</style>
