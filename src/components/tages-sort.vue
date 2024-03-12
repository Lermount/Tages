<template>
    <div class="sort">
        <div class="sortByPrice">
            <p class="sortTitle">Сортировать по:</p>
            <input type="text" class="sort-input" readonly v-model="priceSort" @click="openPrice">
            <i class="sort-input-icon" @click="openPrice"></i>

            <ul v-if="openPriceSort" class="dropdown">
                <li v-for=" price in priceData" :key="price.id" class="dropdown-item"
                    @click="selectPriceSort(price.name, price.id)">
                    {{ price.name }}
                </li>
            </ul>
        </div>
        <div class="sortByMaterial">
            <p class="sortTitle">Материал</p>
            <input type="text" class="sort-input" readonly v-model="material" @click="openMaterials">
            <i class="sort-input-icon" @click="openMaterials"></i>

            <ul v-if="openMaterialSort" class="dropdown">
                <li v-for="material in materialsData" :key="material.id" class="dropdown-item"
                    @click="selectMaterialSort(material.name, material.id)">
                    {{ material.name }}
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
import materialsData from '@/data/materials.json';
import priceData from '@/data/price.json';

export default {
    emits: ['updatePrice', 'updateMaterial'],

    data: () => ({
        priceSort: 'Цена',
        material: 'Материал',
        priceSortID: null,
        materialID: null,
        materialsData,
        priceData,
        openPriceSort: false,
        openMaterialSort: false
    }),

    watch: {
        //Следим за кликом на странице и зкрываем dropdown при клике из вне 
        openPriceSort: function (value) {
            if (value) {
                document.addEventListener('click', event => {
                    if ((!document.querySelector('.sortByPrice')?.contains(event.target))) {
                        this.openPriceSort = false
                    }
                });
            }
        },

        openMaterialSort: function (value) {
            if (value) {
                document.addEventListener('click', event => {
                    if ((!document.querySelector('.sortByMaterial')?.contains(event.target))) {
                        this.openMaterialSort = false
                    }
                });
            }
        },
    },

    methods: {
        //Открытие и закрытие dropdown для priceSort
        openPrice() {
            if (!this.openPriceSort) {
                this.openPriceSort = true
            } else {
                this.openPriceSort = false
            }
        },
        //Открытие и закрытие для materialsSort
        openMaterials() {
            if (!this.openMaterialSort) {
                this.openMaterialSort = true
            } else {
                this.openMaterialSort = false
            }
        },
        
        selectPriceSort(value, ID) {
            this.priceSort = value //Выбираем сортировку, меняем значение инпута на выбранное
            this.priceSortID = ID  //Приравниваем ID выбранного значения к переменной priceSortID
            this.$emit('updatePrice', this.priceSortID); //Эмитим priceSortID в родительский компонент
            this.openPriceSort = false //Закрываем dropdown
        },
        //Все тоже самое, что и у метода selectPriceSort, только теперь для materialID
        selectMaterialSort(value, ID) {
            this.material = value
            this.materialID = ID
            this.$emit('updateMaterial', this.materialID);
            this.openMaterialSort = false
        }
    }
}
</script>
<style scoped>
.sort {
    margin-top: 32px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-wrap: wrap;
}

.sortByPrice,
.sortByMaterial {
    position: relative;
}

.sortByPrice {
    margin-right: 24px;
}

.sortTitle {
    margin-bottom: 6px;
    padding-left: 16px;
    font-family: SFProDisplay;
    font-size: 12px;
    font-weight: 400;
    line-height: 18px;
    letter-spacing: 0.03em;
    text-align: left;
    color: #4F4F4F;
}

.sort-input {
    padding: 0px 16px;
    height: 40px;
    width: 256px;
    border: none;
    background: #F2F2F2;

    font-family: SFProDisplay;
    font-size: 14px;
    font-weight: 400;
    line-height: 21px;
    letter-spacing: 0.03em;
    text-align: left;
    color: #000000;
    cursor: pointer;
}

.sort-input:focus {
    outline: 1px solid #000;
}

.sort-input-icon {
    position: absolute;
    right: 19px;
    bottom: 14px;
    width: 18px;
    height: 11px;
    cursor: pointer;
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
    background-image: url("data:image/svg+xml,%3Csvg width='18' height='11' viewBox='0 0 18 11' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath fill-rule='evenodd' clip-rule='evenodd' d='M16.2929 0.292908L17.7071 1.70712L9.00001 10.4142L0.292908 1.70712L1.70712 0.292908L9.00001 7.5858L16.2929 0.292908Z' fill='black'/%3E%3C/svg%3E%0A");
}

.dropdown {
    position: absolute;
    top: 69px;
    left: 0;
    width: 256px;
    padding: 10px 16px;
    background: #F2F2F2;
    z-index: 1;
}

.dropdown-item {
    font-family: SFProDisplay;
    font-size: 14px;
    font-weight: 400;
    line-height: 21px;
    letter-spacing: 0.03em;
    text-align: left;
    color: #000000;
    cursor: pointer;
    transition: .3s linear;
}

.dropdown-item:hover {
    color: #27AE60;
    transition: .3s linear;
}

.dropdown-item:not(:last-child) {
    margin-bottom: 10px;
}

@media(max-width: 710px) {
    .sortByPrice {
        margin-right: 0;
        margin-bottom: 24px;
    }
}
</style>