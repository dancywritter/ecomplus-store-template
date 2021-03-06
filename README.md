# Storefront theme

## Pages
1. [Storefront theme](./)
2. [Template structure](./structure/)
3. [JavaScript methods API](./methods/)
4. [Implement a search engine](./search/)

## Summary
1. [Introduction](#introduction)
2. [Getting started](#getting-started)
    * [Renderization callback](#renderization-callback)
3. [Guide](#guide)
    * [App main element](#app-main-element)
        * [Specifying store](#specifying-store)
        * [Specifying language](#specifying-language)
    * [Vue instances](#vue-instances)
        * [Store API objects](#store-api-objects)
            * [Object types](#object-types)
            * [Store info sample](#store-info-sample)
            * [Basic product sample](#basic-product-sample)
            * [List of objects](#list-of-objects)
            * [List of categories sample](#list-of-categories-sample)
        * [Search API objects](#search-api-objects)
            * [List items](#list-items)
            * [Products list sample](#products-list-sample)
            * [Search products by keyword](#search-products-by-keyword)
            * [List items from category or brand](#list-items-from-category-or-brand)
            * [List of specific products](#list-of-specific-products)
        * [List products from collection](#list-products-from-collection)
    * [Recommended CSS](#recommended-css)

{% raw %}

# Introduction
This document is intended to list predefined mustache tags,
HTML classes and attributes, and
<a href="./methods/">JavaScript methods</a>
(functions) for layout rendering with
<a href="https://github.com/ecomclub/ecomplus-store-render" target="_blank">ecomplus-store-render</a>.

**It's possible to use any HTML template for E-Com Plus storefront.**
After reading this documentation, you will be able to customize a theme
(editing some elements only) or start a new theme from scratch.

If you want to create a new theme from scratch, be sure to follow
<a href="./structure/">this template structure</a>.

E-Com Plus storefront uses
<a href="https://vuejs.org/v2/guide/" target="_blank">Vue.js 2</a> framework, so
store template specifications follow the
<a href="https://vuejs.org/v2/guide/syntax.html" target="_blank">Vue template syntax</a>.

> After reading the docs,
> <a href="https://github.com/ecomclub/ecomplus-store-template/wiki" target="_blank">visit the Wiki</a>
> to make suggestions or contribute with documentation content and examples.
> If you need help, feel free to
> <a href="https://github.com/ecomclub/ecomplus-store-template/issues" target="_blank">open a new issue</a>.

# Getting started
Your HTML file must include
<a href="https://vuejs.org/v2/" target="_blank">Vue</a>,
<a href="https://github.com/ecomclub/ecomplus-sdk-js" target="_blank">storefront JS SDK</a>
and the
<a href="https://github.com/ecomclub/ecomplus-store-render" target="_blank">layout renderer app</a>.
You can include storefront "all in one" JS file **(recommended)**:

```html
<script src="https://ecvol.com/js/storefront@2/storefront.min.js"></script>
```

Or import the scripts one by one (not recommended):

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/ecomplus-sdk@1/dist/sdk.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/ecomplus-render@2/dist/render.min.js"></script>
```

Then, start the layout rendering with JS below:

```javascript
Ecom.init()
```

The scripts should be loaded after the HTML content of the page,
so you have to put them before `</body>` tag,
we also recommend to load them before other scripts,
such as jQuery lib. Final example:

```html
<body>
  <!-- HTML content -->
  <script src="https://ecvol.com/js/storefront@2/storefront.min.js"></script>
  <script>
    Ecom.init()
  </script>
  <!-- Optionally, other JS -->
</body>
```

## Renderization callback
You probably want to manipulate the rendered elements
with JavaScript (maybe with jQuery),
to work properly, you must do this **after the renderization**.

Send a
<a href="https://developer.mozilla.org/en-US/docs/Glossary/Callback_function" target="_blank">callback function</a>
as first param of _init_ method:

```javascript
Ecom.init(function () {
  // handle events and effects with rendered elements
})
```

# Guide
HTML classes used by this library will be named with the prefix `_ecom-`,
data attributes may be used together with the classes in the elements.

## App main element
Your HTML must have an element
with class `_ecom-store`, it's **required**.

All Vue template must be inside this element,
including all mustache tags and Vue HTML attributes,
so probably it will be the `<body>`, but it's not a rule, you can use
a `<div>`, `<span>` or any other:

```html
<body class="_ecom-store">
  <!-- HTML CODE -->
</body>
```

### Specifying store
By default, store will be defined in function of the site domain,
but you can also use the attributes `data-store` and `data-id`
with your _Store ID_ and _Store Object ID_ respectively:

```html
<div class="_ecom-store" data-store="100" data-id="5a674f224e0dcec2c3353d9d">
```

It's useful if the template is designed for one specific store only,
or if you want to work with multiple stores in the same storefront.

### Specifying language
If you don't want to use the store default language,
you can use the attribute `data-lang`:

```html
<div class="_ecom-store" data-lang="en_us">
```

Use lowercase letters and separate lang of country (if any) by underline,
eg.: `pt_br`, `en_us`, `it`, `es`.

## Vue instances
Each HTML element with class `_ecom-el` will be an
<a href="https://vuejs.org/v2/guide/instance.html" target="_blank">Vue instance</a>.
It represents an object declaration, preceded of a REST API GET request.

Inside `._ecom-el` elements you can use mustache tags and any
<a href="https://vuejs.org/v2/guide/syntax.html" target="_blank">Vue template</a>
attributes.

### Store API objects
<a href="https://ecomstore.docs.apiary.io/" target="_blank">Store API</a> requests
are rendered from `._ecom-el` elements
with the attributes below:

| Attribute   | Description |
| :---:       | :---: |
| `data-type` | Type of object, with one of [these values](#object-types) |
| `data-id`   | API Object ID, the `_id` of the object you are getting from the API _(optional)_ |

In almost all cases, you will not create an HTML for each object,
for example, you will create only one HTML file for all products,
not one per product.
In these cases it's not possible to specify `data-id` (it's dynamic),
let the element without this attribute,
ID will be defined in function of page URL (slug).

The
<a href="https://vuejs.org/v2/guide/instance.html#Data-and-Methods" target="_blank">instance data</a>
will be an object with `body` property, `body` is the object returned from
<a href="https://ecomstore.docs.apiary.io/" target="_blank">Store API</a>,
with the same properties.

#### Object types
Possible values for `data-type`:

| Type          | Object model |
| :---:         | :---: |
| `product`     | [Reference](https://ecomstore.docs.apiary.io/#reference/products/product-object) |
| `brand`       | [Reference](https://ecomstore.docs.apiary.io/#reference/brands/brand-object) |
| `category`    | [Reference](https://ecomstore.docs.apiary.io/#reference/categories/category-object) |
| `collection`  | [Reference](https://ecomstore.docs.apiary.io/#reference/collections/collection-object) |
| `customer`    | [Reference](https://ecomstore.docs.apiary.io/#reference/customers/customer-object) |
| `cart`        | [Reference](https://ecomstore.docs.apiary.io/#reference/carts/cart-object) |
| `order`       | [Reference](https://ecomstore.docs.apiary.io/#reference/orders/order-object) |
| `application` | [Reference](https://ecomstore.docs.apiary.io/#reference/applications/application-object) |
| `store`       | [Reference](https://ecomstore.docs.apiary.io/#reference/stores/store-object) |

#### Store info sample
The example below shows some of the current store information:

```html
<div class="footer _ecom-el" data-type="store">
  <div class="logo" v-if="body.logo">
    <img v-bind:src="body.logo.url" v-bind:alt="body.logo.alt" v-bind:width="width(body.logo)">
  </div>
  <h2 class="store-title"> {{ body.name }} </h2>
  <p> {{ body.description }} </p>
</div>
```

In the example above, Vue data (inside mustache tags and `v-*` attributes) have the
<a href="https://ecomstore.docs.apiary.io/#reference/stores/specific-store" target="_blank">
properties listed here</a>,
following the store object model, but only with public data.

#### Basic product sample
The example below is a simple implementation of a product page:

```html
<div class="_ecom-el" data-type="product">
  <div v-bind:class="'prod-' + body.sku" v-if="body.visible">
    <ul>
      <li v-for="picture in body.pictures">
        <span v-if="picture.big">
          <img v-if="picture.zoom" v-bind:src="picture.big.url" v-bind:alt="picture.big.alt"
            v-bind:data-zoom="picture.zoom.url" />
          <img v-else v-bind:src="picture.big.url" v-bind:alt="picture.big.alt" />
        </span>
      </li>
    </ul>
    <a v-bind:href="body.slug">
      <h1> {{ name() }} </h1>
    </a>
    <p class="price-block">
      <span v-if="onPromotion()">
        {{ body.currency_symbol }}
        <strong class="price"> {{ formatMoney(body.price) }} </strong>
        <span class="base-price"> {{ formatMoney(body.base_price) }} </span>
      </span>
      <span v-else>
        {{ body.currency_symbol }} <strong class="price"> {{ formatMoney(price()) }} </strong>
      </span>
    </p>
    <div v-if="body.available">
      <button v-if="inStock()" class="buy"> Buy </button>
      <div class="no-stock" v-else> Out of stock </div>
    </div>
    <div class="description">
      {{ body.body_html }}
    </div>
  </div>
</div>
```

If you are creating the HTML file for a specific product only,
or embedding one product inside a custom page, you must set `data-id`
with the product ID:

```html
<div class="_ecom-el" data-type="product" data-id="123a5432109876543210cdef">
```

Vue data (inside mustache tags and `v-*` attributes) follows this
<a href="https://ecomstore.docs.apiary.io/#reference/products/product-object" target="_blank">object reference</a>.

Note that you can use similar code for other types of objects (API resources).

#### List of objects
It's possible to render a list of Store API
<a href="https://ecomstore.docs.apiary.io/#reference/categories/all-categories" target="_blank">categories</a>,
<a href="https://ecomstore.docs.apiary.io/#reference/brands/all-brands" target="_blank">brands</a> or
<a href="https://ecomstore.docs.apiary.io/#reference/collections/all-collections" target="_blank">collections</a>,
to do that, you must add the attribute `data-list-all` to the `._ecom-el` element.

This is available only for elements with one of following `data-type`:

| Type          | Object model |
| :---:         | :---: |
| `brand`       | [Reference](https://ecomstore.docs.apiary.io/#reference/brands/all-brands) |
| `category`    | [Reference](https://ecomstore.docs.apiary.io/#reference/categories/all-categories) |
| `collection`  | [Reference](https://ecomstore.docs.apiary.io/#reference/collections/all-collections) |

#### List of categories sample
The example below is a simple implementation of a list of categories,
up to 1000 objects, with random order,
using <a href="https://vuejs.org/v2/guide/list.html" target="_blank">Vue list</a>:

```html
<ul class="_ecom-el" data-type="category" data-list-all="true">
  <li v-for="category in body.result">
    <a v-bind:href="category.slug">
      {{ category.name }}
    </a>
  </li>
</ul>
```

##### Sort list alphabetically
By default, the objects are randomly ordered on the list,
if you want alphabetical order, you can use the `alphabeticalSort` pre-built method:

```html
<li v-for="category in alphabeticalSort(body.result)">
```

### Search API objects
<a href="https://ecomsearch.docs.apiary.io/" target="_blank">Search API</a> requests
are rendered from `._ecom-el` elements
with the `data-type` equal to `items` or `terms`,
and other attributes depending of search case.

The
<a href="https://vuejs.org/v2/guide/instance.html#Data-and-Methods" target="_blank">Vue instance data</a>
will be an object with `body` property, `body` is the object returned from
<a href="https://ecomsearch.docs.apiary.io/" target="_blank">Search API</a>,
with the same properties.

#### List items
To list products, `data-type` must be equal to `items`.
You can get more info and example of returned object from
<a href="https://ecomsearch.docs.apiary.io/#reference/items" target="_blank">API reference</a>.

The `._ecom-el` element must also have the following attributes:

| Attribute         | Description |
| :---:             | :---: |
| `data-type`       | Equal to `items` |
| `data-term`       | Searched keyword _(optional)_ |
| `data-from`       | Results offset number _(optional)_ |
| `data-size`       | Maximum number of results _(optional)_ |
| `data-sort`       | Results ordering, one of [these enumered values](#sort-items-search-result) _(optional)_ |
| `data-ids`        | Filter by specific products IDs separated by `,` _(optional)_ |
| `data-brands`     | Filter by list of brands IDs separated by `,` _(optional)_ |
| `data-categories` | Filter by list of categories IDs separated by `,` _(optional)_ |
| `data-price-min`  | Filter by minimum price _(optional)_ |
| `data-price-max`  | Filter by maximum price _(optional)_ |
| `data-spec-*`     | Filter by product specification _(optional)_ |

#### Products list sample
The example below is a simple implementation of a list of trending products,
using <a href="https://vuejs.org/v2/guide/list.html" target="_blank">Vue list</a>:

```html
<div class="row _ecom-el" data-type="items">
  <div class="col-md-2" v-for="item in body.hits.hits">
    <div v-if="item = item._source" class="item">
      <div v-if="item.pictures && item.pictures[0] && item.pictures[0].normal" class="item-img">
        <img v-bind:src="item.pictures[0].normal.url" v-bind:alt="item.pictures[0].normal.alt" />
      </div>
      <a v-bind:href="item.slug">
        <h3> {{ name(item) }} </h3>
      </a>
      <p class="price-block">
        <span v-if="onPromotion(item)">
          {{ item.currency_symbol }}
          <strong class="price"> {{ formatMoney(item.price) }} </strong>
          <span class="base-price"> {{ formatMoney(item.base_price) }} </span>
        </span>
        <span v-else>
          {{ item.currency_symbol }}
          <strong class="price"> {{ formatMoney(price(item)) }} </strong>
        </span>
      </p>
      <span v-if="item.available">
        <button v-if="inStock(item)" class="buy"> Buy </button>
        <span class="no-stock" v-else> Out of stock </span>
      </span>
    </div>
  </div>
</div>
```

It's possible to specify the maximum number of listed items with `data-size`,
the example below will list up to 12 most popular items:

```html
<div class="_ecom-el" data-type="items" data-size="12">
```

For pagination, you should use data-from (offset) together with data-size (limit).
The example below will list up to 12 items, starting from the 24º,
so, from 24º to 36º:

```html
<div class="_ecom-el" data-type="items" data-from="24" data-size="12">
```

##### Sort items search result
By default, items will be ordered by popularity (number of page views),
but you can use custom sort with `data-sort` attribute:

```html
<div class="_ecom-el" data-type="items" data-sort="1">
```

It must have one of the number values below:

| Enum | Description |
| :--: | :---: |
| `1`  | Sort by sales, products that sells more appear first |
| `2`  | Sort by price ascending, products with lowest price appear first |
| `3`  | Sort by price descending, products with highest price appear first |
| `4`  | Sort by creation date, new products appear first |

#### Search products by keyword
To find products searching by name and/or keywords, you can use `data-term`:

```html
<div class="_ecom-el" data-type="items" data-term="tshirt">
```

<a href="./search/">Here</a>
you can check an complete example of store search engine implementation.

#### Filter items by specifications
It's possible to filter the resultant items list by product specs,
adding attributes of type `data-spec-*`, where the wildcard must be replaced
by the specification (property name):

```html
<div class="_ecom-el" data-type="items" data-spec-colors="Blue" data-spec-size="M">
```

#### List items from category or brand
To list products from specific categories you should use
`data-categories` attribute.

Following example will list items from one category only, but you can
specify more than one by separating categories IDs with `,` (comma):

```html
<div class="_ecom-el" data-type="items" data-categories="f10000000000000000000001">
```

Similar to categories, you can list products from specific brands using
`data-brands` attribute:

```html
<div class="_ecom-el" data-type="items" data-brands="a10000000000000000000001">
```

#### List of specific products
You can also list specific items using the `data-ids` attribute, where
you should put the IDs of respective products separated by comma:

```html
<div class="_ecom-el" data-type="items" data-ids="1234567890abcdef01291510,1234567890abcdef01291511">
```

Note that you can combine the attributes from all above
search and list examples to fit your needs.

### List products from collection
To list products of a collection you have to use both
[Store API](#store-api-objects) and [Search API](#search-api-objects).

The notation is such as the example below:

```html
<div class="_ecom-el" data-type="collection" data-id="c92000000000000000001111"
  data-list="products" data-size="12">
  <a v-bind:href="body.slug">
    <h3 class="coll-name"> {{ body.name }} </h3>
  </a>
  <ul>
    <li v-for="item in body.hits.hits">
      <div v-if="item = item._source" class="item">
        <div v-if="item.pictures && item.pictures[0] && item.pictures[0].normal" class="item-img">
          <img v-bind:src="item.pictures[0].normal.url" v-bind:alt="item.pictures[0].normal.alt" />
        </div>
        <a v-bind:href="item.slug">
          <h4> {{ name(item) }} </h4>
        </a>
        <p> SKU: {{ item.sku }} </p>
        <p class="price-block">
          <span v-if="onPromotion(item)">
            {{ item.currency_symbol }}
            <strong class="price"> {{ formatMoney(item.price) }} </strong>
            <span class="base-price"> {{ formatMoney(item.base_price) }} </span>
          </span>
          <span v-else>
            {{ item.currency_symbol }}
            <strong class="price"> {{ formatMoney(price(item)) }} </strong>
          </span>
        </p>
        <button v-if="inStock(item)" class="buy"> Buy </button>
        <span class="no-stock" v-else> Out of stock </span>
      </div>
    </li>
  </ul>
</div>
```

In these cases you must use `data-type` and `data-id` as expected
for a [Store API object](#store-api-objects).
You also have to use the attribute `data-list` with
value `products`, by doing this, you will be listing the
collection products by IDs such as a [Search API object](#search-api-objects).

In addition, you can use the other
[attributes for a Search API items list](#list-items),
such as the `data-size` used in the above example.

## Recommended CSS
After the renderization of each `._ecom-el` element,
the class `rendered` is automatically added, to hide the non-rendered elements
we recommend to use the following styles:

```css
._ecom-el {
  opacity: 0;
}
._ecom-el.rendered {
  opacity: 1;
}
```

If you want to use fade effect:

```css
._ecom-el {
  opacity: 0;
}
._ecom-el.rendered {
  opacity: 1;
  animation: fadein 1s;
  -moz-animation: fadein 1s;
  -webkit-animation: fadein 1s;
  -o-animation: fadein 1s;
}
@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/* Firefox */
@-moz-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/* Safari and Chrome */
@-webkit-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/* Opera */
@-o-keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
```

Of course you can change the animation time from 1s to what you want.

{% endraw %}
