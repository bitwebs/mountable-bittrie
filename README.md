# @web4/mountable-bittrie

A Bittrie wrapper that supports mounting of sub-Bittries.

### Usage
A MountableBittrie can be mounted within another MountableBittrie by using the `mount` command:
```js
const store = chainstore(ram)
const trie1 = new MountableBittrie(store)
const trie2 = new MountableBittrie(store)

trie2.ready(() => {
  trie1.mount('/a', trie2.key, ...)
})
```
Assuming `trie2` has a value 'hello' at `/b/c`:
```js
trie1.get('/a/b/c', console.log) // Will return Buffer.from('hello')
```

A mount can be removed by performing a `del` on the mountpoint :
```js
trie1.del('/a', err => {
  trie1.get('/a/b/c', console.log) // Will print `null`
})
```
### API
`mountable-bittrie` re-exposes the [`bittrie`](https://github.com/bitwebs/bittrie) API, with the addition of the following methods (and a different constructor):

#### `const trie = new MountableBittrie(chainstore, key, opts)`
- `chainstore`: any object that implements the chainstore interface. For now, it's recommanded to use [`random-access-chainstore`](https://github.com/bitwebs/random-access-chainstore)
- `key` is the bittrie key
- `opts` can contain any `bittrie` options

#### `trie.mount(path, key, opts, cb)`
- `path` is the mountpoint
- `key` is the key for the MountableBittrie to be mounted at `path`

`opts` can include:
```
{
  remotePath: '/remote/path', // An optional base path within the mount.
  version: 1                  // An optional checkout version
}
```

_Note: We're still adding support for many bittrie methods. Here's what's been implemented so far:_

- [x] `get`
- [x] `put`
- [x] `del`
- [ ] `batch`
- [x] `iterator`
- [x] `list`
- [x] `createReadStream`
- [ ] `createWriteStream`
- [x] `checkout`
- [x] `watch`
- [ ] `createHistoryStream`
- [x] `createDiffStream`

### License
MIT
