
### Summary:

- Wallet: C5X626sVSngZHFj9jKjhTJoyciaEbCYFTjtknaBEgtbz

- Token: 63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH
- Token Account: E4k8rntV1srRXDM5cpb945xsJSoH7EcHprtUBb4QsGAA

- NFT Token: C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY
- NFT Account: GuVR9qxU8U7mxC4BFJHVFsWUDRw3tTeQZ5BNC99dEu6K

- Token & NFT Program: TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA


************************************************************************
************************************************************************

---

### Test NFT Wallet

> This is optional but recommended for keeping your tests separate from other activities. Use the following command to create a new keypair:

solana-keygen new --no-bip39-passphrase -o ~/test-keypair.json

```
Generating a new keypair
Wrote new keypair to /Users/balance-guven/test-keypair.json
===============================================================================
pubkey: C5X626sVSngZHFj9jKjhTJoyciaEbCYFTjtknaBEgtbz
===============================================================================
Save this seed phrase to recover your new keypair:
bicycle tunnel weekend neglect valley gold wink night vacant drink laptop owner
===============================================================================
```

************************************************************************

> Next, adjust the Solana configuration to use this new keypair:

solana config set --keypair ~/test-keypair.json

```
Config File: /Users/balance-guven/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com
WebSocket URL: wss://api.mainnet-beta.solana.com/ (computed)
Keypair Path: /Users/balance-guven/test-keypair.json
Commitment: confirmed
```

************************************************************************

> Set the Solana CLI to interact with the devnet cluster:

solana config set --url https://api.devnet.solana.com

```
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /Users/balance-guven/test-keypair.json
Commitment: confirmed
```

************************************************************************

> The new account has no SOL, so you need to request an airdrop:

solana airdrop 1

```
Requesting airdrop of 1 SOL

Signature: 2x8ZF1EbdUTwpmnK1R5c4NAQcw9s4QYVN19mrAbM41rcMH4AuKbWUzXJz3eZuuUXaGRphSpwAGZt7xWPvHDSaH19

1 SOL
```

************************************************************************
************************************************************************

### Create SPL Token

> Now, create a new SPL token:

spl-token create-token

```
Creating token 63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH under program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA

Address:  63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH
Decimals:  9

Signature: 4MiPVWDja9b9g3btrBB76Hp9jcHk2MEgtiHehAgR3bN3n187v74VYksL8eaz6BvrVbJCv9pcYKocqaKYSKMpFPQZ
```

************************************************************************

> Next, create an account that is eligible to hold the tokens you just created:

spl-token create-account 63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH

```
Creating account E4k8rntV1srRXDM5cpb945xsJSoH7EcHprtUBb4QsGAA

Signature: 4XRr9AaaHR5yjCZJKdapM8pH5goDbgWiTmYBrmWVFhd8sWsXpEXz9fwc5jSG8G7QYK3gdbgeubhHMNxJZVXJVXtG
```

************************************************************************


> Mint a new token into this account:

spl-token mint 63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH 1

```
Minting 1 tokens
Token: 63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH
Recipient: E4k8rntV1srRXDM5cpb945xsJSoH7EcHprtUBb4QsGAA

Signature: 3n2b73rror3RSt7sZkjreJZApKvS4f7JndYu63kEjyzLS1KXmJpugnxygBjdrExgYRYsPPUdnfngDfuAU31jndB9
```

https://explorer.solana.com/address/63i71qd3aR2WPBQgguJrLCAegsVnACgdkDZVF1r1BZwH/tokens?cluster=devnet

************************************************************************

> To create non-fungible tokens (NFTs), you need to specify 0 decimals when creating the token:

spl-token create-token --decimals 0

```
Creating token C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY under program TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA

Address:  C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY
Decimals:  0

Signature: 42p4MWsnsqKP7VA9Guum32Lvwnqrx26LuRZp9YbY95gY55i6j3veL4u8otLApP3CPZ7fR5STwgCm5zd8ggV4H3nT
```

************************************************************************
************************************************************************


### Create NFT Account

> Then, create an account for this NFT:

spl-token create-account C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY

```
Creating account GuVR9qxU8U7mxC4BFJHVFsWUDRw3tTeQZ5BNC99dEu6K

Signature: 2ahgMKEzK86HzciKajNCtys8ELyLVRgmhBDNCNipRSyMAZV31dbio9w2h53TRPfArdpiuZNcBAnqrfyRQ3s8xTAE
```

************************************************************************


> Finally, to ensure that no one else can mint more of your NFT, disable the mint authority:

spl-token authorize C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY mint --disable

```
Updating C7Y4hUs6bxEP2x8YfxVWSC5fMwNMwos2A6m9JYT7eMWY
Current mint authority: C5X626sVSngZHFj9jKjhTJoyciaEbCYFTjtknaBEgtbz
New mint authority: disabled

Signature: 2WzhiwQaDrYcJivvxnB41YK7UzksTqia9fDafTA9jbuFHTK6QwBNhHiFEi71PqiFQAR2hTUVBGTQ3edrVi1rzrT5
```

Good Luck :)
