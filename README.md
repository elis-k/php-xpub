# XPub

A simple class to derive address and extended public keys.

Coins supported:
- Bitcoin (Bitcoin Cash/Bitcoin SV)
- Ethereum (Binance Smart Chain)
- Tron
- Binance Chain

Supports `xpub`, `tpub`, `zpub` and `vpub` public key formats.

### Install
`composer require elis/xpub`

### Requirements

* PHP >= 7.1
* BCMath or GMP extension

## Usage

```php
# PSR-4 autoloading with composer
use Elis\XPub;

# Create an XPub class instance from an xpub/tpub/zpub/vpub string.
$xpub = XPub::fromString( '<xpub...>' ); // => BIP44 Original
$xpub = XPub::fromString( '<zpub...>' ); // => BIP84 Native SegWit
# You can also specify the address scheme.
$xpub = XPub::fromString( '<xpub...>', XPub::BIP84 );
$xpub = XPub::fromString( '<zpub...>', XPub::BIP44 );

# Derive a child extended public key from it.
$xpub_i = $xpub->derive( $i );
# You can also pass an array to derive a path.
$xpub_i_k = $xpub->derive( [$i, $k]);

# An XPub can be serialized back into a string.
# Pass $asHex = true to serialize into a HEX string, base58 is the default.
$xpub_string = $xpub_i->toString( $asHex = false );

# An XPub can be converted into an address.
# Pass $coin = 'eth' to convert into an ETH address.
# (xpubs are converted into regular addresses, zpubs are converted into segwit addresses.)
$address = $xpub_i->toAddress( $coin = 'btc' );
```

_[See the tests](test/test.php) for example usage._

The `XPub` class also exposes two common hashing methods:

```php
# Get a hash160
$hashed_hex = XPub::hash160( $input_hex );

# Get a double sha256
$hashed_hex = XPub::doubleSha256( $input_hex );
```
