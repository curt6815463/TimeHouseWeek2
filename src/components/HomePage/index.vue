<template>
  <div>
    <div class="header">
      <div class="logo">
        HaveFun
      </div>
      <div class="explore">
        <font-awesome-icon class="search-icon" icon="search" />
        <input type="text" placeholder="Explore your own activities">
      </div>
    </div>
    <div class="content">
      <div class="sidebar">
        <div class="sidebar-container">

          <div class="sidebar-location">
            <div class="sidebar-title">
              Location
            </div>
            <select class="" name="">
              <option value="">Taiwan</option>
              <option value="">2</option>
              <option value="">3</option>
            </select>
          </div>

          <div class="sidebar-divider">
          </div>

          <div class="sidebar-date">
            <div class="sidebar-title">
              Date
            </div>
            <div class="sidebar-date-form">
              <div class="form-title">
                <label for="from">from</label>
                <label for="to">to</label>
              </div>
              <div class="form-input">
                <input type="text" name="from" value="">
                <input type="text" name="to" value="">
              </div>
            </div>
          </div>

          <div class="sidebar-divider">
          </div>

          <div class="categories">
            <div class="sidebar-title">
              Categories
            </div>
            <div class="sidebar-categories-form">
              <div>
                <input type="checkbox">
                <label for="">all</label>
              </div>
              <div>
                <input type="checkbox">
                <label for="">ca</label>
              </div>
              <div>
                <input type="checkbox">
                <label for="">b</label>
              </div>
              <div>
                <input type="checkbox">
                <label for="">a</label>
              </div>
              <div>
                <input type="checkbox">
                <label for="">ee</label>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="main">

      </div>
    </div>
  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'

export default {
  name: 'HelloWorld',
  components: {
    FontAwesomeIcon
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  @import './index.scss';
</style>
