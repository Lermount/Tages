<template>
    <li class="product-card">
        <span v-if="item.price.old_price" class="discount">Скидка</span>
        <img class="card-image" :src="item.image.url" alt="cartIMG">
        <div class="card-content">
            <p v-if="item.code" class="code"> {{ item.code }}</p>
            <p class="card-name">{{ item.name }}</p>
            <div class="priceAndAction">
                <div class="prices">
                    <span v-if="item.price.old_price" class="oldPrice">{{ Math.round(item.price.old_price) }}₽</span>
                    <span class="price">{{ Math.round(item.price.current_price) }}₽</span>
                </div>

                <div class="action">
                    <i v-if="!isBasket" class="basket" @click="addToBasket(item.id)">
                        <svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd" clip-rule="evenodd"
                                d="M12.6641 10.8023C12.5043 10.8019 12.3502 10.82 12.2038 10.8544C12.0031 10.412 11.7379 10.0151 11.4364 9.7136C10.9246 9.2018 10.1174 9 9 9V10.8C9.6826 10.8 10.0754 10.8982 10.1636 10.9864C10.5038 11.3266 10.8 12.0178 10.8 12.6L10.809 12.7273L11.6909 18.9005C10.2153 18.977 9.07218 20.1229 9.00111 21.5554L9 22.5C9.08714 23.9795 10.2265 25.1177 11.6489 25.1985L11.8532 25.1989C12.2236 26.2481 13.224 27 14.4 27C15.5756 27 16.5757 26.2487 16.9464 25.2H19.0536C19.4243 26.2487 20.4244 27 21.6 27C23.0912 27 24.3 25.7912 24.3 24.3C24.3 22.8088 23.0912 21.6 21.6 21.6C20.4244 21.6 19.4243 22.3513 19.0536 23.4H16.9464C16.5757 22.3513 15.5756 21.6 14.4 21.6C13.2244 21.6 12.2243 22.3513 11.8536 23.4H11.7C11.241 23.3725 10.8293 22.9612 10.7984 22.4461L10.8 21.6C10.8243 21.1332 11.2332 20.7243 11.7446 20.6989L13.5213 20.6993L13.5342 20.7H22.6052L22.738 20.6578C23.4204 20.4413 23.9667 19.9275 24.2253 19.2623L24.3201 19.0746L24.6282 18.4643C24.9469 17.8325 25.2656 17.1997 25.5762 16.5814C26.3369 15.067 26.8153 14.1045 26.9213 13.8682C27.4654 12.6544 26.3632 11.7202 25.2366 11.7003L12.6641 10.8023ZM22.2924 18.9H13.6015C13.5438 18.8839 13.4997 18.835 13.4911 18.774L12.6092 12.6006L25.104 13.4954C24.8854 13.9413 24.4879 14.7376 23.9676 15.7735L23.9546 15.7994C23.6576 16.3906 23.3393 17.0226 23.0211 17.6535L22.7134 18.2631L22.5968 18.4939L22.5529 18.596C22.5058 18.727 22.4125 18.8346 22.2924 18.9ZM21.6 25.1999C22.097 25.1999 22.5 24.797 22.5 24.2999C22.5 23.8029 22.097 23.3999 21.6 23.3999C21.1029 23.3999 20.7 23.8029 20.7 24.2999C20.7 24.797 21.1029 25.1999 21.6 25.1999ZM15.3 24.2999C15.3 24.797 14.8971 25.1999 14.4 25.1999C13.9029 25.1999 13.5 24.797 13.5 24.2999C13.5 23.8029 13.9029 23.3999 14.4 23.3999C14.8971 23.3999 15.3 23.8029 15.3 24.2999Z"
                                fill="black" />
                        </svg>
                    </i>

                    <i v-else class="basket">
                        <svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd" clip-rule="evenodd"
                                d="M18 27.8999C12.5323 27.8999 8.09997 23.4676 8.09997 17.9999C8.09997 12.5323 12.5323 8.09995 18 8.09995C23.4676 8.09995 27.9 12.5323 27.9 17.9999C27.9 23.4676 23.4676 27.8999 18 27.8999ZM18 26.0999C22.4735 26.0999 26.1 22.4734 26.1 17.9999C26.1 13.5264 22.4735 9.89994 18 9.89994C13.5265 9.89994 9.89997 13.5264 9.89997 17.9999C9.89997 22.4734 13.5265 26.0999 18 26.0999ZM20.9636 14.6636L16.2 19.4272L14.1364 17.3636L12.8636 18.6363L16.2 21.9727L22.2364 15.9363L20.9636 14.6636Z"
                                fill="#27AE60" />
                        </svg>

                    </i>

                    <i class="favorite" :class="{ 'favorite-added': isFavorite }" @click="addToFavorite(item.id)">
                        <svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path fill-rule="evenodd" clip-rule="evenodd"
                                d="M22.0979 9.00001C20.795 9 19.9551 9.15522 18.9924 9.62647C18.6359 9.80094 18.3015 10.0111 17.9909 10.2562C17.6923 10.0248 17.3713 9.82433 17.0296 9.65532C16.0482 9.1699 15.1652 9 13.9091 9C10.5448 9 8.09998 11.7875 8.09998 15.4079C8.09998 18.142 9.62442 20.7831 12.4639 23.3369C13.9544 24.6774 15.8574 26.004 17.2202 26.7097L18 27.1135L18.7797 26.7097C20.1426 26.004 22.0456 24.6774 23.536 23.3369C26.3755 20.7831 27.9 18.142 27.9 15.4079C27.9 11.8259 25.4321 9.01382 22.0979 9.00001ZM26.1 15.4079C26.1 17.5499 24.8276 19.7544 22.3324 21.9986C20.972 23.2221 19.2231 24.4449 18 25.0864C16.777 24.4449 15.0281 23.2221 13.6677 21.9986C11.1725 19.7544 9.90002 17.5499 9.90002 15.4079C9.90002 12.733 11.5953 10.8 13.9091 10.8C14.9079 10.8 15.5233 10.9185 16.2316 11.2688C16.65 11.4757 17.0198 11.749 17.3399 12.0904L18.0021 12.7969L18.6583 12.0848C18.9856 11.7297 19.3608 11.4502 19.7837 11.2432C20.4713 10.9067 21.0483 10.8 22.0942 10.8C24.3794 10.8095 26.1 12.7701 26.1 15.4079Z"
                                fill="black" />
                        </svg>

                    </i>
                </div>
            </div>
        </div>
    </li>
</template>
<script>
export default {
    //Получаем все данные о текущем товаре и данные о всех товарах добавленных в избранное и корзину
    props: ['item', 'cartState', 'favState'],
    emits: ['refreshCart', 'refreshFav'],

    computed: {
        isBasket() {//Проверяем, добавлен ли данный товар в корзину
            return this.cartState?.find(i => i == this.item.id) !== undefined;
        },

        isFavorite() {//Проверяем, добавлен ли данный товар в избранное
            return this.favState?.find(i => i == this.item.id) !== undefined;
        },
    },

    methods: {
        addToBasket(ID) {//Добавляем товар в корзину, эмитим событие в product-list.vue
            this.$emit('refreshCart', ID)//Передаем ID товара в функцию loadCartState из product-list.vue
        },

        addToFavorite(ID) {//Добавляем товар в избранное, эмитим событие в product-list.vue
            this.$emit('refreshFav', ID)//Передаем ID товара в функцию loadFavState из product-list.vue
        }
    }
}
</script>
<style scoped>
.product-card {
    position: relative;
    display: flex;
    flex-direction: column;
    padding: 8px 14px 4px 12px;
    border: 1px solid #EEEEEE;
}

.discount {
    position: absolute;
    top: 8px;
    left: 0px;
    padding: 3px 16px;
    background: #EB5757;

    font-family: SFProDisplay;
    font-size: 14px;
    font-weight: 500;
    line-height: 18px;
    letter-spacing: 0.01em;
    text-align: left;
    color: #FFFFFF;
}

.card-image {
    margin: 0 auto;
}

.card-content {
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    flex-grow: 1;
}

.code {
    margin-top: 23px;
    font-family: SFUIText;
    font-size: 10px;
    font-weight: 400;
    line-height: 14px;
    letter-spacing: 0.02em;
    text-align: left;
    color: #888888;
}

.card-name {
    margin-top: 5px;
    font-family: SFUIText;
    font-size: 16px;
    font-weight: 500;
    line-height: 22px;
    letter-spacing: 0.02em;
    text-align: left;
    color: #000000;
}

.priceAndAction {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.prices {
    display: flex;
}

.oldPrice,
.price {
    font-family: SFUIText;
    font-size: 16px;
    font-weight: 400;
    line-height: 22px;
    letter-spacing: 0.02em;
    text-align: left;
}

.oldPrice {
    margin-right: 9px;
    text-decoration: line-through;
    color: #888888;
}

.price {
    color: #000000;
}

.basket,
.favorite {
    cursor: pointer;
}

.basket:hover svg path {
    fill: #27AE60;
    transition: .3s linear
}

.favorite:hover svg path,
.favorite-added svg path {
    fill: #EB5757;
    transition: .3s linear;
}
</style>