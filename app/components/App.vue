<template>
  <Page>
    <ActionBar title="Trending Repos"/>
    <GridLayout columns="*" rows="*">
      <ListView for="item in Repos">
        <v-template>
          <StackLayout>
            <Label :text="item.name" class="ReposName"/>
            <Label :text="item.description" class="description"/>
            <StackLayout orientation="horizontal">
              <Image :src="item.owner.avatar_url" class="avatar" stretch="none"/>
              <Label :text="item.owner.login" class="username" stretch="none"/>
            </StackLayout>
            <StackLayout orientation="horizontal" horizontalAlignment="right">
              <Label :text="item.watchers" class="stars" stretch="none"/>
              <Image src="~/assets/images/star.png" class="stars_icon"/>
            </StackLayout>
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
      Repos: []
    };
  },
  mounted() {
    http
      .getJSON(
        "https://api.github.com/search/repositories?q=created:>2018-10-22&sort=stars&order=desc"
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
  text-align: center;
}

.message {
  vertical-align: center;
  text-align: center;
  font-size: 20;
  color: #333333;
}
.avatar {
  width: 25;
  height: 25;
}
.username {
  font-size: 20;
  color: #333333;
}
.stars {
  font-weight: bold;
}
.ReposName {
  font-weight: bold;
  font-size: 20;
  color: #333333;
  text-align: center;
}
.description {
  color: #333333;
  font-style: italic;
}
.stars_icon {
  width: 25;
  height: 25;
}
</style>
