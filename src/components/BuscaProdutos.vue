<template>
  <div>
    <header class="container-fluid">
      <div class="row background-top">
        <div class="col-12 titles-page">
          <h3>uma seleção de produtos</h3>
          <h1>especial para você</h1>
          <h4>
            Todos os produtos desta lista foram selecionados a partir da sua navegação. Aproveite!
          </h4>
          <nav class="links-items">
            <a
              v-on:click.prevent.stop
              href="#"
              class="btn btn-default">
              Conheça a Linx
            </a>
            <a
              v-on:click.prevent.stop
              href="#"
              class="btn btn-default">
              Ajude o algorítimo
            </a>
            <a
              v-on:click.prevent.stop
              href="#"
              class="btn btn-default">
              Seus produtos
            </a>
            <a
              v-on:click.prevent.stop
              href="#"
              class="btn btn-default">
              Compartilhe
            </a>
          </nav>
        </div>
      </div>
    </header>
    <div class="container">
      <main class="row row-products justify-content-center">
        <div class="col-12">
          <h3 class="text-center">Sua seleção especial</h3>
        </div>
        <transition-group
          class="row row-transition-group justify-content-center"
          name="list"
        >
          <div
            v-for="product in products" v-bind:key="product.id"
            class="col-md-3 col-sm-12 product-item"
          >
            <img
              class="img-fluid"
              :src="`https:${product.image}`"
            />
            <div class="product-info">
              <h4 class="name">
                {{ product.name }}
              </h4>
              <p class="description">
                {{ product.description }}
              </p>
              <p class="price-old">
                De: R${{ product.oldPrice.toLocaleString("pr-BR", {minimumFractionDigits: 2}) }}
              </p>
              <p class="price-currency">
                Por: R${{ product.price.toLocaleString("pr-BR", {minimumFractionDigits: 2}) }}
                </p>
              <p class="price-installment">
                ou {{ product.installments.count }}x
                de R$
                {{ product.installments.value.toLocaleString("pr-BR", {minimumFractionDigits: 2}) }}
              </p>
              <a
                v-on:click.prevent.stop
                href="#"
                class="btn btn-default link-buy"
              >
                Comprar
              </a>
            </div>
          </div>
        </transition-group>
        <div class="col-12 more-products">
          <a
            href="#"
            v-on:click.prevent.stop
            @click="moreProducts(nextPage)"
            class="btn btn-default"
          >
            Ainda mais produtos aqui!
          </a>
        </div>
      </main>
      <article class="row row-friends-list justify-content-center">
        <div class="col-12 list-details">
          <h3>Compartilhe a novidade</h3>
          <p>Quer que seus amigos também ganhem a lista personalizada deles? Preencha agora!</p>
        </div>
        <form action="" class="row">
          <div class="col-md-6 col-sm-12 form-newsletter">
            <label for="">Nome do seu amigo</label>
            <input
              v-model="nameFriend"
              type="text"
              class="form-control"
            >
            <transition name="fade">
              <span
                ref="nameFriend"
                class="msg error"
              >
                Preencha o nome corretamente
              </span>
            </transition>
          </div>
          <div class="col-md-6 col-sm-12 form-newsletter">
            <label for="">E-mail</label>
            <input
              v-model="emailFriend"
              type="text"
              class="form-control"
            >
            <transition name="fade">
              <span
                ref="emailFriend"
                class="msg error"
              >
                Preencha com um e-mail válido
              </span>
            </transition>
          </div>
          <div class="col-12 mt-5">
            <span
              ref="success"
              class="msg success"
            >
              Inscrição realizada com sucesso!
            </span>
            <a
              @click="submitInputs"
              v-on:click.prevent.stop
              href="#"
              class="btn btn-default link-list-friends"
            >
              Enviar agora
            </a>
          </div>
        </form>
      </article>
    </div>
    <footer class="container-fluid">
      <div class="row row-footer-page text-center">
        <div class="col-12">
          <p>
            Testando suas habilidades em HTML, CSS e JS.
          </p>
          <p>
            Linx Impulse
          </p>
          <p>
            2019
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios';
import emailValidator from 'email-validator';

export default {
  name: 'BuscaProdutos',
  data() {
    return {
      products: [],
      nextPage: '#',
      nameFriend: '',
      emailFriend: '',
    };
  },
  mounted() {
    const urlPage = 'frontend-intern-challenge-api.iurykrieger.now.sh/products?page=1';

    this.moreProducts(urlPage);
  },
  methods: {
    moreProducts(linkPage) {
      const self = this;
      axios.get(`https://${linkPage}`)
        .then((response) => {
          if (response.status === 200) {
            response.data.products.forEach((product) => {
              self.products.push(product);
            });
            self.nextPage = response.data.nextPage;
          }
        });
    },
    submitInputs() {
      let formValid = true;
      const self = this;

      self.$refs.success.style.opacity = 0;

      if (!this.nameFriend || this.nameFriend.length === 1) {
        formValid = false;
        this.$refs.nameFriend.style.opacity = 1;
      } else {
        this.$refs.nameFriend.style.opacity = 0;
      }
      if (!this.emailFriend || !emailValidator.validate(this.emailFriend)) {
        formValid = false;
        this.$refs.emailFriend.style.opacity = 1;
      } else {
        this.$refs.emailFriend.style.opacity = 0;
      }

      if (!formValid) {
        return false;
      }

      setTimeout(() => {
        self.$refs.success.style.opacity = 1;
      }, 500);

      return true;
    },
  },
};
</script>

<style scoped lang="scss">
  @import '../assets/scss/_variables.scss';
  *{
    a:focus, input:focus{
      box-shadow: 0 0 0 0.2rem rgba(112, 112, 112, .25)!important;
    }
  }
  .background-top{
    background: $color-gray;
    border: 1px solid $color-gray;
    .titles-page{
      margin-top: 3em;
      color: $color-white;
      text-align: center;
      h3{
        font-size: 1.4em;
        font-weight: normal;
        margin: 0;
      }
      h1{
        font-size: 3em;
        font-weight: bold;
        margin: 0;
      }
      h4{
        font-size: 1em;
        font-weight: normal;
        margin: 1.5em 0 2.5em 0;
      }
    }
    .links-items{
      margin-bottom: 2.5em;
      a{
        background: $color-white;
        color: $color-gray-light;
        margin: 0 1.5em;
        border-radius: 4px;
        box-shadow: 0px 1px 6px #00000029;
        border: 1px solid #707070;
        width: 190px;
        line-height: 2em;
        margin-bottom: 1em;
      }
    }
  }
  .row-products{
    margin-bottom: 8em;
    h3{
      margin: 2em 0 0 0;
      font-size: 1.2em;
      color: $color-gray-light;
    }
    .row-transition-group{
      margin: 0;
      .product-item{
        margin-top: 2.4em;
        text-align: center;
        padding: 0 1.5em;
        .product-info{
          text-align: left;
          color: $color-gray-light;
          .name{
            font-size: 1em;
            margin: 1em 0 0 0;
          }
          .description{
            margin: .5em 0 0 0;
            font-size: .95em;
          }
          .price-old{
            margin: .2em 0 0 0;
            font-size: .9em;
          }
          .price-currency{
            margin: .1em 0 0 0;
            font-size: 1.2em;
            font-weight: bold;
          }
          .price-installment{
            margin: .1em 0 0 0;
            font-size: .9em;
          }
          a.link-buy{
            margin: .5em 0 0 0;
            width: 100%;
            border-radius: 4px;
            border: 1px solid $color-gray;
            background: #FFFFFF;
            color: $color-gray-light;
          }
        }
      }
    }
    .more-products{
      margin-top: 3em;
      color: $color-gray-light;
      text-align: center;
      a{
        border-radius: 4px;
        border: 1px solid $color-gray;
        background: #FFFFFF;
        color: $color-gray-light;
        min-width: 240px;
      }
    }
  }
  .row-friends-list{
    color: $color-gray-light;
    text-align: center;
    margin-bottom: 8em;
    .list-details{
      h3{
        font-size: 1.2em;
        margin: 0;
      }
      p{
        margin-top: 2em;
      }
    }
    .form-newsletter{
      text-align: left;
      label, span{
        max-width: 400px;
        margin: 0 auto 5px;
        display: block;
      }
      input{
        max-width: 400px;
        margin: 0 auto;
        width: 100%;
        background: #FFFFFF 0% 0% no-repeat padding-box;
        border: 1px solid #707070;
        border-radius: 0px;
      }
    }
    a.link-list-friends{
      background: #FFFFFF 0% 0% no-repeat padding-box;
      border: 1px solid #707070;
      border-radius: 4px;
      min-width: 240px;
      margin-top: 2px;
      color: $color-gray-light;
    }
  }
  .row-footer-page{
    background: $color-gray;
    color: #FFFFFF;
    padding: 2.5em 0;
    p{
      margin: 0;
    }
  }
</style>
