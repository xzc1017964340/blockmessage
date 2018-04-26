# BlockMessage

BlockMessage is a tool to send and receive encrypted or plain text messages injected into OP_RETURN Bitcoin based blockchains

As encryption algorithm ECIES is used, so standard BitCoin wallet keys can be used.

*Warning: in alpha/PoC state*

### Usage

The blockchain daemon must be started and synchronized. The RPC port must be reachable, let's assume:
+ rpc port 7777
+ rpc user bmsg
+ rpc pass mypass

First of all, export main wallet keys and an address:

`./blockmessage.py --port=7777 --auth=bmsg:mypass -k > mykey`

Now let's send a plain text message to Alice, you only need one of his wallet addresses

`./blockmessage.py --port=7777 --auth=bmsg:mypass -s <BOB_ADDRESS>:[AMOUNT_TO_SEND]`

Alice will be able to read the message by executing:

`./blockmessage.py --port=7777 --auth=bmsg:mypass -r 0`

Now, let's use encryption! `cat mykey` will show your public key,
this is the one you must share with the people who want to secure communicate with you.

If you want to send a message to Bob, you must know in advance his public key and a wallet address. This is how the command would look:

```
./blockmessage.py --port=7777 --auth=bmsg:mypass -i mykey -s RHsvnXGs7iWbyQjJuqgi9VodA7RQWcu2ZM:1 --encrypt --text="Hi Bob, how is your foot doing?" --pubkey=02cc2916ca157ace0009db33b4c0926d23b2ee308a264468454e90db99811869bf
```

Now Bob can read his encrypted messages by executing:

```
 ./blockmessage.py --port=39967 --auth=vocdoni:vocdoni -i mykey -r 0 --encrypt
```

That's it!


### Command line options

```
usage: blockmessage.py [-h] [--host RPC_HOST] [--port RPC_PORT]
                       [--auth RPC_AUTH] [-k] [-i KEYSFILE] [-r READ]
                       [-s SEND] [--fee FEE] [--text TEXT] [--encrypt]
                       [--pubkey DST_PUBKEY]

blockchain tx messaging with encryption

optional arguments:
  -h, --help           show this help message and exit
  --host RPC_HOST      RPC host to connect
  --port RPC_PORT      RPC port to connect
  --auth RPC_AUTH      RPC authentication params (username:password)
  -k                   Print wallet keys and exit
  -i KEYSFILE          File containing own keys (generated with -k)
  -r READ              Read last received messages
  -s SEND              Send a message to address
  --fee FEE            Specify fee for transaction
  --text TEXT          Text to send in message
  --encrypt            Turn on encryption
  --pubkey DST_PUBKEY  If encryption enabled, use destination pubkey
```
