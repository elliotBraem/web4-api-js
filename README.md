# web4-api-js

Simple client library for authentication, view method calls, and contracts calls for apps deployed to [web4](https://github.com/vgrichina/web4).

* /web4/login → `login()`
* /web4/logout → `logout()`
* GET /web4/contract/{contract_id}/{method_name} → `await view()`
* POST /web4/contract/{contract_id}/{method_name} → `await call()`

To see it in action or deploy your own profile to web4, try out [this example](https://github.com/NEARBuilders/profile).

## Installation

```bash
npm install web4-api-js
# or
yarn add web4-api-js
# or
bun add web4-api-js
```


## Usage

```typescript
import { login, logout, isSignedIn, getAccountId, view, call } from 'web4-api-js';

// Authentication
if (!isSignedIn()) {
  login({
    contractId: 'example.near',
    callbackPath: '/dashboard'
  });
}

const accountId = getAccountId();
console.log('Logged in as:', accountId);

// View method (read-only)
const balance = await view(
  'token.near',
  'ft_balance_of',
  { account_id: accountId }
);

// Call method 
await call(
  'token.near',
  'ft_transfer',
  { 
    receiver_id: 'bob.near',
    amount: '1000000000000000000000000'
  },
  {
    deposit: '1', // in yoctoNEAR
    gas: '100000000000000', // 100 TGas
    callbackUrl: '/transfer/success'
  }
);

// Logout
logout();
```

## API Reference

### Authentication

#### `login(options?: LoginOptions): void`

Initiates the web4 login process by redirecting to global login page

- `options.contractId`: Contract requiring access (optional)
- `options.callbackPath`: Path to return to after login (optional)

#### `logout(): void`

Logs out the current user and clears web4 session data.

#### `isSignedIn(): boolean`

Checks if a user is currently signed in.

#### `getAccountId(): string | undefined`

Gets the currently signed in account ID.

#### `getSessionKey(): string | undefined`

Gets the current session's private key.

### Contract Interaction

#### `view<T>(contractId: string, methodName: string, args?: ViewMethodArgs): Promise<T>`

Calls a view method on a web4 contract.

- `contractId`: The contract to call
- `methodName`: The view method to call
- `args`: Arguments to pass to the method (optional)
- Returns: Promise resolving to the method's return value

#### `call<T>(contractId: string, methodName: string, args: ContractCallArgs, options?: ContractCallOptions): Promise<T>`

Calls a method on a web4 contract that can modify state.

- `contractId`: The contract to call
- `methodName`: The method to call
- `args`: Arguments to pass to the method
- `options`: Optional call configuration
  - `gas`: Gas limit for the transaction
  - `deposit`: Amount of NEAR to attach to the call
  - `callbackUrl`: URL to return to after transaction completion
- Returns: Promise resolving to the execution outcome or redirects for signing

## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

<div align="right">
<a href="https://nearbuilders.org" target="_blank">
<img
  src="https://builders.mypinata.cloud/ipfs/QmWt1Nm47rypXFEamgeuadkvZendaUvAkcgJ3vtYf1rBFj"
  alt="Near Builders"
  height="40"
/>
</a>
</div>
