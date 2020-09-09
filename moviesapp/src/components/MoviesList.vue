<template>
  <BContainer>
    <h3 class="list-title">{{ listTitle }}</h3>
    <BRow>
      <template v-if="isExist">
        <BCol cols="3" v-for="(movie, key) in list" :key="key">
          <MovieItem
            :movie="movie"
            @mouseover.native="onMouseOver(movie.Poster)"
            @removeItem="onRemoveItem" @showInfo="onShowInfo"
          />
        </BCol>
      </template>
      <template v-else>
        <div>Emty list</div>
      </template>
    </BRow>
    <BModal id="movie-info" size="xl" hide-header ok-only ok-title="Close">
      <Description v-if="selectedMovieData" :movie="selectedMovieData" @closeInfo="onCloseInfo" />
    </BModal>
  </BContainer>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import MovieItem from "./MovieItem";
import Description from "./Description";

export default {
  name: "MoviesList",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  components: {
    MovieItem,
    Description
  },
  computed: {
    ...mapGetters("movies", ["isSearch"]),
    isExist() {
      return Boolean(Object.keys(this.list).length);
    },
    listTitle() {
      return this.isSearch ? "Search result" : "IMDB Top 250";
    }
  },
  methods: {
    ...mapActions("movies", ["removeMovie"]),
    ...mapActions(["showNotify"]),
    onMouseOver(poster) {
      this.$emit("changePoster", poster);
    },
    async onRemoveItem({ id, title }) {
      const isConfirmed = await this.$bvModal.msgBoxConfirm(`Do you want to delete ${title}?`);

      if(isConfirmed) {
        this.removeMovie(id);
        this.showNotify({
          msg: "The movie deleted successfully",
          variant: "success",
          title: "Success"
        })
      }
    },
    onShowInfo(id) {
      this.selectedMovieData = this.list[id];
      this.$bvModal.show("movie-info");
    },
    onCloseInfo() {
      this.selectedMovieData = null;
      this.$bvModal.hide("movie-info");
    }

  }
};
</script>

<style scoped>
.list-title {
  font-size: 50px;
  margin-bottom: 30px;
  color: #fff;
}
</style>
