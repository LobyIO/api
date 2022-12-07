# Loby Scanner Web Service

Loby Scanner, which is available for [iOS](https://apps.apple.com/app/loby-scanner/id1616875931) and [Android](https://play.google.com/store/apps/details?id=com.loby.scannerapp), is commonly used to verify, validate and lookup members, tickets etc. But you can actually overwrite the regular behaviour by implementing a web service, that the scanner will communicate with. This allows for using the scanner to scan various barcodes and customizing the behaviour when doing so.


## High level steps

1. The Scanner recognizes a barcode (QR, Code128, EAN-13 etc.)
2. The Scanner sends the barcode information to the endpoint
3. Your Web Service responds to the barcode
4. The Scanner acts according to your Web Service response


## Authentication

https, bearer etc. json in body

# Receive a scan

The following call will be requested when the scanner has successfully recognized a barcode:

`POST https://your-end-point.com/v1/scan`

The data that will be included:

| Property | Type | Default | Description | Example |
| - | - | - | - | - |
| `scanId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Id of scan | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `scannerId`* | `string` ([UUIDv4](https://en.wikipedia.org/wiki/Universally_unique_identifier)) | | Id of the scanner, that can be looked up | `"bd4a0997-39db-41d9-883a-cdfa83e2101f"` |
| `barcodeType`* | `string` | | Type of barcode (`"EAN-13"`, `"Code-128"`) | `"QRCode"` |
| `barcodeData`* | `string` | | Barcode data | `"93487844"` |

Example of json body:
```json
{
  "id": "bd4a0997-39db-41d9-883a-cdfa83e2101f",
  "barcodeType": "EAN-13",
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
