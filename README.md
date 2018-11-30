# node-dht-peer-crawler
A fast and stable DHT crawler.

## Installation
```bash
$ npm install dht-peer-crawler
```

## Usage
```js
import Crawler from 'dht-peer-crawler'

const crawler = new Crawler()

crawler.on('announce_peer', (infoHashStr, addressStr) => {
  console.log(`got a peer ${addressStr} on ${infoHashStr}`)
})

crawler.start().then(() => {
  console.log('start crawler success')
}, (error) => {
  console.error(`start crawler failed: ${error}`)
})

const signalTraps = ['SIGTERM', 'SIGINT', 'SIGUSR2']

signalTraps.map(type => {
  process.once(type, async () => {
    try {
      await crawler.stop()
      console.log('stop crawler success')
    } finally {
      process.kill(process.pid, type)
    }
  })
})
```

## Test
```bash
$ npm test
```

## API
#### `crawler = new Crawler(listenPort)`

Create a new crawler instance.

#### `crawler.on('announce_peer', [infoHashStr, addressStr])`

Emitted when received an `announce_peer` message.

#### `crawler.on('new_info_hash', [infoHashStr])`

Emitted when find a new `info_hash`.