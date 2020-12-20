<template>
  <nav>
    <div class="hr"></div>
    <ul>
      <router-link
        v-for="(item, index) in content"
        :to="'/' + index"
        :key="index"
        :title="item.title"
      >
        <li>
          <div class="img">
            <img :src="item.img" :alt="item.title" />
          </div>
          <span>الحلقه ({{ index + 1 }})</span>
          <span>{{ item.title }}</span>
        </li>
      </router-link>
    </ul>
  </nav>
</template>
<script>
export default {
  name: "NavComponent",
  props: ["content"]
};
</script>
<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Cairo:wght@200&display=swap");

nav {
  margin-top: 10px;

  .hr {
    height: 1px;
    background-image: linear-gradient(90deg, transparent, #868686, transparent);
  }

  ul {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;

    a {
      margin: 10px;
      width: 200px;
      box-shadow: 4px 10px 12px #ccc;
      border-radius: 5px;
      overflow: hidden;
      box-sizing: border-box;
      border: 2px solid transparent;

      li {
        padding: 5px;
        background: rgba(255, 255, 255, 0.623);
        text-align: right;

        .img {
          overflow: hidden;
          height: 130px;
          border-radius: 5px;
          position: relative;

          img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: 1s;
          }

          &::before {
            content: "";
            height: 30%;
            width: 100%;
            display: block;
            position: absolute;
            bottom: 0;
            left: 0;
            background: linear-gradient(transparent, black);
            opacity: 0.4;
            z-index: 1;
            transition: 0.5s;
          }

          &::after {
            content: "";
            width: 20px;
            height: 20px;
            background: #f2f2f2;
            clip-path: polygon(0px 3px, 12px 10px, 0px 17px);
            position: absolute;
            z-index: 1;
            left: 10px;
            bottom: -20px;
            transition: 0.5s;
          }
        }

        span {
          display: block;
          margin: 0rem 0 0;
          font-family: "Cairo", sans-serif;
          color: #212121;
          padding: 0 5px;
        }

        &:hover {
          img {
            transform: scale(1.2);
          }

          .img::after {
            bottom: 10px;
          }

          .img::before {
            opacity: 0.8;
          }
        }
      }

      &.router-link-active {
        border-color: #38c3e1;
        transition: 1s;
      }
    }
  }
}
</style>
