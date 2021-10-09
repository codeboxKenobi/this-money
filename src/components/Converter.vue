<template>
    <div class="converter">
        <div class="tickers-wrapper__converter">
            <div class="ticker__tickers-wrapper">
                <div class="tickerBlock">
                    <div class="currency-pair">
                        <div class="first-currency">
                            <div v-if="showAddCurrency1" class="addFirstCurrency">
                                <div class="closeCross">
                                    <button @click="closeFirstCurrency" class="closeCrossButton">X</button>
                                </div>
                                <div class="select-currency">
                                    <input v-model="searchValuesLeft" class="currencySearchInput" type="text" placeholder="Найти валюту">
                                    <div v-for="currencyUnits of filteredListLeft" :key="currencyUnits.Name" id="currencyUnits.Name" class="selectCurrencyUnit">
                                         <div class="unitData"> 
                                            <button class="selectUnit" @click="addFirstExchangeUnit" >
                                            <div style="height: 40px; margin-bottom: 5px;">
                                                {{currencyUnits.Name}}
                                            </div>
                                            <div style="height: 40px; width: 100%; margin-bottom: 5px;">
                                                {{currencyUnits.CharCode}}  
                                            </div>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="ticker-name">Австралийский дoллар</div>
                            <div class="ticker-unit">
                                <div class="currency">
                                    <button @click="addFirstCurrency" type="button" class="curr">AUD</button>
                                    <img src="../assets/chArrow.svg" alt="">
                                </div>
                                <input class="currency-input" type="text" placeholder="Enter currency">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="arrowDropWrapper">
                    <div class="arrows">
                        <img class="arrowImg" src="../assets/arrows.svg" alt="">
                    </div>
                    <div class="arrowsD">
                        <span class="drop">Сбрoсить</span>
                    </div>
                </div>
                <div class="tickerBlock">
                    <div class="currency-pair">
                        <div class="first-currency">
                            <div v-if="showAddCurrency2" class="addSecondCurrency">
                                <div class="closeCross2">
                                    <button @click="closeSecondCurrency"  class="closeCrossButton2">X</button>
                                </div>
                                <div class="select-currency2">
                                <input v-model="searchValueRight" class="currencySearchInput" type="text" placeholder="Найти валюту">
                                    <div v-for="currencyUnits of filteredListRight" :key="currencyUnits.Name" class="selectCurrencyUnit">
                                         <div class="unitData">
                                            <button class="selectUnit" @click="addUnit" >
                                                <div>
                                                    {{currencyUnits.Name}}
                                                </div>
                                                <div>
                                                    {{currencyUnits.CharCode}}  
                                                </div>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="ticker-name">Рoссийский рубль</div>
                            <div class="ticker-unit">
                                <div class="currency">
                                    <button @click="addSecondCurrency" type="button" class="curr">RUB</button>
                                    <img src="../assets/chArrow.svg" alt="">
                                </div>
                                <input class="currency-input" type="text" placeholder="Enter currency">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            currenciesData: Array,
        },
        data() {
            return {
                showAddCurrency1: false,
                showAddCurrency2: false,
                searchValuesLeft: '',
                searchValueRight: '',

                firstExchangeUnit: [],
                secondExchangeUnit: [],

            }
        },
        computed: {
            filteredListLeft() { 
                if (this.isCyrillic(this.searchValuesLeft) === true) {
                    return this.currenciesData.filter(unit => {
                        return unit.Name.toLowerCase().includes(this.searchValuesLeft.toLowerCase())
                    })
                } else {
                    return this.currenciesData.filter(unit => {
                        return unit.CharCode.toLowerCase().includes(this.searchValuesLeft.toLowerCase())
                    })
                }
            },
            filteredListRight() { 
                if (this.isCyrillic(this.searchValueRight) === true) {
                    return this.currenciesData.filter(unit => {
                        return unit.Name.toLowerCase().includes(this.searchValueRight.toLowerCase())
                    })
                } else {
                    return this.currenciesData.filter(unit => {
                        return unit.CharCode.toLowerCase().includes(this.searchValueRight.toLowerCase())
                    })
                }
            },
        },
        methods: {
            addFirstCurrency() {
                this.showAddCurrency1 = true
            },
            closeFirstCurrency() {
                this.showAddCurrency1 = false
            },
            addSecondCurrency() {
                this.showAddCurrency2 = true
            },
            closeSecondCurrency() {
                this.showAddCurrency2 = false
            },
            isCyrillic(str) {
                return /[а-я]/i.test(str);
            },
            async addFirstExchangeUnit(evt) {
                console.log(evt.target.lastChild);
                let response = await fetch('https://www.cbr-xml-daily.ru/daily_json.js')
                let resData = await response.json();
                console.log(resData.Valute.USD);
            }
        },
        mounted() {
            
        }
    }
</script>

<style lang="css" scoped>
.converter {
    height: calc( 100vh - 64px );
    width: 100%;
    margin-top: 2px;
    background-color: rgb(186, 200, 211);
    display: flex;
    justify-content: center;
    align-items: center;
}
.tickers-wrapper__converter {
    height: 90%;
    width: 99%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 4px;
    overflow-x: hidden;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;
    background-color: white;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
    align-items: center;
}
@media screen and (max-width: 480px) {
    .tickers-wrapper__converter {
        height: 90%;
        width: 90%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 4px;
        overflow-x: hidden;
        overflow-y: scroll;
        -webkit-overflow-scrolling: touch;
        background-color: white;
        display: flex;
        flex-direction: column;
    }
}
.tickers-wrapper__converter::-webkit-scrollbar {
    display:none;
}

.ticker__tickers-wrapper {
    height: 80%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    background-color: rgb(186, 200, 211);
    display: flex;
    justify-content: space-around;
    align-items: center;
}

@media screen and (max-width: 480px) {
    .ticker__tickers-wrapper {
        height: 90%;
        width: 86%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        background-color: rgb(186, 200, 211);
        display: flex;
        flex-direction: column;
        align-items: center;
    }
}
.tickerBlock {
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    height: 80%;
    width: 35%;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
}
@media screen and (max-width: 480px) {
    .tickerBlock {
        border: none;
        height: 36%;
        width: 90%;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: center;
    }
}
.ticker-name {
    height: 40px;
    width: 90%;
    color: white;
    font-size: 20px;
    font-weight: 600;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    
}

@media screen and (max-width: 480px) {
    .ticker-name {
        height: 30px;
        width: 90%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        color: white;
        font-size: 14px;
        margin-left: -14px;
        font-weight: 600;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
.ticker-unit {
    height: 60%;
    width: 80%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.currency-pair {
    height: 90%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    font-family: 'IBM Plex Sans', sans-serif;
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
@media screen and (max-width: 480px) {
    .currency-pair {
        height: 100%;
        width: 90%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        font-family: 'IBM Plex Sans', sans-serif;
        background-color: white;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
}
.currency {
    margin-top: -70px;
}
@media screen and (max-width: 480px) {
    .currency {
        font-size: 18px;
        margin-top: -20px;
    }
}
.curr {
    height: 40px;
    width: 80px;
    font-family: 'IBM Plex Sans', sans-serif;
    background-color: rgb(55, 139, 207);
    border: none;
    color: white;
    font-weight: 600;
    font-size: 32px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-bottom: 5px solid white;
}

.currency-input {
    height: 30px;
    width: 80%;
    font-size: 20px;
    margin-top: 100px;
    outline: none;
    border: none;
    color: white;
    font-size: 30px;
    border-bottom: 5px solid white;
    background-color: rgb(55, 139, 207);
}
@media screen and (max-width: 480px) {
 .currency-input {
    height: 30px;
    width: 70%;
    margin-top: 10px;
    outline: none;
    border: none;
    color: white;
    font-size: 30px;
    border-bottom: 5px solid white;
    background-color: rgb(55, 139, 207);
    }
}
.currency-input::-webkit-input-placeholder { 
   color:white;
   font-size: 14px;
}
@media screen and (max-width: 480px) {
    .currency-input::-webkit-input-placeholder { 
    color:white;
    font-size: 10px;
    }
}
.first-currency {
    height: 90%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    background-color: rgb(55, 139, 207);
    color: white;
    font-size: 40px;
    font-weight: 600;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}

@media screen and (max-width: 480px) {
    .first-currency {
        height: 90%;
        width: 90%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        background-color: rgb(55, 139, 207);
        color: white;
        font-size: 26px;
        font-weight: 600;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
.arrowDropWrapper {
        height: 40%;
        width: 20%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 4px;
        background-color: white;;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: center;
}
@media screen and (max-width: 480px) {
    .arrowDropWrapper {
            height: 14%;
            width: 40%;
            border: 1px solid rgb(55, 139, 207);
            border-radius: 3px;
            background-color: white;
            display: flex;
            flex-direction: column;
            justify-content: space-evenly;
            align-items: center;
    }
}
.arrows {
        height: 24%;
        width: 60%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        background-color: rgb(55, 139, 207);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
}
.arrowsD {
        height: 24%;
        width: 60%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        background-color: rgb(55, 139, 207);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
}


@media screen and (max-width: 480px) {
    .arrows {
        height: 30%;
        width: 80%;
        background-color:rgb(55, 139, 207);
        background-image: url(../assets/arrows.svg);
        background-size: cover;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
@media screen and (max-width: 480px) {
    .arrowsD {
        height: 30%;
        width: 80%;
        border: 1px solid rgb(55, 139, 207);
        border-radius: 3px;
        border-radius: 3px;
        background-color: rgb(55, 139, 207);
        justify-content: center;
        align-items: center;
    }
}
.drop { 
        font-family: 'IBM Plex Sans', sans-serif;
        color:white;
        font-size: 18px;
        font-weight: 600;
        display: flex;
        justify-content: center;
        align-items: center;
}
@media screen and (max-width: 480px) {
    .drop {
        font-family: 'IBM Plex Sans', sans-serif;
        color:white;
        font-size: 14px;
        font-weight: 600;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}

.addFirstCurrency {
    height: 62%;
    width: 32%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    background-color: white;
    position: absolute;
    z-index: 100;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    
}
.addSecondCurrency {
    height: 62%;
    width: 32%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    background-color: white;
    position: absolute;
    z-index: 100;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    
}
.closeCross {
    height: 10%;
    width: 90%;
    color: rgb(55, 139, 207);
    display: flex;
    justify-content: flex-end;
    align-items: center;
}
.closeCross2 {
    height: 10%;
    width: 90%;
    color: rgb(55, 139, 207);
    display: flex;
    justify-content: flex-end;
    align-items: center;
}
.closeCrossButton {
    height: 100%;
    width: 8%;
    color: rgb(55, 139, 207);
    border: none;
    background-color: white;
    font-size: 19px;
}
.closeCrossButton2 {
    height: 100%;
    width: 8%;
    color: rgb(55, 139, 207);
    border: none;
    background-color: white;
    font-size: 19px;
}
.select-currency {
    margin-bottom: 10px;
    height: 86%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    background-color: rgb(55, 139, 207);
    border-radius: 3px;
    overflow-x: hidden;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;
    display: flex;
    flex-direction: column;
    align-items: center;
}
.select-currency::-webkit-scrollbar {
    display:none;
    }
.selectCurrencyUnit {
    margin-top: 80px;
    margin-bottom: 10px;
    height: 90%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    background-color: rgb(55, 139, 207);
    border-radius: 3px;

}
.select-currency2 {
    margin-bottom: 10px;
    height: 86%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    background-color: rgb(55, 139, 207);
    border-radius: 3px;
    overflow-x: hidden;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;
    display: flex;
    flex-direction: column;
    align-items: center;
}
.select-currency2::-webkit-scrollbar {
    display:none;
}
.selectCurrencyUnit2 {
    height: 90%;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    background-color: white;
    border-radius: 3px;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;

}
.unitData {
    height: 90px;
    width: 100%;
    color: rgb(55, 139, 207);
    border: 1px solid white;
    border-radius: 3px;
    background-color: white;
    font-size: 18px;
    margin-top: 0px;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.currencySearchInput {
    position: absolute;
    height: 60px;
    width:76%;
    margin-top: 10px;
    color: rgb(55, 139, 207);
    font-size: 22px;
    padding-left: 20px;
    outline: none;
    background-color: rgb(186, 200, 211);
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;

}
.currencySearchInput::placeholder {
    color: rgb(55, 139, 207);
}
.selectUnit {
    color: rgb(55, 139, 207);
    border: none;
    background-color: white;
    font-size: 22px;
    
}
</style>