<template>
    <div class="currencies-list">
        <div class="tickers-wrapper__currencies-list">
            <div v-for="ticker of filteredList" :key="ticker.Name" class="ticker__tickers-wrapper">
                <div class="ticker-name__ticker">
                    <span class="t-name__ticker-name">{{ticker.Name}}</span>
                </div>
                <div class="previous__ticker">
                    <span id="priceId" 
                        :style="[Math.sign(Math.trunc((ticker.Previous - ticker.Value) * 10000 ) / 10000 ) === -1 ? redPrice : greenPrice]"
                        class="previous-text__previous">
                        {{Math.trunc( (ticker.Previous - ticker.Value) * 10000 ) / 10000 }}
                    </span>
                </div>
                <div class="currency-pair">
                    <div class="first-currency">
                        <span class="text__second-currency-price">{{ticker.Nominal}}</span>
                        <span class="text__first-currency">{{ticker.CharCode}}</span>
                    </div>
                    <div class="second-currency">
                        <span class="text__second-currency-price">{{ticker.Value}}</span>
                        <span class="text__second-currency">RUB</span>
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
            searchValueData: Array
        },
        data() {
            return {
                greenPrice: {
                    color: '#008000'
                },
                redPrice: {
                    color: '#FF0000'
                },
            }
        },
        computed: {
            filteredList() { 
                if (this.isCyrillic(this.searchValueData) === true) {
                    return this.currenciesData.filter(unit => {
                        return unit.Name.toLowerCase().includes(this.searchValueData.toLowerCase())
                    })
                } else {
                    return this.currenciesData.filter(unit => {
                        return unit.CharCode.toLowerCase().includes(this.searchValueData.toLowerCase())
                    })
                }
            }
        },
        methods: {
            isCyrillic(str) {
                return /[а-я]/i.test(str);
            }
        },
    }
    
</script>

<style lang="css" scoped>
.currencies-list {
    height: calc( 100vh - 64px );
    width: 100%;
    margin-top: 2px;
    background-color: rgb(186, 200, 211);
    display: flex;
    justify-content: center;
    align-items: center;
}
.tickers-wrapper__currencies-list  {
    height: 90%;
    width: 90%;
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
.tickers-wrapper__currencies-list::-webkit-scrollbar {
    display:none;
    }
.ticker__tickers-wrapper {
    height: 220px;
    width: 200px;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    margin: 20px;
    background-color: rgb(186, 200, 211);
    display: flex;
    flex-direction: column;
    align-items: center;
}
.ticker-name__ticker {
    height: 30px;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    margin: 10px;
    background-color: white;
    color: rgb(55, 139, 207);
    font-weight: 600;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    
}
.t-name__ticker-name {
    font-size: 13px;
    font-family: 'IBM Plex Sans', sans-serif; 
}
.previous__ticker {
    height: 30px;
    width: 50%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    font-family: 'IBM Plex Sans', sans-serif;
    background-color: white;
    font-weight: 600;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}
.currency-pair {
    height: 52%;
    width: 90%;
    margin-top: 10px;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    font-family: 'IBM Plex Sans', sans-serif;
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.first-currency {
    height: 30px;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    margin: 10px;
    background-color: rgb(55, 139, 207);
    color: white;
    font-weight: 600;
    display: flex;
    justify-content: space-around;
    align-items: center;
}
.second-currency {
    height: 30px;
    width: 90%;
    border: 1px solid rgb(55, 139, 207);
    border-radius: 3px;
    margin: 10px;
    background-color: rgb(55, 139, 207);
    color: white;
    font-weight: 600;
    display: flex;
    justify-content: space-around;
    align-items: center;
}
</style>