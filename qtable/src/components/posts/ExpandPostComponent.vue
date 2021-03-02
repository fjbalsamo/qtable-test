<template>
  <q-td colspan="100%">
    <div
        v-for="(post, index) in posts"
        :key="index"
        class="q-ma-md"
    >
        <span class="q-mr-md">
            <strong>POST_ID</strong> {{post.id}}
        </span>
        <!-- <blockquote>
            <p class="text-uppercase">{{post.title}}</p>
            <p>
                <small>
                    {{post.body}}
                </small>
            </p>
        </blockquote> -->
    </div>
  </q-td>
</template>

<script>
export default {
  name: "ExpandPostComponent",
  props: {
      posts: {
          type: Array,
          default: () => []
      }
  }
};
</script>

<style></style>
