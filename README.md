# Northfold Stoneware Shopify Theme

A Shopify Liquid theme for the Northfold Stoneware storefront. The theme uses Shopify product, collection, navigation, cart, and media data as its source of truth. Its Northfold page components are built into Liquid sections and snippets; it does not require a separate JavaScript framework.

## Requirements

- A Shopify store with permission to develop themes.
- Shopify CLI installed and authenticated for that store.
- A current product catalog, collections, product media, and navigation configured in Shopify Admin. Product cards and forms render the data available in the store.

## Local development

From the theme root, start a Shopify development preview:

```sh
shopify theme dev --store your-store.myshopify.com
```

Shopify CLI uploads the local theme as a development theme, starts a local preview, and prints the local preview, Theme Editor, and shareable preview URLs. While the command is running, theme file changes are synced to that development theme. This command does not activate the theme, but it does communicate with the selected store. Use the intended development store and review the target before running it.

To run the local static theme checks:

```sh
shopify theme check
```

If storefront password protection is enabled, set a separate password in Shopify Admin under the online store password protection settings. Do not put the password in this repository, source files, command examples, or Git history. Supply it privately in the submission's designated note field.

## Theme architecture

- `layout/theme.liquid` loads critical CSS, metadata, Shopify's `content_for_header`, the header and footer section groups, and the active page template. It also references `assets/favicon.ico`.
- `sections/header.liquid` renders the announcement bar, configurable Shopify navigation menu, mobile menu, and search, account, and cart links.
- `sections/footer.liquid` renders the configured footer menu, copyright, and optional payment icons.
- `sections/` contains page components. Component styles and JavaScript are colocated in Liquid section files using Shopify's `{% stylesheet %}` and `{% javascript %}` tags where used.
- `snippets/` contains reusable markup, including `northfold-product-card.liquid` and the general image and metadata snippets.
- `templates/` determines which sections appear on each page. `config/settings_schema.json` defines global theme settings; `config/settings_data.json` stores their theme configuration. The local theme metadata in `config/settings_schema.json` still identifies the base theme as “Skeleton” version 0.1.0 by Shopify.
- `locales/en.default.json` contains storefront copy and accessible labels. `locales/en.default.schema.json` contains Theme Editor labels.

## Pages and storefront behavior

### Homepage

`templates/index.json` places the Northfold hero (`sections/hello-world.liquid`) above the featured collection (`sections/northfold-featured-collection.liquid`). The hero can use a merchant-selected collection, featured product, or image, with product/collection image fallbacks. The featured collection uses the selected collection and can feature a selected flagship product; it renders no more than four actual catalog products. On narrow screens, the product row can scroll horizontally. It does not create catalog items to fill empty card positions.

The shared header and footer are rendered through `sections/header-group.json` and `sections/footer-group.json`. Navigation destinations come from Shopify routes or the merchant-selected menu.

### Collection page

`templates/collection.json` uses `sections/collection.liquid`. It displays the Shopify collection title and description, renders product cards through `snippets/northfold-product-card.liquid`, paginates products, and shows pagination when there is more than one page. Cards use responsive Shopify images and prices. Variant swatches appear only when the product's option values have Shopify swatch color or image data.

### Product page

`templates/product.json` uses `sections/northfold-product.liquid` and includes three editable benefit blocks by default. The product section renders Shopify product media, a responsive main image, and a thumbnail strip. The thumbnail strip shows up to three thumbnails at once; its accessible previous/next controls appear when scrolling is needed, and selecting a thumbnail updates the main image in place.

Product options are generated from the product's actual Shopify options and values. Selecting an option requests the updated section from Shopify, then refreshes the selected variant ID, price, availability, and option states without a full-page navigation. Previous in-flight requests are aborted so an older response cannot replace a more recent selection. A subtle busy indicator is attached to the price. The form uses Shopify's native product form and submits the selected variant and quantity to the Shopify cart workflow. The quantity selector can be controlled in the section settings; when hidden, the form submits quantity one.

The merchant can add, remove, and reorder up to three benefit blocks. Each block has a title and description. The section also exposes a benefits heading, background color, and quantity visibility setting. Empty benefit values are omitted from the rendered benefit card.

## Validation and review

Run `shopify theme check` from the theme root after theme changes. For visual review, use a Shopify CLI development preview and inspect the homepage, collection, and product pages at desktop and mobile widths. On the product page, check gallery selection and scrolling, variant price/availability changes, quantity behavior, add-to-cart, and the benefit blocks. On collection and homepage cards, verify links, prices, and any catalog-provided swatches.

This repository does not include a test suite or a project-configured Shopify store/theme target. A live storefront preview requires Shopify authentication and a selected store; create a development preview only when that external store interaction is authorized.
