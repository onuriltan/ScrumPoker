<template>
  <div>
    <div class="row ">
      <div class="col pb-4">
        <div style="font-size: 25px; text-align: right">
          Active Story: <b>{{currentStoryName}}</b>
        </div>
      </div>
    </div>
    <div class="row ">
      <div class="col">
        <StoryTable :storyList="this.storyList" :currentStory="this.currentStoryName"/>
      </div>
      <div class="col">
        <VotePanel :voter="voter" :storyName="currentStoryName" :pokerName="pokerName"/>
      </div>
    </div>
  </div>

</template>

<script>
  import pokerService from '../services/poker.service'
  import VotePanel from "../components/VotePanel";
  import StoryTable from "../components/StoryTable";

  export default {
    name: "TeamBoard",
    components: {VotePanel, StoryTable},
    data() {
      return {
        storyList: [],
        voter: "Voter",
        currentStoryName: '',
        pokerName: this.$route.params.pokerName
      }
    },
    mounted() {
      this.getStories()
      setInterval(this.getStories, 2000)
    },
    methods: {
      getStories() {
        pokerService.getStories(this.pokerName).then(res => {
          this.storyList = res.data
          this.currentStoryName = this.getCurrentStory().name
        }).catch(err => {
          console.log(err)
        })
      },
      getCurrentStory() {
        let currentStory = this.storyList.find(story => {
          return story.status === 'Not Voted'
        })
        if (currentStory) this.currentStoryName = currentStory.name
        else this.currentStoryName = this.storyList[this.storyList.length -1].name
      }
    }
  }
</script>
