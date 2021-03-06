---
layout: "google"
page_title: "Google: google_kms_crypto_key"
sidebar_current: "docs-google-kms-crypto-key-x"
description: |-
 Allows creation of a Google Cloud Platform KMS CryptoKey.
---

# google\_kms\_crypto\_key

Allows creation of a Google Cloud Platform KMS CryptoKey. For more information see
[the official documentation](https://cloud.google.com/kms/docs/object-hierarchy#cryptokey)
and
[API](https://cloud.google.com/kms/docs/reference/rest/v1/projects.locations.keyRings.cryptoKeys).

A CryptoKey is an interface to key material which can be used to encrypt and decrypt data. A CryptoKey belongs to a
Google Cloud KMS KeyRing.

~> Note: CryptoKeys cannot be deleted from Google Cloud Platform. Destroying a Terraform-managed CryptoKey will remove it
from state and delete all CryptoKeyVersions, rendering the key unusable, but **will not delete the resource on the server**.

## Example Usage

```hcl
resource "google_kms_key_ring" "my_key_ring" {
  name     = "my-key-ring"
  project  = "my-project"
  location = "us-central1"
}

resource "google_kms_crypto_key" "my_crypto_key" {
  name            = "my-crypto-key"
  key_ring        = "${google_kms_key_ring.my_key_ring.self_link}"
  rotation_period = "100000s"
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The CryptoKey's name.
    A CryptoKey’s name must be unique within a location and match the regular expression `[a-zA-Z0-9_-]{1,63}`

* `key_ring` - (Required) The id of the Google Cloud Platform KeyRing to which the key shall belong.

- - -

* `rotation_period` - (Optional) Every time this period passes, generate a new CryptoKeyVersion and set it as
    the primary. The first rotation will take place after the specified period. The rotation period has the format
    of a decimal number with up to 9 fractional digits, followed by the letter s (seconds). It must be greater than
    a day (ie, 86400).

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

* `self_link` - The self link of the created CryptoKey. Its format is `projects/{projectId}/locations/{location}/keyRings/{keyRingName}/cryptoKeys/{cryptoKeyName}`.

## Import

CryptoKeys can be imported using the CryptoKey autogenerated `id`, e.g.

```
$ terraform import google_kms_crypto_key.my_crypto_key my-gcp-project/us-central1/my-key-ring/my-crypto-key

$ terraform import google_kms_crypto_key.my_crypto_key us-central1/my-key-ring/my-crypto-key
```
