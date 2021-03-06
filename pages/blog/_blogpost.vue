<template>
  <div>
    <Header :image="post.thumbnail">
      <h1 class="title">{{ post.title }}</h1>
      <h2 class="subtitle">{{ post.summary }}</h2>
    </Header>
    <div class="container content-container">
      <p class="description-tag">TL;DR:</p>
      <p class="description-text">{{post.description}}</p>
      <hr>
      <nuxtdown-body class="body" :body="post.body"/>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>
import Header from "~~/components/header.vue";
import Footer from "~~/components/footer.vue";
import Prism from "prismjs";

export default {
  components: {
    Header,
    Footer
  },
  // The head function generates all the metadata for the HTML's head
  head: function() {
    return {
      title: `${this.post.title}`,
      meta: [
        // SEO meta
        {
          hid: "description",
          name: "description",
          content: this.post.description
        },
        {
          hid: "keywords",
          name: "keywords",
          content: this.post.keywords
        },
        // Facebook's Open Graph Markup
        {
          hid: "og:url",
          property: "og:url",
          content: this.fullURL
        },
        {
          hid: "og:type",
          property: "og:type",
          content: "article"
        },
        {
          hid: "og:title",
          property: "og:title",
          content: this.post.title
        },
        {
          hid: "og:description",
          property: "og:description",
          content: this.post.description
        },
        {
          hid: "og:image",
          property: "og:image",
          content: this.imageURL
        },
        // Twitter's Card Markup
        {
          hid: "twitter:card",
          name: "twitter:card",
          content: "summary_large_image"
        },
        {
          hid: "twitter:title",
          name: "twitter:title",
          content: this.post.title
        },
        {
          hid: "twitter:description",
          name: "twitter:description",
          content: this.post.description
        },
        {
          hid: "twitter:image",
          name: "twitter:image",
          content: this.imageURL
        }
      ]
    };
  },
  // Nuxt method that uses the Nuxtdown content-api to load a post to render this page with it.
  asyncData: async ({ app, route, payload }) => {
    return {
      post: (await app.$content("/blog").get(route.path)) || payload
    };
  },
  // Once the component is mounted, we need to run Prism again to highlight the code blocks
  mounted() {
    Prism.highlightAll();
  },
  computed: {
    // imageURL will always return the correct URL for the thumbnail, both for development and once deployed
    imageURL: function() {
      return `${process.env.BASE_URL}${this.post.thumbnail}`;
    },
    // fullURL will always return the correct URL for the og:URL, both for development and once deployed
    fullURL: function() {
      return `${process.env.BASE_URL}${this.$route.path}/`;
    }
  }
};
</script>

<style>
.title {
  color: #ffffff;
  font-family: "Eczar";
  font-weight: 500;
  font-size: 48px;
  line-height: 64px;
}

.subtitle {
  color: #ffffff;
  font-family: "Work Sans";
  font-size: 21px;
  letter-spacing: -0.88px;
  line-height: 35px;
}

.content-container {
  max-width: 50%;
}

.description-tag {
  margin-top: 60px;
  color: #ff445b;
  font-family: "Work Sans";
  font-size: 14px;
  font-weight: 600;
  letter-spacing: 2.47px;
  line-height: 16px;
  margin-bottom: 20px;
}

.description-text {
  color: #000000;
  font-family: "Work Sans";
  font-size: 25px;
  letter-spacing: -0.88px;
  line-height: 37px;
  margin-bottom: 10px;
}

hr {
  border: 1px solid #e2e5ed;
  margin-top: 50px;
  margin-bottom: 50px;
}

.body a {
  color: #ff445b;
}
.body a:hover {
  color: #ff445b;
  text-decoration: underline;
}

.number {
  display: inline;
  padding: inherit;
  font-size: inherit;
  line-height: inherit;
  text-align: inherit;
  vertical-align: inherit;
  border-radius: inherit;
  font-weight: inherit;
  white-space: inherit;
  background: inherit;
  margin: inherit;
}

blockquote > p {
  display: inherit;
  padding: inherit;
  font-size: inherit;
  line-height: inherit;
  text-align: inherit;
  vertical-align: inherit;
  border-radius: inherit;
  font-weight: inherit;
  white-space: inherit;
  background: inherit;
  margin: inherit;
  margin-bottom: 4px !important;
  margin-block-start: inherit;
  margin-block-end: inherit;
  margin-inline-start: inherit;
  margin-inline-end: inherit;
  font-family: "Work Sans";
  font-size: 16px;
}

blockquote {
  margin-bottom: 20px !important;
  background: #f9f9f9;
  border-left: 10px solid #ccc;
  margin: 0.25em 10px;
  padding: 0.25em 10px;
}

p {
  color: #000000;
  font-family: "Work Sans";
  font-size: 21px;
  letter-spacing: -0.88px;
  line-height: 32px;
  margin-bottom: 25px;
}

h2 {
  color: #000000;
  font-family: "Eczar";
  font-weight: 500;
  font-size: 28px;
  line-height: 64px;
}

h3 {
  color: #000000;
  font-family: "Work Sans";
  font-weight: 600;
  font-size: 21px;
  line-height: 64px;
}

ul {
  list-style-type: disc;
  list-style-position: inside;
  margin-left: 15px;
  margin-bottom: 25px;
}

p ~ pre {
  margin-top: -20px !important;
  margin-bottom: 14px !important;
}

@media screen and (max-width: 1087px) {
  .navbar-menu {
    background-color: rgba(0, 0, 0, 0);
  }

  .hero-body {
    padding-bottom: 12px;
  }

  .title {
    font-size: 30px;
    line-height: 36px;
  }

  .subtitle {
    padding-top: 15px;
    font-size: 22px;
    line-height: 30px;
  }

  .content-container {
    max-width: 90%;
  }

  .description-text {
    font-size: 20px;
    line-height: 30px;
  }

  p {
    font-size: 18px;
  }
}

@media screen and (min-width: 1088px) and (max-width: 1280px) {
  .content-container {
    max-width: 75%;
  }
}
</style>
