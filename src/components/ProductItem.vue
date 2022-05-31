<template>
    <article class="catalog-item">
        <div class="img-holder">
            <img :src="`https://c.roastmarket.de/media/catalog/product${product._source.image}`"
                :alt="product._source.image_label">
        </div>
        <div class="catalog-item-body">
            <span class="item-price">{{ product._source.base_price_amount }} &euro;</span>
            <span class="item-price-second">{{ product._source.base_price_amount }} &euro; / {{
                    product._source.base_price_base_amount
            }}{{ product._source.base_price_base_unit }} </span>
            <a href="#" class="item-name">{{ product._source.name }}</a>
            <span>Rating: {{ product._source.yotpo_rating}}</span> 
        </div>
    </article>

</template>

<script>

export default {
    name: 'ProductItem',
    props: {
        product: {
            type: Object,
            default: () => { }
        }
    }
}
</script>+

<style lang="scss">
.catalog {
    padding: 30px 0;

    @media screen and (min-width: 768px) {
        padding: 50px 0;
    }

    h5 {
        text-align: left;
        color: #848687;
    }
}

.catalog-list {
    display: flex;
    flex-wrap: wrap;
    margin-left: -10px;
    margin-right: -10px;

}

.catalog-item {
    width: calc(100% - 20px);
    margin: 0 10px 10px;
    box-shadow: 0px 0px 30px rgba(0, 0, 0, 0.1);

    @media (min-width: 375px) {
        width: calc(50% - 20px);
    }

    @media (min-width: 768px) {
        width: calc(33.333% - 20px);
    }

    @media (min-width: 1024px) {
        width: calc(25% - 20px);
    }
}

.img-holder {
    padding-top: 150%;
    position: relative;
    width: 100%;
    max-width: 100%;
    height: auto;
    mix-blend-mode: multiply;
    overflow: hidden;

    // &:hover{
    //     transform: scale(1.5);
    //     transition: transform;
    //     transition-duration:1s;
    // }

    img {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        margin: auto;
    }
}

.catalog-item-body {
    padding: 10px;
    background-color: #fff;
    text-align: left;
}

.item-price,
.item-price-second,
.item-name {
    display: block;
    margin-bottom: 5px;
}

.item-price-second {
    font-size: 14px;
    color: #848687;
}

</style>