<template>
  <div class="container">
    <div class="text">
      {{ filteredCount === prCount ? '' : `${filteredCount} /` }} {{ prCount }} open pull requests from {{ repos.length }} repos
    </div>
    <pull-request v-for="pr in filteredPrs" v-bind="pr" :key="pr.id"></pull-request>
  </div>
</template>

<script>
  import GitHub from 'github-api';
  import PullRequest from '../pr/PullRequest.vue';

  const github = new GitHub({ token: '' });
  const paybase = github.getOrganization('paybase');

  export default {
    name: 'github-tracker',
    components: { PullRequest },
    props: [ 'filter' ],
    data() {
      return {
        repos: [],
        prList: [],
        prCount: 0,
      };
    },
    computed: {
      orderedPrs: function() {
        return this.prList.sort(function(a, b) {
          return new Date(b.created_at).getTime() - new Date(a.created_at).getTime();
        });
      },
      filteredPrs: function() {
        return this.orderedPrs.filter(pr => pr.user.login.includes(this.filter) || pr.head.repo.name.includes(this.filter));
      },
      filteredCount: function() { return this.filteredPrs.length},
    },
    created() {
      this.fetchRepos();
      setInterval(this.fetchRepos.bind(this), 60000 * 5)
    },
    methods: {
      fetchRepos() {
        paybase.getRepos((err, res) => {
          this.repos = [];
          this.prList = [];
          this.prCount = 0;
          if (res) {
            const repoPrs = res.map(async (repo) => {
              const data = await fetch(`https://api.github.com/repos/paybase/${repo.name}/pulls`, {
                method: 'get',
                headers: {
                  'Authorization': 'token ''',
                  'Content-Type': 'application/x-www-form-urlencoded'
                },
              })
              const json = await data.json()
              this.repos.push({
                name: repo.name,
                url: repo.html_url,
                pullRequests: json,
              })
              this.prList.push(...json);
              await this.countPrs();
              return json;
            });
          }
        })
      },
      countPrs() {
        this.prCount = this.repos.reduce((acc, val) => acc + val.pullRequests.length, 0);
      }
    }
  }
</script>

<style scoped>
  .text {
    padding-top: 24px;
    text-align: center;
  }
</style>
