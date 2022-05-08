
<template>
  <header>
    <div class="container">

      <div class="title">
        SITCON 2022 Issue 小精靈
      </div>
      <p>
        幫你開 Issue 小精靈
      </p>
    </div>
  </header>
  <div class="container">

    <div class="box">
      <div class="title">
        小提醒
      </div>
      <p>
        在 Title 或 Description 輸入 #{group} 會自動帶入組別名稱。
      </p>
    </div>
    <label>Title</label>
    <input v-model="title" @input="updateLinks" />

    <label>Description</label>
    <v-md-editor v-model="description" height="400px" @change="updateLinks"></v-md-editor>


    <button @click="createIssue" class="magic-button">🪄 來點魔法！</button>
    <p class="text-center">若魔法施展失敗，請檢查瀏覽器是否封鎖了快顯視窗，或直接點擊下方連結。</p>
    <div class="links">
      <a v-for="link in links" :href="link.href" target="_blank" :key="link.title">{{ link.title }}</a>
    </div>
    <footer>
      Developed by <a href="https://gnehs.net" target="_blank">勝勝寶寶</a>
    </footer>
  </div>
</template>

<style lang="sass">
body,html,.v-md-textarea-editor pre, .v-md-textarea-editor textarea,.vuepress-markdown-body
  font-family: 'Ubuntu Mono', 'Noto Sans TC', sans-serif !important
* 
  box-sizing: border-box
.text-center
  text-align: center
.container 
  max-width: 960px
  margin: 0 auto
  font-size: 16px
  line-height: 1.5
 
.title
  font-size: 1.25rem
  font-weight: bold
  color: blue
  margin-bottom: 8px
p
  font-size: 1rem
  opacity: 0.5
  margin: 0
header
  padding: 64px 0
  margin-bottom: 16px
  background-color: #fafafa
  .title
    font-size: 2rem
footer
  font-size: 0.875rem
  margin: 32px 0
  text-align: center
  color: #999

.box
  padding: 16px 
  margin-bottom: 16px
  border: 1px solid #f0f0f0
  border-radius: 4px
label
  display: inline-block
  font-size: 1rem 
  margin-top: 16px
  margin-bottom: 8px  
  color: #666
input
  width: 100%
  padding: 8px
  border: 1px solid #f0f0f0
  border-radius: 4px
  font-size: 1.2rem
  &:focus
    outline: none
    border: 1px solid blue
.links
  display: flex
  flex-wrap: wrap
  justify-content: center
  align-items: center
  gap: 16px 
  a
    color: #666
.magic-button
  background-color: #0000ff
  color: #fff
  font-size: 2rem
  padding: 8px 16px
  display: block
  margin: 8px auto
  margin-top: 32px
  border: none
  border-radius: 4px
  cursor: pointer
  &:hover
    background-color: #0000cc
  &:active
    background-color: #000099

</style>
<script>

export default {
  data() {
    return {
      title: '[#{group}] 填寫蓬蓬鬆餅預約表單',
      description: '請#{group}組協助填寫蓬蓬鬆餅預約表單，謝謝！\n\n不支援上傳圖片，若你要插入圖片，請先上傳在其他地方再以 markdown 格式插入。',
      groupList: [
        '議程',
        '總召',
        '編輯',
        '製播',
        '開發',
        '設計',
        '財務',
        '紀錄',
        '行政',
        '行銷',
        '場務',
      ],
      links: []
    }
  },
  mounted() {
    this.updateLinks()
  },
  methods: {
    updateLinks() {
      this.links = []
      for (let group of this.groupList) {
        let title = this.title.replaceAll('#{group}', group)
        let description = this.description.replaceAll('#{group}', group)
        description = `/label "Status::Inbox" /label "組別::${group}組"\n` + description
        let link = new URL('https://gitlab.com/sitcon-tw/2022/2022-board/-/issues/new')
        link.searchParams.append('issue[title]', title)
        link.searchParams.append('issue[description]', description)
        this.links.push({ title: group, href: link.href })
      }
    },
    createIssue() {
      for (let link of this.links) {
        window.open(link.href)
      }
    }
  }
}
</script>
