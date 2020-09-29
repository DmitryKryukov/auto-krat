<template lang="pug">
.discount(
  ref="card",
  :class='{"transform-none" : mouseOver}'
  :style="{ transform: `rotateX(${rollValue * 30}deg) rotateY(${tiltValue * 50}deg)` }",
  @mouseenter="enter()",
  @mouseleave="leave()"
)
  .discount-main
    .discount__tag Кредит от 0&#8239;%
    h2.discount__title Автомобили в&nbsp;кредит от 0&#8239;%
    .discount__text Рассчитайте кредит онлайни получите решение банка за&nbsp;30&nbsp;секунд.
    .discount__link.link.link--arrowed
      .link__arrow(:style="{ backgroundColor: btnColor }")
      .link__text Рассчитать
  .discount__img-group.img-paralax
    .img-paralax__layer.img-paralax__layer--3(
      :style="{ transform: `rotateX(${rollValue * 15}deg) rotateY(${tiltValue * 30}deg) translateZ(50px)` }"
      :class='{"transform-none" : mouseOver}'
    )
    .img-paralax__layer.img-paralax__layer--2(
      :style="{ transform: `rotateX(${rollValue * 20}deg) rotateY(${tiltValue * 40}deg) translateZ(100px) translateY(30px)` }"
      :class='{"transform-none" : mouseOver}'
    )
    .img-paralax__layer.img-paralax__layer--1(
      :style="{ transform: `rotateX(${rollValue * 5}deg) rotateY(${tiltValue * 10}deg) translateZ(10px)` }"
      :class='{"transform-none" : mouseOver}'
    )
      svg(xmlns="http://www.w3.org/2000/svg", viewBox="0 0 320 200")
        defs
          mask#mask(
            maskunits="userSpaceOnUse",
            maskcontentunits="userSpaceOnUse"
          )
            image(
              xlink:href="../../assets/discounts/mask.gif",
              height="100%",
              width="100%"
            )
        g(mask="url(#mask)")
          circle(
            cx="0",
            cy="0",
            r="200%",
            style="stroke: none; fill: var(--background-color);"
          )

    .img-paralax__layer.img-paralax__layer--0
  anchor(href="#", unbound)
</template>

<script>
import anchor from "@/components/controls/Anchor.vue";
import { defineComponent, ref , watch } from "vue-demi";
import { useMediaQuery } from "@vueuse/core";
import { useParallax } from "@vueuse/core";

const { tilt, roll, source } = useParallax();

export default {
  name: "DiscountCard",
  components: {
    anchor,
  },
  props: {
    discount: {
      type: Object,
    },
    color: {
      type: String,
      default: "#fff",
    },
    btnColor: {
      type: String,
      default: "#F2C94C",
    },
  },
  setup() {
    const { tilt, roll, source } = useParallax({
      deviceOrientationTiltAdjust: (i) => i * 2,
      deviceOrientationRollAdjust: (i) => i * 2,
      targetElement: card,
    })
    const card = ref(null);
    const rollValue = ref(0);
    const tiltValue = ref(0);
    const mouseOver = ref(false);

    watch([mouseOver, tilt, roll], ([mouseOverV, tiltV, rollV])=>{
      rollValue.value = mouseOverV ? rollV : 0
      tiltValue.value = mouseOverV ? tiltV : 0
    })

    return {
      tilt,
      roll,
      source,
      isMobile: useMediaQuery("(max-width: 700px)"),
      card,
      mouseOver,
      rollValue,
      tiltValue
    };
  },
  data() {
    return {
      paralaxCardStyle: {
        transform: `rotateX(${roll * 20}deg) rotateY(${tilt * 20}deg)`,
      },
    };
  },
  mounted() {
    this.$refs.card.style.setProperty("--background-color", this.color);
  },
  methods: {
    enter() {
      this.mouseOver = true;
    },
    leave() {
      this.mouseOver = false;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../../styles/toptier";

.discount {
  --accent-primary: #f2c94c; //Из рута

  --background-color: #fff;
  --rotation: 2deg;

  position: relative;
  padding: 2.25rem 1.5rem 0;
  background-color: var(--background-color);
  border-radius: var(--border-radius);
  overflow: hidden;
  transition: filter 0.3s var(--ease), transform .3s var(--ease);
}
.discount__tag {
  display: flex;
  margin-bottom: 1rem;
  font-size: 0.75rem;
  font-weight: bold;
  line-height: 1rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;

  padding-top: 0.25em; //Визуальная компенсация
}
.discount__tag:before {
  content: "";
  width: 0.75rem;
  height: 0.75rem;
  margin-right: 0.625rem;
  background-color: var(--accent-primary);
  border-radius: 3px;
  mix-blend-mode: multiply;
  transform: rotate(-3deg);
}
.discount__title {
  margin-bottom: 0.75rem;
  font-weight: bold;
  font-size: 1.875rem;
  line-height: 110%;
}
.discount__text {
  margin-bottom: 1rem;
  font-weight: 500;
  font-size: 1rem;
  line-height: 1.5rem;
}
.discount__link {
  position: relative;
  color: var(--text-main-accented);
  z-index: 9;
}
//Из отдельного компонента
.link {
  display: flex;
  align-items: center;
}
.link__arrow {
  position: relative;
  width: 2rem;
  height: 2rem;
  margin-right: 0.875rem;
  background-color: var(--accent-primary);
  border-radius: 0.1875rem;
  transform: rotate(calc(var(--rotation) * -1));
  transition: all 0.3s var(--ease);
}
.link__arrow:after {
  content: "→";
  position: absolute;
  top: calc(50% - 0.75rem - 2px); // 2px — визуальная компенсация
  left: calc(50% - 0.5rem);
  font-size: 1.5rem;
  line-height: 100%;
  transform: translateX(5px);
  transition: transform 0.3s var(--ease);
}
.link__text {
  margin-top: -0.1875rem; //Визуальная компенсация
}

.img-paralax {
  position: relative;
  margin: 0 -1.5rem;
  height: 200px;
}
.img-paralax__layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.img-paralax__layer--0 {
  background-image: url("https://us.123rf.com/450wm/sudowoodo/sudowoodo1606/sudowoodo160600027/58944604-seamless-star-pattern-silver-and-golden-stars-on-black-background-tile-able-texture-.jpg?ver=6");
  animation: 60s linear 0s infinite backgroundAnim;
  @keyframes backgroundAnim {
    from {
      background-position: 0% 0%;
    }
    to {
      background-position: 450px 450px;
    }
  }
}
.img-paralax__layer--1 {
  left: -1rem;
  top: -1rem;
  width: calc(100% + 2rem);
  height: calc(100% + 1rem);
  z-index: 1;
  transition: transform .5s var(--ease);
}
.img-paralax__layer--2 {
  background-image: url("../../assets/discounts/pug.png");
  background-position: center center;
  background-size: contain;
  background-repeat: no-repeat;
  height: calc(100% + 10px);

  z-index: 3;
  transition: transform .5s var(--ease);
}
.img-paralax__layer--3 {
  z-index: 2;
  background-image: url("../../assets/discounts/graph.png");
  background-position: center center;
  background-size: contain;
  background-repeat: no-repeat;
  mix-blend-mode: difference;
  transition: transform .5s var(--ease);
}

.discount:hover {
  filter: brightness(0.95) saturate(1.2);
  .link__arrow {
    border-radius: 50%;
  }
  .link__arrow:after {
    transform: none;
  }
}

.transform-none {
  transition: none;
}
</style>

