# Loby Scanner Web Service

Loby Scanner, which is available for [iOS](https://apps.apple.com/app/loby-scanner/id1616875931) and [Android](https://play.google.com/store/apps/details?id=com.loby.scannerapp), is commonly used to verify, validate and lookup members, tickets etc. But you can actually overwrite the regular behaviour by implementing a web service, that the scanner will communicate with. This allows for using the scanner to scan various barcodes and customizing the behaviour when doing so.


## High level steps

1. The Scanner recognizes a barcode (QR, Code128, EAN-13 etc.)
2. The Scanner sends the barcode information to the endpoint
3. Your Web Service reads the barcode and returns a response
4. The Scanner acts according to your Web Service response


# Receive a scan

The following call will be requested when the scanner has successfully recognized a barcode:

`POST https://your-end-point.com/v1/scan`

The data that will be included:

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `scanRequestId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Id of scan | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `hmac`* | `string` | | Base 64 encoded HMAC of the id of the scan to verify authenticity | `"LjtublVo7f29ztAZq1aVT1W9lXX7JoBwBp1hrNFGFoF5URBjSXKtAJpn1ejXrG+Mh4pMI0samNlpdmogtBabuw=="` |
| `scannerId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Id of the scanner, that can be looked up | `"51c2df21-a7f3-4b92-a7b2-3d221ced61ae"` |
| `barcodeType`* | `string` | | Type of barcode (`"AZTEC"`, `"CODABAR"`, `"CODE39"`, `"CODE93"`, `"CODE128"`, `"CODE39MOD43"`, `"DATAMATRIX"`, `"EAN13"`, `"EAN8"`, `"INTERLEAVED2OF5"`, `"ITF14"`, `"MAXICODE"`, `"PDF417"`, `"RSS14"`, `"RSSEXPANDED"`, `"UPC_A"`, `"UPC_E"`, `"UPC_EAN"`, `"QR"`) | `"QR"` |
| `barcodeData`* | `string` | | Barcode data | `"93487844"` |

Example of json body:
```json
{
  "scanRequestId": "bd4a0997-39db-41d9-883a-cdfa83e2101f",
  "hmac": "LjtublVo7f29ztAZq1aVT1W9lXX7JoBwBp1hrNFGFoF5URBjSXKtAJpn1ejXrG+Mh4pMI0samNlpdmogtBabuw==",
  "scannerId": "51c2df21-a7f3-4b92-a7b2-3d221ced61ae",
  "barcodeType": "EAN13",
  "barcodeData": "90834098394"
}
```

# Respond to a scan

When you receive a scan `POST` you need to return the following data:

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `success`* | `boolean` | | Whether the scanner should react successfully or fail | `true` |
| `message` | `string` | `null` | Message, that the scanner will show | `"Entrance ticket Ok!"` |
| `icon` | `string` | `null` | Which icon to show. Can be omitted. (`"CHECK_MARK"`, `"WARNING"`) | `"CHECK"` |

Example of response:
```json
{
  "success": false,
  "message": "Entrance ticket already used!",
  "icon": "WARNING"
}
```

## Verifying a Scanner Request

In order to cut down the number of round trips, the requests are send directly from the scanner client. That means that it would be impossible to predict the IP of the incoming requests and thereby only white list certain ip addresses to prevent ddos, exploits etc. Instead, each request includes an HMAC, that the web service can use to verify the authenticity of the request.

To generate the HMAC and verify it in Node, you can do the following:

```js
import crypto from 'crypto'

const hmac = crypto.createHmac('sha512', secret).update(data.scanRequestId).digest('base64')

if (hmac === data.hmac) {

  // All good!

}

```
