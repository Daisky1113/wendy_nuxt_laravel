<template>
  <div>
    <div>
      <span @click="$router.push('/')" style="cursor: pointer" v-text="title"></span>
      <button v-if="!userName" @click="$router.push('/user/login')">ログイン・新規登録</button>
      <button v-if="userName" @click="logout">ログアウト</button>
    </div>
    <main>
      <div class="maypage-menu">
        <h1>マイページ</h1>
          <p>ユーザーネーム：{{ userName }}</p>
          <p><nuxt-link :to="`/user/mypage-favorite`">マイページ-お気に入り店舗一覧</nuxt-link></p>
          <p><nuxt-link :to="`/user/mypage-info`">マイページ-お知らせ</nuxt-link></p>
          <p><nuxt-link :to="`/user/mypage-history`">マイページ-利用履歴</nuxt-link></p>
          <p><nuxt-link :to="`/user/mypage-setting`">マイページ-メール＆パスワード再設定</nuxt-link></p>
          <button v-if="userName" @click="logout">ログアウト</button>
      </div>
      <div>
        <nuxt />
      </div>
    </main>
    <footer>
      <span>&copy; WENDY {{ new Date().getFullYear() }}</span>
    </footer>
  </div>
</template>

<script>
//firebaseの初期化の部分のインストール
import firebase from '../plugins/firebase'

import { mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'WENDY'
    }
  },
    created() {
    // onAuthStateChangedは引数に認証の状態が変わった時に呼び出されるコールバック関数を受け取る
    // ログイン・ログアウトの際に引数の関数が呼ばれ、ログイン時にはユーザーのオブジェクトが渡ってくる、ログアウトのときはnullが渡ってくる
    firebase.auth().onAuthStateChanged(user => {
      if (user) {
        const { uid, email, displayName } = user
        this.setLoginUser({ uid, email, displayName })
        if(this.$router.currentRoute.name === 'user-login') this.$router.push({ name: 'user-mypage-favorite' })
      } else {
        this.deleteLoginUser()
        this.$router.push({ name: 'index' })
      }
    })
  },
  computed: {
    ...mapGetters('auth', ['userName'])
  },
  methods: {
    ...mapActions('auth', ['setLoginUser', 'logout', 'deleteLoginUser'])
  }
}
</script>

<style lang="scss">
.maypage-menu {
  background-color: #EB5276;
  height: 344px;
  h1,h2 {
    text-align: center;
    padding: 1rem;
  }
}

</style>