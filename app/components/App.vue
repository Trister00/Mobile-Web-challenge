<template>
  <Page>
    <ActionBar title="Trending Repos"/>
    <GridLayout columns="*" rows="*">
      <ListView for="item in Repos">
        <v-template>
          <StackLayout>
            <Label :text="item.name"/>
            <Label :text="item.description"/>
            <Label :text="item.owner.login"/>
            <Image :src="item.owner.avatar_url"/>
            <Label :text="item.watchers"/>
          </StackLayout>
        </v-template>
      </ListView>
    </GridLayout>
  </Page>
</template>

<script >
const http = require("http");
export default {
  data() {
    return {
      msg: "Hello World!",
      Repos: []
    };
  },
  mounted() {
    http
      .getJSON(
        "https://api.github.com/search/repositories?q=created:>2017-10-22&sort=stars&order=desc"
      )
      .then(
        r => {
          this.Repos.push(...JSON.parse(JSON.stringify(r)).items);
        },
        error => {
          console.log(error);
        }
      );
  }
};
</script>

<style scoped>
ActionBar {
  background-color: #53ba82;
  color: #ffffff;
}

.message {
  vertical-align: center;
  text-align: center;
  font-size: 20;
  color: #333333;
}
</style>
