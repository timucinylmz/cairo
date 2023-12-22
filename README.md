

**A library for secure smart contract development** written in Cairo for [Starknet](https://starkware.co/product/starknet/), a decentralized ZK Rollup.

> **Warning**
> This repo contains highly experimental code.
> It has no code coverage checks.
> It hasn't been audited.
> **Use at your own risk.**

## Usage

> **Warning**
> Expect rapid iteration.
> Some contracts or features are not ready to be deployed.
> Check the **Unsupported** section below.

### Prepare the environment

Simply [install Cairo and scarb](https://docs.swmansion.com/scarb/download).

### Set up your project

Create a new project and `cd` into it.

```bash
scarb new my_project && cd my_project
```

The contents of `my_project` should look like this:

```bash
$ ls

Scarb.toml src
```

### Install the library

Edit `scarb.toml` and add:

```toml
[dependencies]
openzeppelin = { git = "https://github.com/OpenZeppelin/cairo-contracts.git", tag = "v0.8.0" }
```

Build the project to download it:

```bash
$ scarb build

Updating git repository https://github.com/OpenZeppelin/cairo-contracts
Compiling my_project v0.1.0 (~/my_project/Scarb.toml)
Finished release target(s) in 6 seconds
```

### Using the library

Open `src/lib.cairo` and write your contract.

For example, this is how to write an ERC20-compliant contract:

```cairo
#[starknet::contract]
mod MyToken {
    use openzeppelin::token::erc20::ERC20Component;
    use starknet::ContractAddress;

    component!(path: ERC20Component, storage: erc20, event: ERC20Event);

    #[abi(embed_v0)]
    impl ERC20Impl = ERC20Component::ERC20Impl<ContractState>;
    #[abi(embed_v0)]
    impl ERC20MetadataImpl = ERC20Component::ERC20MetadataImpl<ContractState>;
    impl ERC20InternalImpl = ERC20Component::InternalImpl<ContractState>;

    #[storage]
    struct Storage {
        #[substorage(v0)]
        erc20: ERC20Component::Storage
    }

    #[event]
    #[derive(Drop, starknet::Event)]
    enum Event {
        #[flat]
        ERC20Event: ERC20Component::Event
    }

    #[constructor]
    fn constructor(
        ref self: ContractState,
        initial_supply: u256,
        recipient: ContractAddress
    ) {
        let name = 'MyToken';
        let symbol = 'MTK';

        self.erc20.initializer(name, symbol);
        self.erc20._mint(recipient, initial_supply);
    }
}
```


### Cairo

- [Cairo book](https://book.cairo-lang.org/)
- [Cairo language documentation](https://docs.cairo-lang.org/)
- [Starknet book](https://book.starknet.io/)
- [Starknet documentation](https://docs.starknet.io/documentation/)
- [Cairo 1.0 mini-docs](https://github.com/Starknet-Africa-Edu/Cairo1.0)
- [Cairopractice](https://cairopractice.com/)

### Tooling

- [Scarb](https://docs.swmansion.com/scarb)


### Set up the project

Clone the repository:

```bash
git clone git@github.com:OpenZeppelin/cairo-contracts.git
```

`cd` into it and build:

```bash
$ cd cairo-contracts
$ scarb build

Compiling lib(openzeppelin) openzeppelin v0.8.0 (~/cairo-contracts/Scarb.toml)
Compiling starknet-contract(openzeppelin) openzeppelin v0.8.0 (~/cairo-contracts/Scarb.toml)
Finished release target(s) in 16 seconds
```

### Run tests

```bash
scarb test
```

## Security

> ⚠️ Warning! ⚠️
> This project is still in a very early and experimental phase. It has never been audited nor thoroughly reviewed for security vulnerabilities. Do not use in production.

Refer to [SECURITY.md](SECURITY.md) for more details.


