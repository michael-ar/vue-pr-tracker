<template>
  <a class="repo" :href="html_url" target="_blank">
    <a :href="html_url" target="_blank">
      <h2>{{ title }}</h2>
    </a>
    <p><span>opened: <span class="bold">{{ formatDate(created_at) }}</span></span></p>
    <p><span>repo: <a :href="head.repo.html_url" class="bold" target="_blank">{{ head.repo.name }}</a></span></p>
    <p><span>10x dev legend: <span class="bold">{{ user.login }}</span></span></p>
  </a>
</template>

<script>
import moment from 'moment';
export default {
  name: 'pull-request',
  props: [ 'title', 'html_url', 'created_at', 'head', 'user' ],
  methods: {
    formatDate(date) {
      const mDate = moment(date);
      const weekAgo = moment().subtract(7, 'days').startOf('day');
      const lessThanWeek = mDate.isAfter(weekAgo)
      return lessThanWeek ? mDate.fromNow() : mDate.format('h:mma, MMMM Do YYYY');
    }
  }
}
</script>

<style scoped>
  .repo {
    text-align: left;
    display: inline-block;
    width: 40%;
    padding: 24px;
    margin: 20px;
    border: 1px solid #fedd00;
  }
  .repo:hover {
    transform: scale(1.02);
    transform-origin: center;
  }

  a {
    color: #000000;
    text-decoration: none;
  }
  h2 {
    margin: 0;
    font-size: 20px;
    position: relative;
    font-weight: 700;
    margin-bottom: 12px;
  }
  h2:after{
    content: "";
    position: absolute;
    width: 100%;
    bottom: 1px;
    left: 0;
    z-index: -1;
    opacity: 60%;
    background: #fedd00;
    height: 8px;
  }
  h3, p {
    margin: 0;
    padding: 0;
  }
  h3 {
    font-size: 16px;
  }
  .bold {
    font-weight: 700;
  }
  p {
    margin-bottom: 6px;
  }
</style>
