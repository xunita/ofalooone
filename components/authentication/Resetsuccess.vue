<template>
  <div class="py-3">
    <div class="w-full flex flex-col">
      <div class="oath-log flex flex-col space-y-10">
        <div
          class="border bg-white rounded px-5 sm:px-8 py-5 flex flex-col space-y-2"
        >
          <div v-if="okay" class="w-full top-0 appearZ pt-2">
            <span
              class="block rounded border border-green-600 text-green-600 text-c py-1 px-10 text-white font-semibold size-12"
              >Mot de passe réinitialisé avec success √</span
            >
          </div>
          <div v-if="err" class="w-full top-0 appearZ pt-2">
            <span
              class="block rounded border border-red-600 text-red-600 text-c py-1 px-10 text-white font-semibold size-12"
              >Désolé, une erreur est survenue</span
            >
          </div>
          <div v-if="ex" class="w-full top-0 appearZ pt-2">
            <span
              class="block rounded border border-red-600 text-red-600 text-c py-1 px-10 text-white font-semibold size-12"
              >Lien expiré</span
            >
          </div>
          <div v-if="den" class="w-full top-0 appearZ pt-2">
            <span
              class="block rounded border border-red-600 text-red-600 text-c py-1 px-10 text-white font-semibold size-12"
              >Accès non authorisé</span
            >
          </div>
          <div v-if="saming" class="w-full top-0 appearZ pt-2">
            <span
              class="block rounded border border-red-600 text-red-600 text-c py-1 px-10 text-white font-semibold size-12"
              >Vous avez déja utilisé ce mot de passe</span
            >
          </div>
          <h4 class="size-16 logo-color font-semibold mb-2">
            Definir un nouveau mot de passe
          </h4>
          <div class="w-full">
            <div class="flex align-center justify-between">
              <label for="pwd" class="size-14">Nouveau Mot de passe</label>
            </div>

            <div class="relative flex align-center">
              <input
                id="pwd"
                v-model="pwd"
                :type="pwdhidden ? 'password' : 'text'"
                class="border pr-8 w-full py-1 h-7 size-14 rounded no-outlines outline-none px-2"
                :class="{
                  'border-red-700': passerror,
                  'border-green-700': $linker.pwdValidated(pwd),
                }"
              />
              <button
                class="bg-transparent no-outlines absolute right-0 mr-2 logo-color"
                @click="
                  {
                    pwdhid = !pwdhid
                  }
                "
              >
                <svg
                  v-show="pwdhidden"
                  class="w-5 h-5"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="1"
                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                  ></path>
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="1"
                    d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                  ></path>
                </svg>
                <svg
                  v-show="!pwdhidden"
                  class="w-5 h-5"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="1"
                    d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21"
                  ></path>
                </svg>
              </button>
            </div>
            <p
              v-show="passerror"
              class="size-12 appearZ text-red-700 leading-4 pt-1"
            >
              Le mot de passe doit contenir au moins 8 caractères avec lettres &
              chiffres
            </p>
          </div>
          <div class="w-full">
            <div class="flex align-center justify-between">
              <label for="pwdx" class="size-14"
                >Nouveau Mot de passe confirmation</label
              >
            </div>
            <input
              id="pwdx"
              v-model="pwdcf"
              :type="pwdhidden ? 'password' : 'text'"
              class="border pr-8 w-full py-1 h-7 size-14 rounded no-outlines outline-none px-2"
              :class="{
                'border-red-700s':
                  didpwdunmatch ||
                  ($linker.pwdValidated(pwd) &&
                    pwdcf.length >= pwd.length &&
                    !samepwd) ||
                  ($linker.pwdValidated(pwd) &&
                    pwdcf.length > 0 &&
                    pwdcf.length < pwd.length &&
                    !pwd.startsWith(pwdcf)),
                'border-green-700': samepwd,
              }"
            />
            <p
              v-show="
                ($linker.pwdValidated(pwd) &&
                  pwdcf.length >= pwd.length &&
                  !samepwd) ||
                didpwdunmatch ||
                ($linker.pwdValidated(pwd) &&
                  pwdcf.length > 0 &&
                  pwdcf.length < pwd.length &&
                  !pwd.startsWith(pwdcf))
              "
              class="size-12 appearZ text-red-700 leading-4 pt-1"
            >
              Le mot de passe et la confirmation ne correspondent pas
            </p>
          </div>
          <a
            class="button block btn-008489 border rounded-md flex align-center space-x-2 relative top-05x bottom-0x"
            :class="{ noclick: ding || okay }"
            @click="reinit"
          >
            <span
              class="size-13 flex items-center space-x-2 text-white font-semibold"
              ><span class="size-13 text-white font-semibold"
                >Réinitialiser le mot de passe</span
              >
              <span v-show="ding" class="w-fit h-fit"
                ><i class="animate-spin fas fa-circle-notch color-white"></i
              ></span> </span
          ></a>
          <div class="w-fit m-0-auto pt-3">
            <div class="flex align-center text-center space-x-2 size-13 mt-5">
              <a href="/connexion" class="color-008489 underline-hover"
                >Se connecter</a
              >
              <span>|</span>
              <a href="/inscription" class="color-008489 underline-hover"
                >Créer un compte</a
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      pwd: '',
      pwdcf: '',
      pwdandcf: false,
      user: '',
      email: '',
      alinux: '',
      pwderr: false,
      pwdcferr: false,
      maierror: false,
      pwdhid: true,
      notpwdok: false,
      phone: '',

      ok: false,
      error: false,
      exp: false,
      denied: false,
      pwding: false,
      same: false,
    }
  },
  computed: {
    didpwdunmatch() {
      return this.notpwdok === true
    },
    samepwd() {
      return (
        this.$linker.pwdValidated(this.pwd) &&
        this.$linker.pwdValidated(this.pwdcf) &&
        this.pwd === this.pwdcf
      )
    },
    pwdhidden() {
      return this.pwdhid === true
    },
    passcferror() {
      return this.pwdcferr === true
    },
    passerror() {
      return this.pwderr === true
    },
    mailerror() {
      return this.maierror === true
    },
    okay() {
      return this.ok === true
    },
    saming() {
      return this.same === true
    },
    err() {
      return this.error === true
    },
    ex() {
      return this.exp === true
    },
    den() {
      return this.denied === true
    },
    ding() {
      return this.pwding === true
    },
  },
  watch: {
    email() {
      if (this.mailerror) {
        this.maierror = false
      }
    },
    pwd() {
      this.ok = false
      this.error = false
      this.exp = false
      this.denied = false
      this.same = false
      if (this.passerror) {
        this.pwderr = false
      }
      this.notpwdok = false
    },
    phone(newval, oldval) {
      if (isNaN(newval) || newval.toString().includes('.')) {
        this.phone = oldval
      } else this.phone = newval
    },
    pwdcf(newval, oldval) {
      this.ok = false
      this.error = false
      this.exp = false
      this.denied = false
      this.same = false
      if (newval.length <= this.pwd.length && this.didpwdunmatch) {
        this.notpwdok = false
      }
    },
  },
  beforeMount() {
    if (sessionStorage.new) {
      this.email = sessionStorage.getItem('new')
      this.alinux = sessionStorage.getItem('ofaloo_h')
      console.log(this.email, this.alinux)
    } else {
      this.$router.replace('/')
    }
  },
  methods: {
    infosValidated() {
      if (this.$linker.pwdValidated(this.pwd)) {
        this.pwderr = false
      } else this.pwderr = true

      if (this.$linker.pwdValidated(this.pwd) && this.samepwd) {
        this.notpwdok = false
      } else if (this.$linker.pwdValidated(this.pwd) && !this.samepwd)
        this.notpwdok = true

      return (
        this.passerror === false &&
        this.passcferror === false &&
        this.samepwd === true
      )
    },
    async verification() {
      return await new Promise((resolve, reject) => {
        resolve(
          this.$axios.$get(
            'reset-pwd/' + this.email + '/' + this.alinux + '/' + this.pwd
          )
        )
      }).catch(() => {
        console.error("Oops, can't resolve your promise getting hash")
      })
    },
    modify() {
      this.ok = false
      this.error = false
      this.exp = false
      this.denied = false
      this.same = false
      this.verification()
        .then((result) => {
          console.log(result)
          if (result.status === '200') {
            this.ok = true
            this.pwding = false
            setTimeout(() => {
              location.assign('/connexion')
            }, 1000)
          }
          if (result.status === '302') {
            this.same = true
            this.pwding = false
          }
          if (result.status === '500') {
            this.error = true
            this.pwding = false
          }
          if (result.status === '1997' || result.status === '404') {
            this.exp = true
            this.pwding = false
            setTimeout(() => {
              location.assign('/')
            }, 1000)
          }
          if (result.status === '401') {
            this.denied = true
            this.pwding = false
            if (this.$auth.loggedIn) {
              this.$auth.logout().then((res) => {
                if (localStorage.hdzd) localStorage.removeItem('hdzd')
                location.reload()
              })
            } else location.assign('/')
          }
        })
        .catch(() => {
          this.error = true
          this.pwding = false
        })
    },
    reinit() {
      this.pwding = true
      if (this.infosValidated()) {
        this.modify()
      } else {
        this.pwding = false
      }
    },
  },
}
</script>
