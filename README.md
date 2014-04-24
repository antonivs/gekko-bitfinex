# gekko-bitfinex

Bitfinex exchange module for Gekko trading bot

## Installing

This module is designed to be used with [Gekko](https://github.com/askmike/gekko), but has not yet been integrated with it. 
To install this module, you need the following prerequisites:

1. An installed version of Gekko
2. An installed version of the node.js package manager ```npm```
3. An installed version of CoffeeScript, to be able to compile the [Bitfinex API module](https://github.com/gferrin/bitfinex) from npm

Follow these steps to install this module into Gekko:

1. Copy the file [bitfinex.js](https://raw.githubusercontent.com/antonivs/gekko-bitfinex/master/bitfinex.js) into gekko's ```exchanges``` subdirectory.  
This is the Gekko-specific wrapper for the Bitfinex API module.
2. Change to the main gekko directory and install the Bitfinex API module from npm by running: ```npm install bitfinex```
3. The above module is implemented in CoffeeScript and must be compiled to Javascript as follows:
  * From the main gekko directory, change directory as follows: ```cd node_modules/bitfinex``` (use a backslash instead if you're on Windows)
  * Compile the Bitfinex npm module using CoffeScript as follows: ```coffee -c bitfinex.coffee```   This should generate a file named ```bitfinex.js``` in the ```node_modules/bitfinex``` subdirectory.
4. In the Gekko file ```exchanges.js```, add the following as the last entry in the array of exchanges (don't forget to add a comma after the prior entry):

    ```javascript
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
    ```

5. In the Gekko file ```config.js```, edit the ```config.watch``` entry to look like this:

    ```javascript
    config.watch = {
      enabled: true,
      exchange: 'Bitfinex',
      currency: 'USD',
      asset: 'BTC'
    }
    ```

6. Again in ```config.js```, ensure that trading is disabled in the ```config.trader``` section, as follows:

    ```javascript
    config.trader = {
      enabled: false,
    ```

Once the above steps have been successfully completed, you should be able to run Gekko (i.e. by executing ```node gekko```) and it should start retrieving data from Bitfinex.

Trading should not be enabled until you have verified that watching the market and trading advice is working properly.

## Notes and caveats

1. Use at your own risk. This module is currently only very lightly tested, and may still have undiscovered bugs.
2. Only Bitcoin/USD trading is currently supported.  LiteCoin support is not yet implemented.
3. Margin trading is not currently supported.
4. All orders placed are limit orders, which incur a 0.1% fee on Bitfinex.  Volume discounts on fees are not currently taken into account.
5. Handling of partially filled orders may need work. Gekko says that they "should be treated as not filled."  However, limit orders on Bitfinex may never be completely filled, if the market price goes beyond the limit price before the order is filled.

