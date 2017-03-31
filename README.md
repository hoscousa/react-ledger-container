# React Ledger Container (WIP)

### React component that implements the [Ledger Nano API](https://github.com/LedgerHQ/ledger-node-js-api).

## Features

* App config automatically passed as props
  * `version`
  * `arbitraryDataEnabled`
* Auto-configured `ethLedger` methods injection
  * `signTransaction`
  * `getAddress` (TODO)
  * `signPersonalMessage` (TODO)
* More convenient API
  * Pass transaction object rather than rawTx (and handle compatibility)
  * Automatically self-configures based on device version
  * Backwards compatibility with old firmware version
  * Polls for connectivity, with intelligent poll times for enhanced UX
* `expect` option for validating address
* `onReady` event handler for signing actions

## Example

```javascript
import React, { PropTypes, Component } from 'react';
import LedgerContianer from '@digix/react-ledger-container';

export default class SignLedgerTransaction extends Component {
  constructor(props) {
    super(props);
    this.handleSign = this.handleSign.bind(this);
  }
  handleSign({ signTransaction }) {
    const { txData, account, publishTransaction } = this.props;
    const { kdPath } = account;
    // this generated method will show the tx signing data on the ledger screen
    signTransaction(kdPath, txData).then((signedTx) => {
      publishTransaction({ signedTx });
    })
  }
  render() {
    const { kdPath, address } = this.props.account;
    return (
      <LedgerContianer
        expect={{ kdPath, address }}
        onReady={this.handleSign}
        renderReady={({ config }) => <p>Ready to sign! Using ledger version {config.version}.</p>}
      />
    );
  }
}

SignLedgerTransaction.propTypes = {
  publishTransaction: PropTypes.func.isRequired,
  account: PropTypes.object.isRequired,
  txData: PropTypes.object.isRequired,
};
```

## TODO

* Test with new versions of Ledger firmware
* `getAddress` & `signPersonalMessage`
* Support for Bitcoin API