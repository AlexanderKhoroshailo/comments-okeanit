<template>
  <div>
    <li>
      <span
        >{{ getDate }} / {{ comment.authorName }} /
        {{ comment.authorUrl }}</span
      >
      <p v-html="getComment"></p>
    </li>
    <hr />
  </div>
</template>

<script>
export default {
  props: ["comment", "id"],

  computed: {
    getComment() {
      let resultComment = [...this.comment.content];
      const sortedPostTones = [...this.comment.contentPostTones];
      sortedPostTones.sort(({ position: s1 }, { position: s2 }) => s2 - s1);

      for (let i = 0; i < sortedPostTones.length; i++) {
        const coloredArr = this.comment.content
          .substring(
            sortedPostTones[i].position,
            sortedPostTones[i].position + sortedPostTones[i].length
          )
          .split();

        resultComment.splice(
          sortedPostTones[i].position,
          sortedPostTones[i].length,
          `<span style="background-color:hsl(${
            60 + 60 * sortedPostTones[i].tone
          },75%,75%);">
          ${coloredArr}
          </span>`
        );
      }
      return resultComment.join("");
    },

    getDate() {
      const newDate = new Date(this.comment.date);
      return (
        newDate.toLocaleString("ru", {
          hour: "numeric",
          minute: "numeric",
        }) +
        ", " +
        newDate.toLocaleString("ru", {
          month: "long",
          day: "numeric",
          year: "numeric",
          timezone: "UTC",
        })
      );
    },
  },

  methods: {},
};
</script>

<style>
li{
  margin-bottom: 1%;
}
hr{
  margin-bottom: 3%;
}
</style>
