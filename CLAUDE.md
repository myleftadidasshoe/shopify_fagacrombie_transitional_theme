# Fagacrombie

Store: bc1vrb-hy.myshopify.com, storefront https://fagacrombie.com.

## Product listings: always attach a sales channel

A product created through the Admin API (Shopify connector or `productCreate`)
starts with **no sales channels**. Setting it to Active in admin does not add one,
so it stays invisible on fagacrombie.com (`onlineStoreUrl` is null).

When creating any product draft:

1. Right after `productCreate`, run `publishablePublish` to the **Online Store**
   publication (`gid://shopify/Publication/140140511345`). A draft product stays
   hidden from customers even when published to a channel, so this is safe.
2. After the user approves and the product is set to Active, read it back and
   confirm `onlineStoreUrl` is set and `resourcePublicationsV2` shows Online Store
   as published. Do not report a listing as live until this check passes.

The fagacrombie-product-publisher `publish` step already publishes to the
channel; this rule covers any listing created outside that script.

## Listing format (agreed 2026-10-02, Hollister hoodie is the reference)

| Field | Rule | Example |
|---|---|---|
| `custom.brand` | Brand only, no stray spaces | Hollister |
| `fagacrombie.item_name` | Detail/color · garment, never the brand | Cream · Surf Open Fleece Hoodie |
| Shopify title | Same as item_name, so the brand is never printed twice | Cream · Surf Open Fleece Hoodie |
| SEO title | Brand + garment + key detail + size | Hollister Cream Fleece Hoodie, Surf Open Embroidery, Size M |
| `fagacrombie.size_label` | Tagged size only, no fit text | M |
| `fagacrombie.fits_like` | Exactly `Fits like Medium` style, only when measurements support it | Fits like Medium |
| Measurements | **Inches only**, in `fagacrombie.*_in`, plus `measurement_method`. Never write inches into `*_cm` fields. | chest_in 22.5 |
| Vendor | `Fagacrombie` | |

Do not add final-sale wording to listings: the theme prints
"Pre-owned/vintage, final sale" and "All sales final" on every product page.

The inch display and item-name heading come from the publisher theme patch,
installed on the unpublished theme "Transitional + inches (review)"
(`gid://shopify/OnlineStoreTheme/188847161457`). Until that theme is published,
the live theme only renders `*_cm` fields and `product.title`.
