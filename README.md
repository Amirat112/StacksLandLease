# Stacks Land Lease Contract

## Overview
The **STX Virtual Land NFT Rental Contract** is a decentralized smart contract built on the **Stacks blockchain** that enables virtual landowners to mint, list, rent, and manage virtual land NFTs in a secure and transparent manner. This contract follows the **SIP-009 NFT trait**, allowing seamless integration with other NFT platforms on the Stacks ecosystem.

## Features
- **Minting Virtual Land NFTs**: Allows the contract owner to create new virtual land NFTs.
- **Listing Virtual Land for Rent**: Landowners can set rental prices and list NFTs for rent.
- **Renting Virtual Land**: Users can rent available land by making a payment in STX tokens.
- **Ending Rental Agreements**: Automatically removes tenants after the rental period expires.
- **Ownership & Tenant Verification**: Ensures only the rightful owner or tenant can execute rental-related actions.
- **Error Handling & Security**: Implements multiple checks to prevent unauthorized access and invalid transactions.

## Contract Details
- **Contract Name**: `STXVirtualLand`
- **Version**: `1.0.0`
- **Owner**: Contract deployer
- **Max Rental Period**: `52,560` blocks (~1 year)

## Data Structures
### Token Storage (`tokens` Map)
```clarity
{ token-id: uint }
{ owner: principal, uri: (string-utf8 256) }
```
Stores minted NFT details, including:
- `token-id`: Unique identifier for each virtual land NFT
- `owner`: Principal address of the landowner
- `uri`: Metadata URI

### Rental Storage (`rental-details` Map)
```clarity
{ token-id: uint }
{
    tenant: (optional principal),
    rental-start: (optional uint),
    rental-end: (optional uint),
    rental-price: (optional uint)
}
```
Stores rental information, including:
- `tenant`: Address of the current tenant (if rented)
- `rental-start`: Block height when rental started
- `rental-end`: Block height when rental expires
- `rental-price`: Rental fee in STX

## Public Functions
### 1. Mint Virtual Land NFT
```clarity
(define-public (mint-stx-virtual-land (recipient principal) (uri (string-utf8 256)))
```
- **Description**: Mints a new virtual land NFT and assigns it to a recipient.
- **Caller**: Only the contract owner.
- **Returns**: The newly minted `token-id`.

### 2. List Virtual Land for Rent
```clarity
(define-public (list-virtual-land-for-rent (token-id uint) (price uint)))
```
- **Description**: Allows landowners to set a rental price for their land.
- **Caller**: Only the NFT owner.
- **Returns**: `true` if successful.

### 3. Rent Virtual Land
```clarity
(define-public (rent-virtual-land (token-id uint) (rental-period uint)))
```
- **Description**: Allows users to rent an available virtual land NFT by paying STX.
- **Caller**: Any user.
- **Returns**: Updates rental details with the tenant’s information.

### 4. End Virtual Land Rental
```clarity
(define-public (end-virtual-land-rental (token-id uint)))
```
- **Description**: Ends a rental agreement once the period has expired.
- **Caller**: Only the tenant or landowner.
- **Returns**: Resets rental details.

## Read-Only Functions
- `get-owner(token-id)`: Returns the owner of the specified token.
- `get-token-uri(token-id)`: Fetches the metadata URI of the token.
- `get-rental-details(token-id)`: Returns rental details for a specific token.
- `is-land-rented(token-id)`: Checks if a virtual land NFT is currently rented.
- `get-land-tenant(token-id)`: Fetches the current tenant (if any).
- `get-rental-expiry(token-id)`: Returns the block height when rental expires.
- `get-total-lands()`: Returns the total number of minted lands.
- `get-active-rentals()`: Returns the number of currently rented lands.

## Error Handling
| Error Code | Description |
|------------|-------------|
| `u100` | Only contract owner can perform this action |
| `u101` | Caller is not the token owner |
| `u102` | Token ID not found |
| `u103` | Land is already rented |
| `u104` | Land is not rented |
| `u105` | Rental period has expired |
| `u106` | Invalid rental period |
| `u404` | Rental period exceeds maximum limit |
| `u405` | Invalid rental price |
| `u406` | Not authorized to perform this action |
| `u407` | Rental has not yet expired |

## Deployment & Usage
### Deploying the Contract
To deploy the contract, use the Stacks CLI or Stacks Explorer:
```bash
clarinet deploy stx-virtual-land-nft
```

### Minting a Virtual Land NFT
```bash
contract-call? .stx-virtual-land-nft mint-stx-virtual-land SP3... "https://metadata.uri"
```

### Listing Land for Rent
```bash
contract-call? .stx-virtual-land-nft list-virtual-land-for-rent u1 u1000
```

### Renting a Land NFT
```bash
contract-call? .stx-virtual-land-nft rent-virtual-land u1 u5000
```

## Future Improvements
- **Integrate NFT Marketplace Compatibility**: Enable trading of rented lands.
- **Automated Rent Payments**: Implement recurring payments for extended rentals.
- **Smart Contract Upgradability**: Support future enhancements via contract extensions.

## License
This project is open-source and licensed under the **MIT License**.

## Contact
For inquiries or contributions, reach out via **Stacks Discord** or open an issue on **GitHub**.

