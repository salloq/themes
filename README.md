# Salloq Themes

A Shopify-compatible theme system for the Salloq ecommerce platform. Build beautiful, customizable storefronts using familiar Liquid templating syntax.

## Overview

Salloq Themes provides a powerful, flexible theming layer that allows developers and designers to create stunning ecommerce experiences. Our theme engine supports Liquid-style templating, making it easy for Shopify developers to migrate existing themes or build new ones from scratch.

**Key Features:**
- Shopify-compatible Liquid templating syntax
- Visual theme customizer with live preview
- Section-based page building
- Multi-language support (i18n)
- Mobile-first responsive design
- One-click theme installation from the Theme Store

## Getting Started

### Quick Start

1. **Download a starter theme** from the [Salloq Theme Store](https://themes.salloq.com)

2. **Extract to your themes directory:**
   ```bash
   unzip salloq-dawn.zip -d storage/themes/your-store-id/
   ```

3. **Activate via Admin Panel:**
   Navigate to `Admin → Themes → Install Theme` and select your theme folder.

### Development Setup

For local development:

```bash
# Clone the starter theme
git clone https://github.com/salloq/theme-starter.git my-theme
cd my-theme

# Install the Salloq CLI (optional)
npm install -g @salloq/cli

# Start development server
salloq theme serve
```

## Theme Structure

Every Salloq theme follows this directory structure:

```
my-theme/
├── assets/                    # Static assets
│   ├── css/
│   │   └── theme.css         # Main stylesheet
│   ├── js/
│   │   └── theme.js          # Main JavaScript
│   └── images/               # Theme images & icons
│       ├── logo.svg
│       ├── favicon.svg
│       ├── theme-preview.svg # Theme store preview
│       └── payment/          # Payment provider icons
│
├── config/
│   └── theme.json            # Theme configuration & settings schema
│
├── layout/
│   └── theme.html            # Base layout (required)
│
├── templates/                 # Page templates
│   ├── index.html            # Homepage
│   ├── page.html             # CMS pages
│   ├── cart/
│   │   └── cart.html
│   ├── checkout/
│   │   └── checkout.html
│   ├── collection/
│   │   └── collection.html
│   ├── customers/
│   │   ├── account.html
│   │   ├── addresses.html
│   │   ├── login.html
│   │   ├── order.html
│   │   ├── orders.html
│   │   └── register.html
│   └── product/
│       └── product.html
│
├── sections/                  # Reusable sections
│   ├── header.html
│   ├── footer.html
│   ├── hero.html
│   ├── featured-collection.html
│   └── newsletter.html
│
├── snippets/                  # Reusable components
│   ├── product-card.html
│   ├── cart-drawer.html
│   ├── mobile-menu.html
│   ├── pagination.html
│   ├── payment-icons.html
│   └── search-modal.html
│
├── locales/                   # Translations
│   ├── en.json
│   └── es.json
│
└── README.md                  # Theme documentation
```

### Required Files

| File | Purpose |
|------|---------|
| `config/theme.json` | Theme metadata and settings schema |
| `layout/theme.html` | Base layout wrapper with `{{ content_for_layout }}` |
| `assets/css/theme.css` | Main stylesheet |

## Configuration

### theme.json

The `config/theme.json` file defines your theme's metadata and customizable settings:

```json
{
  "name": "My Awesome Theme",
  "version": "1.0.0",
  "author": "Your Name",
  "description": "A modern, responsive ecommerce theme",
  "documentation_url": "https://yoursite.com/docs",
  "support_url": "https://yoursite.com/support",
  "thumbnail": "assets/images/theme-preview.png",
  
  "features": [
    "responsive",
    "ajax-cart",
    "product-variants",
    "mega-menu",
    "quick-view"
  ],
  
  "settings_schema": [
    {
      "name": "Colors",
      "settings": [
        {
          "type": "color",
          "id": "color_primary",
          "label": "Primary color",
          "default": "#000000"
        },
        {
          "type": "color",
          "id": "color_background",
          "label": "Background color",
          "default": "#ffffff"
        }
      ]
    },
    {
      "name": "Typography",
      "settings": [
        {
          "type": "font_picker",
          "id": "font_heading",
          "label": "Heading font",
          "default": "Inter"
        },
        {
          "type": "range",
          "id": "font_size_base",
          "label": "Base font size",
          "min": 14,
          "max": 20,
          "step": 1,
          "default": 16,
          "unit": "px"
        }
      ]
    }
  ]
}
```

### Setting Types

| Type | Description |
|------|-------------|
| `text` | Single line text input |
| `textarea` | Multi-line text input |
| `richtext` | Rich text editor with HTML output |
| `color` | Color picker |
| `image_picker` | Image upload/selection |
| `font_picker` | Font family selector |
| `select` | Dropdown selection |
| `checkbox` | Boolean toggle |
| `range` | Numeric slider |
| `url` | URL input |
| `collection` | Collection picker |
| `product` | Product picker |

## Template Syntax

Salloq uses Liquid-compatible templating syntax.

### Variables

```liquid
{{ product.title }}
{{ product.price | money }}
{{ shop.name }}
```

### Filters

```liquid
{{ product.price | money }}                    <!-- Format as currency -->
{{ product.title | truncate: 50 }}             <!-- Truncate text -->
{{ product.created_at | date: '%B %d, %Y' }}   <!-- Format date -->
{{ 'cart.title' | t }}                         <!-- Translation -->
{{ 'logo.png' | asset_url }}                   <!-- Asset URL -->
{{ product.featured_image | image_url: '400x' }}  <!-- Image resize -->
{{ product.description | escape }}             <!-- HTML escape -->
{{ items | json }}                             <!-- JSON encode -->
{{ price | default: 0 }}                       <!-- Default value -->
```

### Control Flow

```liquid
{% if product.available %}
  <button>Add to Cart</button>
{% else %}
  <button disabled>Sold Out</button>
{% endif %}

{% unless cart.empty %}
  <a href="/cart">View Cart ({{ cart.item_count }})</a>
{% endunless %}

{% case product.type %}
  {% when 'physical' %}
    <p>Ships within 3-5 days</p>
  {% when 'digital' %}
    <p>Instant download</p>
  {% else %}
    <p>Contact for details</p>
{% endcase %}
```

### Loops

```liquid
{% for product in collection.products %}
  {% include 'product-card' %}
{% endfor %}

{% for item in cart.items %}
  <div class="cart-item">
    <h3>{{ item.product.title }}</h3>
    <p>{{ item.quantity }} × {{ item.price | money }}</p>
  </div>
{% endfor %}
```

### Includes & Sections

```liquid
<!-- Include a snippet -->
{% include 'product-card' %}
{% include 'product-card', product: featured_product %}

<!-- Render a section -->
{% section 'header' %}
{% section 'featured-collection' %}

<!-- Render content in layout -->
{{ content_for_layout }}
{{ content_for_header }}
```

### Syntax Limitations

The Salloq template renderer supports most Liquid syntax but has some limitations:

**Supported `{% assign %}` patterns:**
```liquid
{% assign my_var = product.title %}
{% assign price_formatted = product.price | money %}
{% assign is_sale = false %}
{% assign items = collection.products %}
```

**Avoid complex chained filter assigns:**
```liquid
<!-- May not render correctly -->
{% assign product_age = 'now' | date: '%s' | minus: product.created_at | date: '%s' %}

<!-- Instead, handle in the controller or use simpler conditionals -->
{% if product.new_badge %}
  <span class="badge">New</span>
{% endif %}
```

## Global Objects

These objects are available in all templates:

| Object | Description |
|--------|-------------|
| `shop` | Store information (name, domain, currency) |
| `settings` | Theme settings from customizer |
| `cart` | Current cart contents |
| `customer` | Logged-in customer (if authenticated) |
| `request` | Current request info (path, host) |
| `page_title` | Current page title |
| `canonical_url` | Canonical URL for current page |

### Page-Specific Objects

| Template | Objects |
|----------|---------|
| `product.html` | `product`, `collection` |
| `collection.html` | `collection`, `collections` |
| `cart.html` | `cart` |
| `customers/account.html` | `customer`, `orders` |
| `customers/order.html` | `order` |
| `page.html` | `page` |

## Sections

Sections are modular, reusable components with their own settings.

### Creating a Section

```html
<!-- sections/hero.html -->
<section class="hero" style="background-image: url('{{ section.settings.image | image_url }}')">
  <div class="hero-content">
    <h1>{{ section.settings.heading }}</h1>
    <p>{{ section.settings.subheading }}</p>
    {% if section.settings.button_link != blank %}
      <a href="{{ section.settings.button_link }}" class="btn">
        {{ section.settings.button_text }}
      </a>
    {% endif %}
  </div>
</section>

{% schema %}
{
  "name": "Hero Banner",
  "settings": [
    {
      "type": "image_picker",
      "id": "image",
      "label": "Background image"
    },
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Welcome to our store"
    },
    {
      "type": "textarea",
      "id": "subheading",
      "label": "Subheading"
    },
    {
      "type": "text",
      "id": "button_text",
      "label": "Button text",
      "default": "Shop Now"
    },
    {
      "type": "url",
      "id": "button_link",
      "label": "Button link"
    }
  ]
}
{% endschema %}
```

### Using Sections

```liquid
<!-- In templates or layout -->
{% section 'hero' %}
{% section 'featured-collection' %}
{% section 'newsletter' %}
```

## Layout

The layout file (`layout/theme.html`) wraps all templates:

```html
<!DOCTYPE html>
<html lang="{{ request.locale.iso_code }}">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{ page_title }} | {{ shop.name }}</title>
  
  <link rel="canonical" href="{{ canonical_url }}">
  <link rel="stylesheet" href="{{ 'theme.css' | asset_url }}">
  
  {{ content_for_header }}
</head>
<body>
  {% section 'header' %}
  
  <main id="main-content">
    {{ content_for_layout }}
  </main>
  
  {% section 'footer' %}
  
  <script src="{{ 'theme.js' | asset_url }}"></script>
  
  {% bolt_hook 'body_end' %}
</body>
</html>
```

## Cart Integration

### Cart Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/cart` | GET | View cart page |
| `/cart/add` | POST | Add item to cart |
| `/cart/update` | POST | Update item quantities |
| `/cart/remove` | POST | Remove item from cart |

### JavaScript Cart API

```javascript
// Add to cart
fetch('/cart/add', {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: `product_id=${productId}&quantity=${qty}&variant_id=${variantId}`
});

// Update cart
fetch('/cart/update', {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: `item_id=${itemId}&quantity=${newQty}`
});

// Get cart JSON
fetch('/cart.json')
  .then(r => r.json())
  .then(cart => console.log(cart));
```

## Translations (i18n)

### Locale Files

Create JSON files in the `locales/` directory:

```json
// locales/en.json
{
  "general": {
    "search": "Search",
    "menu": "Menu",
    "close": "Close"
  },
  "cart": {
    "title": "Your Cart",
    "empty": "Your cart is empty",
    "subtotal": "Subtotal",
    "checkout": "Checkout"
  },
  "product": {
    "add_to_cart": "Add to Cart",
    "sold_out": "Sold Out",
    "quantity": "Quantity"
  }
}
```

### Using Translations

```liquid
<h1>{{ 'cart.title' | t }}</h1>
<button>{{ 'product.add_to_cart' | t }}</button>
```

## Bolt Integration

Themes can integrate with Salloq Bolts (apps/extensions) using hook points:

```liquid
<!-- In layout/theme.html <head> -->
{% bolt_hook 'head' %}

<!-- After product price -->
{% bolt_hook 'product_meta' %}

<!-- Before </body> -->
{% bolt_hook 'body_end' %}
```

Bolts automatically inject their functionality at these hook points without requiring theme modifications.

## Theme Store Submission

To submit your theme to the Salloq Theme Store:

1. **Complete the checklist:**
   - [ ] All required files present
   - [ ] Responsive design (mobile, tablet, desktop)
   - [ ] Cross-browser tested (Chrome, Firefox, Safari, Edge)
   - [ ] Settings schema properly configured
   - [ ] Translations for supported languages
   - [ ] Theme preview image (1200×800px recommended)

2. **Package your theme:**
   ```bash
   zip -r my-theme.zip my-theme/ -x "*.DS_Store" -x "*.git*"
   ```

3. **Submit via the Developer Portal:**
   Visit [developers.salloq.com](https://developers.salloq.com) to submit.

## Available Themes

| Theme | Style | Best For |
|-------|-------|----------|
| **Dawn** | Clean, minimal | General stores |
| **Horizon** | Modern, airy | Fashion, lifestyle |
| **Studio** | Bold, editorial | Art, photography |
| **Origin** | Soft, organic | Wellness, beauty |
| **Publisher** | Classic, literary | Books, education |
| **Harmony** | Calm, balanced | Health, wellness |
| **Pipeline** | Industrial, grid | Tech, industrial |
| **Avante** | High fashion | Luxury, fashion |
| **Athens** | Mediterranean | Food, travel |

## API Reference

### Theme Management Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/admin/api/themes` | List all themes |
| GET | `/admin/api/themes/{id}` | Get theme details |
| POST | `/admin/api/themes` | Upload new theme |
| PUT | `/admin/api/themes/{id}` | Update theme |
| DELETE | `/admin/api/themes/{id}` | Delete theme |
| POST | `/admin/api/themes/{id}/activate` | Activate theme |

### Theme Settings Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/admin/api/themes/{id}/settings` | Get settings & schema |
| PUT | `/admin/api/themes/{id}/settings` | Update settings |
| GET | `/admin/api/themes/{id}/presets` | List presets |
| POST | `/admin/api/themes/{id}/presets` | Save preset |

### Theme Files Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/admin/api/themes/{id}/files` | List all files |
| GET | `/admin/api/themes/{id}/files/content?path=` | Get file content |
| PUT | `/admin/api/themes/{id}/files/content` | Update file |

## Support

- **Documentation:** [docs.salloq.com/themes](https://docs.salloq.com/themes)
- **Theme Store:** [themes.salloq.com](https://themes.salloq.com)
- **Developer Portal:** [developers.salloq.com](https://developers.salloq.com)
- **Community Forum:** [community.salloq.com](https://community.salloq.com)

## License

Salloq themes are released under the MIT License. See [LICENSE](LICENSE) for details.
