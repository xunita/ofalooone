<template>
  <div
    class="py-8"
    :class="{
      'px-20': size >= 1230,
      'px-31': size > 999 && size < 1150,
      'px-30': size > 690 && size < 830,
      'px-15': size > 500 && size <= 690,
      'px-0 bg-white': size <= 500,
    }"
  >
    <div
      class=""
      :class="{
        'flex space-x-10': size >= 1150,
        '': size < 1150,
      }"
    >
      <div
        class="flex w-fulls flex-col space-y-6"
        :class="{
          '': size < 1150,
        }"
      >
        <div
          class="flex w-fulls px-3 py-2 bg-white h-16min"
          :class="{
            'justify-between space-x-2': size >= 450,
            'flex-col space-y-3': size < 450,
            'border-t border-b': size <= 500,
            rounded: size > 500,
          }"
        >
          <div>
            <div
              class="logo-color block sm:size-15 size-14 font-semibold mb-0.5"
            >
              <div>
                <h4 v-if="fifi === 'Acheter'">
                  <span
                    v-if="
                      $route.query.type !== null &&
                      $route.query.type !== undefined
                    "
                    class="inline-block"
                    >{{ $route.query.type }},&nbsp;</span
                  >Acheter des proriétés<span
                    v-if="search !== ''"
                    class="logo-color"
                    >, resultat de "{{ search }}"</span
                  >
                </h4>
                <h4 v-else-if="fifi === 'Louer'">
                  <span
                    v-if="
                      $route.query.type !== null &&
                      $route.query.type !== undefined
                    "
                    class="inline-block"
                    >{{ $route.query.type }},&nbsp;</span
                  >Louer des proriétés<span
                    v-if="search !== ''"
                    class="logo-color"
                    >, resultat de "{{ search }}"</span
                  >
                </h4>
                <h4 v-else>
                  <span
                    v-if="
                      $route.query.type !== null &&
                      $route.query.type !== undefined
                    "
                    class="inline-block"
                    >{{ $route.query.type }},&nbsp;</span
                  >Acheter et louer des proriétés<span
                    v-if="search !== ''"
                    class="logo-color"
                    >, resultat de "{{ search }}"</span
                  >
                </h4>
              </div>
            </div>
            <div v-if="!moring || charged">
              <span v-if="toting > 1" class="logo-color size-14"
                >1-{{ propLength }} sur {{ toting }} résultats</span
              >
              <span v-else-if="toting === 1" class="logo-color size-14"
                >1 résultat</span
              >
              <span v-else class="logo-color size-14">0 résultat</span>
            </div>
          </div>
          <div
            class="pb-1"
            :class="{
              'self-end ': size >= 450,
            }"
          >
            <sortres @sort="sorting"></sortres>
          </div>
        </div>
        <filters
          v-if="size < 1150"
          :class="{
            'border-t border-b': size <= 500,
            rounded: size > 500,
          }"
          class="w-fulls"
          @go="go"
          @going="going"
          @finished="finished"
          @resetdone="resetdone"
        ></filters>
        <div
          v-if="charged && $route.query.ofloowa !== undefined && propLength > 0"
          class="flex w-fulls align-center space-x-2 justify-between px-3 py-2 bg-008489"
          :class="{
            rounded: size > 500,
          }"
        >
          <div class="flex align-center space-x-5 w-fits">
            <a class="flex w-fits align-center space-x-2"
              ><img
                class="rounded-full is-40x40 border self-startes"
                :src="'https://ofaloo.blob.core.windows.net/ofaloo/' + user_pic"
                alt="Image"
              />
              <span class="size-14 block font-semibold text-white w-fits">{{
                user_name
              }}</span></a
            >
            <a
              v-if="supery"
              href="/recherche/?q=super-agent"
              class="button w-fits block bg-transparent px-3 h-fit rounded border-white size-12 text-white"
              >Super agent</a
            >
          </div>
          <button
            class="delete search text-white"
            aria-label="close"
            @click="gotorecherche"
          ></button>
        </div>
        <div
          v-if="
            charged &&
            $route.query.q !== undefined &&
            $route.query.q === 'super-agent' &&
            propLength > 0
          "
          class="flex w-fulls align-center justify-between px-3 py-2 bg-008489"
          :class="{
            rounded: size > 500,
          }"
        >
          <div class="flex align-center space-x-2">
            <span class="text-white">Rechercher des propriétés de </span>
            <a
              class="button cursor-default block bg-transparent px-3 h-fit rounded border-white size-12 text-white"
              >Super agent</a
            >
          </div>
          <button
            class="delete search text-white"
            aria-label="close"
            @click="gotorecherche"
          ></button>
        </div>

        <p v-if="has_error" class="h-fit w-fit h-centers mt-5">
          Oops, erreur pendant le chargement des propriétés. Veuillez recharger
          la page !
        </p>
        <p v-else-if="!has_data" class="h-fit w-fit h-centers mt-5">
          Chargement des propriétés...
        </p>
        <div v-else-if="dataOk" class="w-fulls">
          <div
            class="flex flex-col w-fulls"
            :class="{
              'space-y-2': size <= 499,
              'space-y-8': size > 499,
            }"
          >
            <homeproplarge
              v-for="(property, i) in properties.data"
              :key="i"
              :property="property"
            ></homeproplarge>
          </div>
          <div
            v-if="propLength < toting"
            class="pt-10 w-fulls"
            :class="{
              'px-2.5': size <= 500,
            }"
          >
            <p v-if="moring" class="h-fit w-fit h-centers mb-4">
              Chargement des propriétés...
            </p>
            <div class="w-full" :class="{ noclick: moring }">
              <a
                class="button block w-fit m-0-auto bg-white px-5 py-2 rounded border-008489ss size-14 color-008489"
                @click="next"
                >Plus de propriétés ({{ toting - propLength }})</a
              >
            </div>
          </div>
          <p v-else class="h-fit w-fit h-centers mt-5">
            --- Fin des résultats ---
          </p>
        </div>
        <p v-else class="h-fit w-fit h-centers mt-5">
          Oops désolé, aucune propriété ne correspond à votre description
        </p>
        <!-- <div class="can-add-ads"></div> -->
      </div>
      <filters
        v-show="size >= 1150"
        @go="go"
        @going="going"
        @finished="finished"
        @resetdone="resetdone"
      ></filters>
    </div>
    <div v-show="recently" class="px-4 pt-1 pb-1.5 bg-white rounded mt-20">
      <sameagent :title="'Récemment consultées'"></sameagent>
    </div>
    <div v-show="villing" class="px-4 pt-1 pb-1.5 bg-white rounded mt-10">
      <sameagents
        v-show="okay"
        :title="'Propriétés à'"
        :ville="ipadresse !== null ? ipadresse.city : 'unknown'"
        @loaded="datating"
      ></sameagents>
    </div>
  </div>
</template>

<script>
import Sortres from '~/components/dropdown/Sortres.vue'
import Filters from '~/components/filter/Filters.vue'
// import Pagination from '~/components/pagination/Pagination.vue'
import Homeproplarge from '~/components/propriete/Homeproplarge.vue'
import Sameagent from '~/components/Sameagent.vue'
import Sameagents from '~/components/Sameagents.vue'
export default {
  components: { Sortres, Homeproplarge, Filters, Sameagent, Sameagents },
  middleware: 'recherche',
  data() {
    return {
      ipadresse: null,
      ok: false,
      recent: false,
      ville: false,
      filter: {
        what: 'all',
        achat_location: {
          multiple: ['Tous types'],
        },
        part: {
          tous: 'Toute part',
        },
        type_loc: {
          tous: 'Tous types',
        },
        achat: {
          type: "Tous types d'achats",
          multiple: [],
        },
        rent: {
          type: 'Tous types de locations',
          multiple: [],
        },
        property: {
          tous: 'Tous types de propriétés',
          multiple: [],
          tous_search: 'Tous types',
        },
        bed: {
          tous: 'Tous types de pièces',
          tous_search: 'Tous types',
        },
        bath: {
          tous: 'Tous types',
        },
        search_price: {
          tous: 'Tout prix',
          min: 0,
          max: 0,
        },
        price: {
          tous: 'Tout prix',
          min: 0,
          max: 0,
        },
        price_loc: {
          tous: 'Tout prix',
          min: 0,
          max: 0,
        },
        taille: {
          tous_search: 'Tous types',
          tous: 'Tous types de tailles',
          min: 0,
          max: 0,
        },
        garage: {
          tous_search: 'Tous types',
          tous: 'Tous types de garages',
        },
        date: {
          tous: 'Tous temps',
          date: null,
        },
        availability: {
          tous: 'Tous types',
          multiple: [],
        },
        indoor: {
          multiple: [],
        },
        outdoor: {
          multiple: [],
        },
        energy: {
          multiple: [],
        },
      },
      properties: {
        data: [],
      },
      pagedata: false,
      current: 1,
      total: 0,
      sort: 'Le plus pertinent',
      q: null,
      s: null,
      ofloowa: null,
      loaded: false,
      search: '',
      charged: false,
      more: false,
      error: false,
      first: false,
      fromPT: false,
    }
  },
  head() {
    return {
      title:
        this.search !== ''
          ? 'Rechercher des propriétés, resultat(s) de ' +
            this.search +
            ' | Ofaloo'
          : 'Rechercher des propriétés | Ofaloo',
    }
  },
  computed: {
    dataOk() {
      return this.properties !== undefined && this.properties.data !== undefined
        ? this.properties.data.length > 0
        : false
    },
    recently() {
      return this.recent === true
    },
    villing() {
      return this.ville === true
    },
    okay() {
      return this.ok === true
    },
    propLength() {
      return this.properties !== undefined && this.properties.data !== undefined
        ? this.properties.data.length
        : 0
    },
    user_pic() {
      return this.properties !== undefined &&
        this.properties.data !== undefined &&
        this.properties.data.length > 0
        ? this.properties.data[0].user_pic
        : 'default/user/user.png'
    },
    user_id() {
      return this.properties !== undefined &&
        this.properties.data !== undefined &&
        this.properties.data.length > 0
        ? this.properties.data[0].property.user_id
        : 0
    },
    user_name() {
      return this.properties !== undefined &&
        this.properties.data !== undefined &&
        this.properties.data.length > 0
        ? this.properties.data[0].agence.name
        : 'Agent'
    },
    supery() {
      return (
        this.properties !== undefined &&
        this.properties.data !== undefined &&
        this.properties.data.length > 0 &&
        this.properties.data[0].agence.super === 'yes'
      )
    },
    size() {
      return this.$store.state.size
    },
    filters() {
      return this.filter
    },
    has_load() {
      return this.loaded === true
    },
    moring() {
      return this.more === true
    },
    has_data() {
      return this.charged === true
    },
    has_error() {
      return this.error === true
    },
    pipting() {
      return this.pagination
    },
    curting() {
      return this.current
    },
    fifi() {
      return this.filter !== undefined ? this.filter.what : ''
    },
    toting() {
      return this.total
    },
  },
  watch: {
    has_load(nv, ov) {
      if (nv) {
        this.searching()
      }
    },
  },
  beforeMount() {
    this.getIP().then(() => {
      if (this.ipadresse !== null && this.ipadresse !== undefined) {
        this.getPropVille()
      } else {
        console.log('perm denied')
      }
    })
    // this.getPropVille()
    if (localStorage.viewed) {
      this.recent = true
    } else {
      this.recent = false
    }
    if (sessionStorage.sort) {
      this.sort = sessionStorage.getItem('sort')
    }
    this.fill()
    this.getproperties()
    console.log(this.$route.query.q, this.$route.query.ofloowa)
  },
  methods: {
    datating() {
      this.ok = true
    },
    async getPropVille() {
      // console.log(this.ipadresse.city)
      try {
        const result = await this.$axios.$get(
          (this.$auth.loggedIn ? 'aproperties' : 'properties') +
            '/villesfirst/' +
            this.ipadresse.city +
            '/-4'
        )
        if (result > 0) {
          this.ville = true
        }
      } catch (error) {
        console.log(error)
      }
    },
    async getIP() {
      try {
        const result = await fetch(
          'https://ipinfo.io/?token=c2507a294b3386'
        ).then((res) => res.json())
        this.ipadresse = result
      } catch (error) {
        this.ipadresse = null
        console.log("Can't read ipadresse")
      }
    },
    async searching() {
      this.charged = false
      if (sessionStorage.search) {
        this.search = sessionStorage.getItem('search')
      }
      if (sessionStorage.filter) {
        this.filter = await JSON.parse(sessionStorage.getItem('filter'))
      }
      if (isNaN(this.filter.part.tous)) {
        if (this.filter.part.tous.includes('Toute part')) {
          this.filter.part.tous = -1
        } else {
          this.filter.part.tous = +this.filter.part.tous.substr(
            0,
            this.filter.part.tous.indexOf('%')
          )
        }
      }
      if (isNaN(this.filter.bed.tous_search)) {
        if (this.filter.bed.tous_search.includes('Tous types')) {
          this.filter.bed.tous_search = -1
        } else {
          this.filter.bed.tous_search = +this.filter.bed.tous_search.substr(
            0,
            this.filter.bed.tous_search.indexOf(' ')
          )
        }
      }
      if (isNaN(this.filter.bath.tous)) {
        if (this.filter.bath.tous.includes('Tous types')) {
          this.filter.bath.tous = -1
        } else {
          this.filter.bath.tous = +this.filter.bath.tous.substr(
            0,
            this.filter.bath.tous.indexOf(' ')
          )
        }
      }
      if (isNaN(this.filter.garage.tous_search)) {
        if (this.filter.garage.tous_search.includes('Tous types')) {
          this.filter.garage.tous_search = -1
        } else {
          this.filter.garage.tous_search = +this.filter.garage.tous_search.substr(
            0,
            this.filter.garage.tous_search.indexOf(' ')
          )
        }
      }
      this.apply()
        .then((res) => {
          // this.properties = res
          this.properties.data = res.data
          if (res.meta !== undefined) this.total = res.meta.total
          else this.total = 0
          this.error = false
          this.charged = true
          this.first = true
          this.fromPT = false
          // console.log(res)
        })
        .catch(() => {
          this.error = true
          this.charged = true
          this.first = true
          this.fromPT = false
        })
    },
    fill() {},
    getproperties() {},
    finished(val) {
      this.loaded = val
    },
    go() {
      this.searching()
    },
    going(filter) {
      this.searching()
    },
    sorting(val) {
      this.sort = val
      if (sessionStorage.sort) sessionStorage.removeItem('sort')
      sessionStorage.setItem('sort', this.sort)
      if (this.first) {
        this.apply()
          .then((res) => {
            // this.properties = res
            this.properties.data = res.data
            this.total = res.meta.total
            this.error = false
            this.charged = true
          })
          .catch(() => {
            this.error = true
            this.charged = true
          })
      }
    },
    paging(val) {
      this.current = val
    },
    resetdone() {
      sessionStorage.removeItem('filter')
      sessionStorage.removeItem('search')
      sessionStorage.removeItem('txt')
      sessionStorage.removeItem('sort')
      location.assign('/recherche')
    },
    gotorecherche() {
      location.assign('/recherche')
    },
    next() {
      this.applyGinate()
        .then((res) => {
          // this.properties = res
          res.data.forEach((element) => {
            this.properties.data.push(element)
          })
          this.error = false
          this.more = false
        })
        .catch(() => {
          this.error = true
          this.more = false
        })
    },
    async apply() {
      this.current = 1
      this.charged = false
      return await new Promise((resolve, reject) => {
        resolve(
          this.$axios.$post(
            (this.$auth.loggedIn ? 'aproperties' : 'properties') + '/search',
            {
              filter: this.filter,
              search: this.search,
              sort: this.sort,
              q: this.$route.query.q,
              ofloowa: this.$route.query.ofloowa,
            }
          )
        )
      }).catch(() => {
        this.error = true
        this.charged = true
        console.error("Oops, can't resolve your promise searching")
      })
    },
    async applyGinate() {
      this.current++
      this.more = true
      return await new Promise((resolve, reject) => {
        resolve(
          this.$axios.$post(
            (this.$auth.loggedIn ? 'aproperties' : 'properties') +
              '/search?page=' +
              this.current,
            {
              filter: this.filter,
              search: this.search,
              sort: this.sort,
              q: this.$route.query.q,
              ofloowa: this.$route.query.ofloowa,
            }
          )
        )
      }).catch(() => {
        this.error = true
        this.more = false
        console.error("Oops, can't resolve your promise searching")
      })
    },
  },
}
</script>

<style scoped></style>
