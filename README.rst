==================================
Welcome to python-bitrue-lab 0.0.1
==================================

Note
----



This is an unofficial Python wrapper for the `Bitrue exchange REST API v1/ <https://github.com/Bitrue-exchange/bitrue-official-api-docs>`_. I am in no way affiliated with Bitrue, use at your own risk.

If you came here looking for the `Bitrue exchange <https://www.binance.com/?ref=10099792>`_ to purchase cryptocurrencies, then `go here <https://www.bitrue.com/activity/task/task-landing?inviteCode=EWHAHT>`_. If you want to automate interactions with Binance stick around.


Source code
  https://github.com/sammchardy/python-bitrue-lab

Documentation
  https://python-bitrue.readthedocs.io/en/latest/  (pending release)

Binance  Telegram
  https://t.me/BitrueOfficial

Features
--------

- Implementation of all General, Market Data and Account endpoints.
- Simple handling of authentication
- No need to generate timestamps yourself, the wrapper does it for you
- Response exception handling
- Websocket handling with reconnection and multiplexed connections
- Symbol Depth Cache
- Historical Kline/Candle fetching function
- Withdraw functionality
- Deposit addresses



| Bitrue Official API | Status | python-bitrue |
| ------------- | ------------- | ----- |
| [General endpoints (PUBLIC)](https://github.com/Bitrue-exchange/bitrue-official-api-docs#general-endpoints)|	 | |
| [Test connectivity](https://github.com/Bitrue-exchange/bitrue-official-api-docs#test-connectivity)	| complete | 			ping |
| [Check server time]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#check-server-time)| complete | get_server_time |
| [Exchange information]（Some fields not support. only reserved）	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#exchange-information-some-fields-not-support-only-reserved)	| complete | get_exchange_info |
| [Symbol Info (From Exchange Info)]	| complete | get_symbol_info |
| Market Data endpoints (PUBLIC)]| | |
| [Order book]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#order-book)	| complete | get_order_book |
| [Recent trades list]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#recent-trades-list)	| complete | get_recent_trades |
| [Old trade lookup (MARKET_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#old-trade-lookup-market_data)	| complete | get_historical_trades |
| [Compressed/Aggregate trades list]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#compressedaggregate-trades-list)	| complete | get_aggregate_trades |
| | IN DEVELOPMENT | aggregate_trade_iter |
| NOT AVAILABLE	| get_klines |
| NOT AVAILABLE	| REQUIRES KLINES	| _get_earliest_valid_timestamp |
| NOT AVAILABLE	| REQUIRES KLINES	| get_historical_klines |
| NOT AVAILABLE	| REQUIRES KLINES	| get_historical_klines_generator |
| [24hr ticker price change statistics]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#24hr-ticker-price-change-statistics)	| complete | get_ticker_24h |
| [Symbol price ticker]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#symbol-price-ticker)	| complete | get_ticker_price |
| [Symbol order book ticker]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#symbol-order-book-ticker) | complete | get_orderbook_ticker |
| [Account endpoints (PRIVATE)] | | |
| [New order (TRADE)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#new-order--trade) | complete | create_order |
| | complete | order_limit |
| | complete | order_limit_buy |
| | complete | order_limit_sell |
| | complete | order_market |
| | complete | order_market_buy |
| | complete | order_market_sell |
| NOT AVAILABLE | complete | create_test_order |
| [Query order (USER_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#query-order-user_data)  | complete | get_order |
| [Cancel order (TRADE)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#cancel-order-trade)	 | complete | cancel_order |
| [Current open orders (USER_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#current-open-orders-user_data) | complete | get_open_orders |
| [All orders (USER_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#all-orders-user_data) | complete | get_all_orders |
| [Account information (USER_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#account-information-user_data) | complete | get_account |
| | complete | get_asset_balance
| [Account trade list (USER_DATA)]	 (https://github.com/Bitrue-exchange/bitrue-official-api-docs#account-trade-list-user_data) | complete | get_my_trades |
| NOT AVAILABLE	| complete | get_system_status |
| NOT AVAILABLE	| complete | get_account_status |
| NOT AVAILABLE	| complete | get_dust_log |
| NOT AVAILABLE	| complete | get_trade_fee |
| NOT AVAILABLE	| complete | get_asset_details |
| NOT AVAILABLE	| complete | withdraw |
| NOT AVAILABLE	| complete | get_deposit_history |
| NOT AVAILABLE	| complete | get_withdraw_history |
| NOT AVAILABLE	| complete | get_deposit_address |
| NOT AVAILABLE	| complete | get_withdraw_fee |
| NOT AVAILABLE	| complete | stream_get_listen_key |
| NOT AVAILABLE	| complete | stream_keepalive |
| NOT AVAILABLE	| complete | stream_close |

Quick Start
-----------

`Register an account with Binance <https://www.bitrue.com/activity/task/task-landing?inviteCode=EWHAHT>`_.

`Generate an API Key <https://www.bitrue.com/account/api>`_ and assign relevant permissions.

.. code:: bash

    pip install python-bitrue (pending release)


.. code:: python

    from bitrue.client import Client
    api_key=''
    api_secret=''
    client = Client(api_key, api_secret)

    # get market depth
    depth = client.get_order_book(symbol='BNBBTC')

    # place a test market buy order, to place an actual order use the create_order function
    order = client.create_test_order(
        symbol='XRPUSDT',
        side=Client.SIDE_BUY,
        type=Client.ORDER_TYPE_MARKET,
        quantity=100)

    # get all symbol prices
    prices = client.get_all_tickers()

    # withdraw 1000 XRP
    # check docs for assumptions around withdrawals
    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        result = client.withdraw(
            asset='ETH',
            address='<eth_address>',
            amount=100)
    except BinanceAPIException as e:
        print(e)
    except BinanceWithdrawException as e:
        print(e)
    else:
        print("Success")

    # fetch list of withdrawals
    withdraws = client.get_withdraw_history()

    # fetch list of ETH withdrawals
    eth_withdraws = client.get_withdraw_history(asset='ETH')

    # get a deposit address for BTC
    address = client.get_deposit_address(asset='BTC')

    # Fetch balance of asset, create a sell order - subsequently cancel sell order using orderId
    asset = test.get_asset_balance(asset='XRP')
    asset_free = asset['free']
    order = test.create_order(symbol='XRPUSDT',
                            side=Client.SIDE_SELL,
                            type=Client.ORDER_TYPE_LIMIT,
                            quantity=int(float(asset_free)),
                             price=4.01)
    print(order)
    orderId = order['orderId']
    print('Order ID: ',orderId, 'sleeping for 10')
    time.sleep(10)
    pprint(test.cancel_order(symbol='XRPUSDT', orderId=orderId))

    #check for open orders and format data for easy reading.
    try:
        open = test.get_open_orders(symbol='CSCXRP',orderId=orderId) #,orderId=orderId
        filled = open[0]['executedQty']
        total = open[0]['origQty']
        print('Order status: ', float(filled), 'of',float(total))
    except:
        print('no orders open')

    # get historical kline data from any date range - BITRUE CURRENTLY DOES NOT OFFER KLINE QUERIES VIA API

    # fetch 1 minute klines for the last day up until now
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # fetch 30 minute klines for the last month of 2017
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # fetch weekly klines since it listed
    klines = client.get_historical_klines("NEOBTC", Client.KLINE_INTERVAL_1WEEK, "1 Jan, 2017")

For more `check out the documentation <https://python-binance.readthedocs.io/en/latest/>`_.

Donate
------

If this library helped you out feel free to donate.

- ETH: 0xD7a7fDdCfA687073d7cC93E9E51829a727f9fE70
- LTC: LPC5vw9ajR1YndE1hYVeo3kJ9LdHjcRCUZ
- NEO: AVJB4ZgN7VgSUtArCt94y7ZYT6d5NDfpBo
- BTC: 1Dknp6L6oRZrHDECRedihPzx2sSfmvEBys

Other Exchanges
---------------

If you use `Binance Chain <https://testnet.binance.org/>`_ check out my `python-binance-chain <https://github.com/sammchardy/python-binance-chain>`_ library.

If you use `Kucoin <https://www.kucoin.com/ucenter/signup?rcode=E42cWB>`_ check out my `python-kucoin <https://github.com/sammchardy/python-kucoin>`_ library.

If you use `Allcoin <https://www.allcoin.com/Account/RegisterByPhoneNumber/?InviteCode=MTQ2OTk4MDgwMDEzNDczMQ==>`_ check out my `python-allucoin <https://github.com/sammchardy/python-allcoin>`_ library.

If you use `IDEX <https://idex.market>`_ check out my `python-idex <https://github.com/sammchardy/python-idex>`_ library.

If you use `Quoinex <https://accounts.quoinex.com/sign-up?affiliate=PAxghztC67615>`_
or `Qryptos <https://accounts.qryptos.com/sign-up?affiliate=PAxghztC67615>`_ check out my `python-quoine <https://github.com/sammchardy/python-quoine>`_ library.

If you use `BigONE <https://big.one>`_ check out my `python-bigone <https://github.com/sammchardy/python-bigone>`_ library.

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance?pixel&useReferer
