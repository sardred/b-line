<template>
  <div class="wrapper">
    <error-container></error-container>
    <div class="container">
      <h2>Favorites</h2>
      <favorite-field
        v-for="(field, index) in fields"
        ref="fields"
        :key="index"
        :name="field.name"
        :icon="field.icon"
        :value="field.value"
        :updateData="updateData"
      />
      <div v-if="showSuccess">
        <p>Succesfully saved!</p>
      </div>
      <button @click="save">Save</button>
    </div>
  </div>
</template>

<script>
import homeIcon from "@/assets/fav_home.svg";
import workIcon from "@/assets/fav_work.svg";
import schoolIcon from "@/assets/fav_school.svg";
import newIcon from "@/assets/fav_new.svg";
import ErrorContainer from "@/components/ErrorContainer.vue";
import FavoriteField from "@/components/FavoriteField.vue";

export default {
  components: {
    ErrorContainer,
    FavoriteField
  },
  data() {
    return {
      fields: [
        {
          name: "home",
          icon: homeIcon,
          value: this.$root.$data.favorites["home"]
            ? this.$root.$data.favorites["home"].verbose
            : ""
        },
        {
          name: "work",
          icon: workIcon,
          value: this.$root.$data.favorites["work"]
            ? this.$root.$data.favorites["work"].verbose
            : ""
        },
        {
          name: "school",
          icon: schoolIcon,
          value: this.$root.$data.favorites["school"]
            ? this.$root.$data.favorites["school"].verbose
            : ""
        },
        {
          name: "new",
          icon: newIcon,
          value: this.$root.$data.favorites["new"]
            ? this.$root.$data.favorites["new"].verbose
            : ""
        }
      ],
      showSuccess: false
    };
  },
  methods: {
    save() {
      this.$refs.fields.forEach(field => {
        if (field.verbose) {
          field.setFavorite();
        }
      });
      this.showSuccess = true;
      var vm = this;
      setTimeout(function() {
        vm.showSuccess = false;
      }, 5 * 1000);
    },
    updateData(name, value) {
      this.fields.forEach(field => {
        if (field.name === name) {
          field.value = value;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import "@/common/constants.scss";
@import "@/common/card.scss";

.wrapper {
  @extend %card;
}
</style>
