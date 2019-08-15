<template>
    <div class="table-wrapper">
        <h3 class="table-title">Table data using web-sockets</h3>
        <div class="data-table">
            <v-data-table :headers="headers"
                :items="data"
                class="data-table">
                <template v-slot:items="props">
                    <td>{{props}}</td>
                </template>
            </v-data-table>
        </div>
        <div class="buttons-wrapper">
            <button @click="connect"
                    class="buttons">Subscribe</button>
            <button @click="disconnect"
                    class="buttons">Unsubscribe</button>
        </div>
        
    </div>
</template>

<script>

const url = 'wss://stream.globitex.com/market-data';

export default {
    name: 'Content',
    data() {
        return {
            data: [],
            headers: [
                {
                    text: 'seqNo',
                    align: 'center',
                    sortable: true,
                    value: 'seqNo',
                },
                {
                    text: 'Symbol',
                    value: 'symbol',
                    align: 'center',
                },
                {
                    text: 'Exchange Status',
                    value: 'exchangeStatus',
                    align: 'center',
                },
                {
                    text: 'Time',
                    value: 'currentTime',
                    align: 'center',
                },
            ],
            connection: false,
        }
    },
    methods: {
        connect(e) {
            this.socket = new WebSocket(url);
            this.socket.onmessage = (e) => {
                let dataObj = JSON.parse(e.data);

                let object = dataObj.MarketDataIncrementalRefresh;
                let timestamp = object.timestamp;
                let date = new Date(timestamp*1000);
                let hours = date.getHours() < 10 ? '0' + date.getHours() : date.getHours();
                let minutes = date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes();
                let seconds = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds();
                let time = {
                    currentTime: `${hours}:${minutes}:${seconds}`,
                };
                let objectWithDate = Object.assign(object, time);

                this.data.push(objectWithDate);
                // this.data = dataObj;
                // this.data.push(object);
                console.log(this.data);
            }
        },
        disconnect(e) {
            this.socket.close();
            this.data = [];
        },
    },
}
</script>

<style lang="scss">
    .wrapper {
        display: flex;
        justify-content: space-around;
        margin: 0 auto;
    }

    .content-wrapper {
        margin: 0 auto;
    }

    .table-wrapper {
        margin: 40px auto;
    }

    .table-title {
        text-align: center;
    }

    .item {
        width: 50%;
        text-align: center;

        @media only screen and (min-width: 768px) {
            width: 150px;
        };

        & h3 {
            text-align: center;
            padding: 5px 0;
        }
    }
    .data {
        margin: 5px 0;

        &-table {
            margin: 40px auto;
        }
    }

    .buttons {
        padding: 10px 15px;
        margin: 0;
        text-align: center;
        border: 2px solid #000;
        border-radius: 20px;
        background: #FFA600;
        font-size: 16px;
        cursor: pointer;
        box-shadow: 0px 0px 5px 2px rgba(255,166,0,0.75);
        transition: all 1s ease;

        &:hover {
            background: #000;
            color: #FFA600;
            border: 2px solid #FFA600;
            
        }

        &-wrapper {
            margin: 40px auto;
            width: 300px;
            display: flex;
            justify-content: space-around;
        }
    }

    .data-wrap {
        display: flex;
    }

    .item {

        &-wrapper {
            display: flex;
        }
    }

    .table-data {
        color: #fff;
    }

</style>