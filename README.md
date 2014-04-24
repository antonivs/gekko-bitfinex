# gekko-bitfinex

Bitfinex exchange module for Gekko trading bot

## Installing

This code is designed to be used with [Gekko](https://github.com/askmike/gekko), but has not yet been integrated.  
To install it, you need the following prerequisites:

1. An installed version of Gekko.
2. An installed version of the node.js package manager ```npm```.
3. An installed version of CoffeeScript, to be able to compile the [Bitfinex module](https://github.com/gferrin/bitfinex) from npm.

Follow these steps to install this module into Gekko:

1. Copy the file [bitfinex.js](https://raw.githubusercontent.com/antonivs/gekko-bitfinex/master/bitfinex.js) into gekko's ```exchanges``` subdirectory.
2. Change to the main gekko directory and install the Bitfinex module from npm by running: ```npm install bitfinex```
3. The above module is implemented in CoffeeScript and must be compiled to Javascript as follows:
  * Change to the node_modules subdirectory within the gekko directory, e.g. ```cd node_modules/bitfinex``` (use a backslash instead if you're on Windows)
  * Compile the npm module with CoffeScript as follows: ```coffee -c bitfinex.coffee``` .  This should generate a file named ```bitfinex.js```.
4. In the Gekko file ```exchanges.js```, add the following as the last entry in the array of exchanges:

    {
      name: 'Bitfinex',
      slug: 'bitfinex',
      direct: false,
      infinityOrder: false,
      currencies: ['USD'],
      assets: ['BTC'],
      markets: [
        {
          pair: ['USD', 'BTC'], minimalOrder: { amount: 0.01, unit: 'currency' }
        }
      ],
      requires: ['key', 'secret'],
      providesHistory: false
    }

5. In the Gekko file ```config.js```, edit the ```config.watch``` entry to look like this:

    config.watch = {
      enabled: true,
      exchange: 'Bitfinex', // 'MtGox', 'BTCe', 'Bitstamp', 'cexio' or 'kraken'
      currency: 'USD',
      asset: 'BTC'
    }

6. Again in ```config.js```, ensure that trading is disabled in the ```config.trader``` section, as follows:
    config.trader = {
      enabled: false,

Once the above steps have been successfully completed, you should be able to run Gekko (i.e. ```node gekko```) and it should start retrieving data from Bitfinex.

Trading should not be enabled until you have verified that watching the market and trading advice is working properly.

## Notes and caveats

1. Use at your own risk. This module is currently only very lightly tested, and may still have undiscovered bugs.
2. Margin trading is not currently supported.
3. Only Bitcoin/USD trading is currently supported.  LiteCoin support is not yet implemented.
4. All orders placed are limit orders, which incur a 0.1% fee on Bitfinex.  Volume discounts on fees are not currently taken into account.
5. Handling of partially filled orders may need work. Gekko says that they "should be treated as not filled."  However, limit orders on Bitfinex may never be completely filled, if the market price goes beyond the limit price before the order is filled.

