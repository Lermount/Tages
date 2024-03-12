<template>
    <ul class="product-list">
        <item v-for="item in sortedItems" :key="item.id" :cartState="cartState" :favState="favState"
            @refreshCart="loadCartState" @refreshFav="loadFavState" :item="item" />
    </ul>
</template>
<script>
import items from '@/data/items.json';
import item from '@/components/product-item.vue';

export default {
    props: ['priceSortID', 'materialID'], //Получили данные из tages-sort через App.vue

    components: {
        item
    },

    data: () => ({//Собираем данные из карточек товара и эти же данные отправляем обратно им
        cartState: [],
        favState: [],
    }),

    mounted() {//При загрузке страницы проверяем наш LS, если в нем есть данные, то записываем их в массивы
        this.cartState = JSON.parse(localStorage.getItem('cartState')) || [];
        this.favState = JSON.parse(localStorage.getItem('favState')) || [];
    },

    methods: {
        //Т.к. часто товар можно удалить из корзины, с самой страницы "Корзина", то удаление добавил только для избранного.
        loadCartState(value) { //Полуаем ID товара который нужно добавить в корзину
            this.cartState.push(value) //Данный товар добавляем в массив
            localStorage.setItem('cartState', JSON.stringify(this.cartState)) //Массив сохраняем в LS
        },

        loadFavState(value) { //Полуаем ID товара который нужно добавить в избранное
            if (this.favState.find(i => i == value)) { //Проверяем, есть ли такой товар в избранном
                this.favState = this.favState.filter(i => i !== value) //Если товар уже есть в избранном, удаляем его от туда
            } else {
                this.favState.push(value) //Если товара в избранном нет, добавляем его в массив
            }
            localStorage.setItem('favState', JSON.stringify(this.favState))  //Массив сохраняем в LS
        }
    },
    // При помощи полученных данных из tages-sort сортируем наш массив с товарами или же возвращаем исходный вариант
    computed: {
        sortedItems() {
            let itemsData = items
            if (this.materialID) { //Сортировка мо патериалу
                itemsData = itemsData.filter(i => i.material === Number(this.materialID))
            }
            if (this.priceSortID && Number(this.priceSortID) === 1) {//Цена по возрастанию
                itemsData = itemsData.sort((a, b) => a.price.current_price - b.price.current_price)
            }
            if (this.priceSortID && Number(this.priceSortID) !== 1) { //Цена по убыванию
                itemsData = itemsData.sort((a, b) => b.price.current_price - a.price.current_price)
            }
            return itemsData
        }
    }
}
</script>
<style scoped>
.product-list {
    margin-top: 41px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(2, 1fr);
    gap: 40px 48px;
}

@media(max-width: 1500px) {
    .product-list {
        gap: 20px 20px;
    }
}

@media(max-width: 1200px) {
    .product-list {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media(max-width: 710px) {
    .product-list {
        grid-template-columns: repeat(1, 1fr);
        gap: 10px 0px;
    }
}
</style>