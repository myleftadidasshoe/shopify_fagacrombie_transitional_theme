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
