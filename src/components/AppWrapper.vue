<template>
    <div class="app-wrapper">
        <header class="header__app-wrapper">
            <div class="search__header">
                <input v-if="showInput" v-model="searchValue" 
                    type="text" 
                    style="padding-left: 20px"
                    placeholder="Найти валюту" 
                    class="search-input__search">
            </div>
            <div class="navigation__header">
                <button @click="goList" class="nav-button__navihation">Список валют</button>
                <button @click="goConverter" class="nav-button__navihation">Конвертер</button>
            </div>
        </header>
        <list-of-currencies :searchValueData="searchValue" :currenciesData="priceDataStorage" v-if="listIsOpen" />
        <converter v-if="converterIsOpen" :currenciesData="priceDataStorage" />
    </div>
</template>

<script>
import listOfCurrencies from './ListOfCurrencies';
import Converter from './Converter'

export default {
    data() {
        return {
            priceDataStorage: [],

            listIsOpen: true,
            converterIsOpen: false,
            showInput: true,
            searchValue: '',
        }
    },
    
    methods: {
        async getPrice() {
            try {
                let response = await fetch('https://www.cbr-xml-daily.ru/daily_json.js')
                let priceData = await response.json();
                let unitPriceData = Object.values(priceData.Valute)
                unitPriceData.map(item => this.priceDataStorage.push(item))
                // console.log(this.priceDataStorage[0].Name);
            } catch (error) {
                console.log(error);
            }
        },  
        goList() {
            this.listIsOpen = true,
            this.converterIsOpen = false
            this.showInput = true
        },
        goConverter() {
            this.listIsOpen = false,
            this.converterIsOpen = true,
            this.showInput = false
        },
    },

    mounted() {
        this.getPrice()
    },
    components: { 
        listOfCurrencies,
        Converter
    },
}
</script>

<style lang="css" scoped>
    .app-wrapper {
        height: 100vh;
        width: 100%;
    }
    @media screen and (max-width: 780px) {
        .app-wrapper {
            height: 100vh;
            width: 100%;
        }
    }
    .header__app-wrapper {
        height: 60px;
        width: 100%;
        border-top: 1px solid rgb(55, 139, 207);
        border-bottom: 1px solid rgb(55, 139, 207);
        background-color: rgb(186, 200, 211);
        display: flex;
        justify-content: space-evenly;
        align-items: center;
    }
    @media screen and (max-width: 780px) {
        .header__app-wrapper {
            height: 140px;
            width: 100%;
            border-top: 1px solid grey;
            border-bottom: 1px solid grey;
            display: flex;
            flex-direction: column;
            justify-content: space-evenly;
            align-items: center;
        }
    }
    .search__header {
        height: 40px;
        width: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    @media screen and (max-width: 780px) {
        .search__header {
            height: 40px;
            width: 82%;
            display: flex;
            justify-content: space-around;
            align-items: center;
        }
    }
    .search-input__search {
        height: 26px;
        width: 66%;
        outline: none;
        color: rgb(55, 139, 207);
        font-size: 14px;
        border: 1px solid rgb(166, 191, 201);
        border-radius: 3px;
    }
    .search-input__search::placeholder {
        color: rgb(55, 139, 207);
        font-size: 14px;
    }
    .search-button__search {
        height: 30px;
        width: 20%;
        margin-left: 20px;
        color: rgb(55, 139, 207);
        border: 1px solid rgb(166, 191, 201);
        border-radius: 3px;
    }
    .navigation__header {
        height: 40px;
        width: 40%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    @media screen and (max-width: 780px) {
        .navigation__header {
            height: 40px;
            width: 92%;
            display: flex;
            justify-content: space-evenly;
            align-items: center;
        }
    }
    .nav-button__navihation {
        height: 30px;
        width: 30%;
        margin-left: 20px;
        color: rgb(55, 139, 207);
        border: 1px solid rgb(166, 191, 201);
        border-radius: 3px;
    }
    @media screen and (max-width: 780px) {
        .nav-button__navihation {
            height: 30px;
            width: 40%;
            margin: 10px;
            
        }
    }
</style>