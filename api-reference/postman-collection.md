# Postman Collection

The Matra API is available as a Postman collection for easier integration and testing. This collection contains all the endpoints needed to integrate with the Matra platform.

## Collection Overview

The Tradematra V1 Postman collection includes endpoints for:

- User Verification
- Authentication
- Wallet Management
- P2P Trading
- Transaction Management
- Bank Integration

## Using the Collection

### Prerequisites
1. [Download and install Postman](https://www.postman.com/downloads/)
2. Import the collection JSON file into Postman

### Import Steps
1. Open Postman
2. Click on "Import" in the top left corner
3. Upload the `Tradematra V1.postman_collection.json` file
4. Set up your environment variables, especially `{{base_url}}`

### Environment Variables

The collection uses the following environment variables:
- `base_url`: The base URL for the API (e.g., `https://api.matra.io/v1`)
- `token`: Your authentication token after login

## Testing Notes

As noted in the collection description:

> Note that this endpoint is at its early stages and in test mode, meaning some endpoints would not work, e.g., usdc_trc20, usdt_erc20 and usdt_bep20.
>
> Also BSC transactions would fail for accounts older than May 20, 2024.
>
> Test tokens can be gotten from the developer or on the internet. Sending to known accessible accounts would be recommended in order to retrieve test tokens back.
>
> Transaction fee estimation endpoints can be pricey; ensure not to call too frequently (do not call on HTML events relating to user actions in input fields).

## Download Collection

You can download the Postman collection [here](path/to/Tradematra_V1.postman_collection.json). 