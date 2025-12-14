# Affiliate Content System

Complete affiliate content implementation system for WordPress sites. Creates FTC-compliant, conversion-optimized affiliate content templates.

## Purpose

This skill handles **implementing** affiliate content on pages (the affiliate-hunter-agent handles **finding** programs). Use this for:
- Product comparison tables
- Single product review blocks
- Rating systems
- Pros/cons layouts
- Price displays
- Affiliate disclosures
- CTA button patterns
- Amazon PAAPI integration

---

## Amazon Product Advertising API (PAAPI) Integration

### Setup Requirements

**Credentials (from Nick's memory):**
```
Tag: familyflourish-20 (for Family-Flourish.com)
Access Key: AKPAS7WQE01765077425
Secret Key: HHgtdOX4VyYpXS5JXoeNyU9IdeDiCPeg8yw/qCGL
```

**Note:** Each site should have its own affiliate tag for tracking.

### PAAPI Endpoints

```
ItemLookup: Get product details by ASIN
ItemSearch: Search products by keywords
BrowseNodeLookup: Get category info
```

### WordPress Integration Options

**Option 1: Content Egg Plugin (Recommended)**
- Already in plugin stack
- Handles PAAPI calls
- Caches product data
- Auto-updates prices

**Option 2: Custom PAAPI Integration**
```php
// functions.php - PAAPI Helper
function get_amazon_product($asin, $affiliate_tag) {
    $params = [
        'Service' => 'ProductAdvertisingAPI',
        'Operation' => 'GetItems',
        'ItemIds' => $asin,
        'PartnerTag' => $affiliate_tag,
        'PartnerType' => 'Associates',
        'Resources' => [
            'ItemInfo.Title',
            'ItemInfo.Features',
            'Images.Primary.Large',
            'Offers.Listings.Price'
        ]
    ];
    // AWS Signature V4 authentication
    // Return product data
}
```

---

## FTC Compliance Requirements

### Disclosure Text (Required)

**Short version (inline):**
```html
<span class="affiliate-disclosure-inline">
  (As an Amazon Associate, we earn from qualifying purchases)
</span>
```

**Full version (page top):**
```html
<div class="affiliate-disclosure-full">
  <p><strong>Affiliate Disclosure:</strong> This page contains affiliate links. 
  If you purchase through these links, we may earn a commission at no extra cost 
  to you. We only recommend products we genuinely believe in.</p>
</div>
```

### Placement Rules

1. **Above the fold** - Before any affiliate links
2. **Near affiliate links** - Within reasonable proximity
3. **Clear and conspicuous** - Readable, not hidden
4. **Every page with affiliate links** - No exceptions

### CSS for Disclosures

```css
.affiliate-disclosure-full {
  background: var(--color-surface);
  border-left: 4px solid var(--color-accent);
  padding: 1rem 1.5rem;
  margin-bottom: 2rem;
  font-size: 0.875rem;
  color: var(--color-text-muted);
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
}

.affiliate-disclosure-inline {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  font-style: italic;
}
```

---

## Component Templates

### 1. Product Comparison Table

**Use case:** "Best X for Y" roundup articles

```html
<!-- wp:group {"className":"comparison-table-wrapper"} -->
<div class="comparison-table-wrapper">
  <table class="comparison-table">
    <thead>
      <tr>
        <th>Product</th>
        <th>Best For</th>
        <th>Rating</th>
        <th>Price</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr class="comparison-row editor-choice">
        <td class="product-cell">
          <span class="badge badge-best">Editor's Choice</span>
          <img src="product-image.jpg" alt="Product Name" />
          <span class="product-name">Product Name Pro</span>
        </td>
        <td class="best-for-cell">Power users who need advanced features</td>
        <td class="rating-cell">
          <span class="rating">4.8</span>
          <span class="stars">★★★★★</span>
        </td>
        <td class="price-cell">
          <span class="price">$299</span>
          <span class="price-note">Check price</span>
        </td>
        <td class="action-cell">
          <a href="#" class="btn-affiliate">View on Amazon</a>
        </td>
      </tr>
      <!-- More rows -->
    </tbody>
  </table>
</div>
<!-- /wp:group -->
```

**CSS:**
```css
.comparison-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.9375rem;
}

.comparison-table th {
  background: var(--color-surface);
  padding: 1rem;
  text-align: left;
  font-weight: 600;
  border-bottom: 2px solid var(--color-border);
}

.comparison-table td {
  padding: 1.25rem 1rem;
  border-bottom: 1px solid var(--color-border);
  vertical-align: middle;
}

.comparison-row.editor-choice {
  background: linear-gradient(90deg, 
    rgba(var(--color-accent-rgb), 0.05) 0%, 
    transparent 100%);
}

.badge-best {
  display: inline-block;
  background: var(--color-accent);
  color: white;
  font-size: 0.6875rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 0.25rem 0.5rem;
  border-radius: var(--radius-sm);
  margin-bottom: 0.5rem;
}

.product-cell img {
  width: 80px;
  height: 80px;
  object-fit: contain;
  margin: 0.5rem 0;
}

.product-name {
  display: block;
  font-weight: 600;
  color: var(--color-text);
}

.rating {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--color-text);
}

.stars {
  color: var(--color-rating, #FFD700);
  letter-spacing: -2px;
}

.price {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-primary);
}

.btn-affiliate {
  display: inline-block;
  background: var(--color-primary);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: var(--radius-md);
  font-weight: 600;
  text-decoration: none;
  transition: all 0.2s ease;
}

.btn-affiliate:hover {
  background: var(--color-primary-dark);
  transform: translateY(-2px);
}
```

### 2. Single Product Review Block

**Use case:** In-depth single product reviews

```html
<!-- wp:group {"className":"product-review-block"} -->
<div class="product-review-block">
  <div class="product-header">
    <div class="product-image">
      <img src="product.jpg" alt="Product Name" />
    </div>
    <div class="product-info">
      <h3 class="product-title">Product Name Pro Max</h3>
      <div class="product-rating">
        <span class="rating-score">4.7</span>
        <span class="rating-stars">★★★★★</span>
        <span class="rating-count">(2,451 reviews)</span>
      </div>
      <div class="product-price">
        <span class="current-price">$279.99</span>
        <span class="original-price">$349.99</span>
        <span class="discount-badge">20% OFF</span>
      </div>
      <a href="#" class="btn-primary-affiliate">Check Price on Amazon</a>
    </div>
  </div>
  
  <div class="product-verdict">
    <h4>Our Verdict</h4>
    <p>Brief 2-3 sentence summary of the product and who it's best for.</p>
  </div>
  
  <div class="product-specs">
    <h4>Key Specifications</h4>
    <dl class="specs-list">
      <dt>Battery Life</dt><dd>10 hours</dd>
      <dt>Weight</dt><dd>245g</dd>
      <dt>Connectivity</dt><dd>Bluetooth 5.3, Wi-Fi</dd>
    </dl>
  </div>
  
  <div class="product-pros-cons">
    <div class="pros">
      <h4>✓ What We Like</h4>
      <ul>
        <li>Excellent battery life</li>
        <li>Premium build quality</li>
        <li>Accurate sensors</li>
      </ul>
    </div>
    <div class="cons">
      <h4>✗ Could Be Better</h4>
      <ul>
        <li>Premium price point</li>
        <li>Learning curve for app</li>
      </ul>
    </div>
  </div>
</div>
<!-- /wp:group -->
```

### 3. Quick Pick Boxes

**Use case:** "Best budget" / "Best premium" callouts

```html
<!-- wp:group {"className":"quick-pick-box best-overall"} -->
<div class="quick-pick-box best-overall">
  <span class="pick-badge">🏆 Best Overall</span>
  <div class="pick-content">
    <img src="product.jpg" alt="Product" class="pick-image" />
    <div class="pick-details">
      <h4 class="pick-title">Product Name</h4>
      <p class="pick-reason">Best for most users who want reliability and features.</p>
      <div class="pick-price">$249</div>
      <a href="#" class="pick-cta">Check Price →</a>
    </div>
  </div>
</div>
<!-- /wp:group -->
```

**Badge variants:**
- 🏆 Best Overall
- 💰 Best Value  
- ⭐ Premium Pick
- 🔥 Most Popular
- 💡 Editor's Choice
- 🆕 Best New Release

### 4. Rating Breakdown Component

```html
<div class="rating-breakdown">
  <div class="overall-score">
    <span class="score-number">4.7</span>
    <span class="score-label">Overall Score</span>
  </div>
  <div class="score-bars">
    <div class="score-item">
      <span class="score-category">Performance</span>
      <div class="score-bar">
        <div class="score-fill" style="width: 95%"></div>
      </div>
      <span class="score-value">9.5</span>
    </div>
    <div class="score-item">
      <span class="score-category">Value</span>
      <div class="score-bar">
        <div class="score-fill" style="width: 80%"></div>
      </div>
      <span class="score-value">8.0</span>
    </div>
    <div class="score-item">
      <span class="score-category">Features</span>
      <div class="score-bar">
        <div class="score-fill" style="width: 90%"></div>
      </div>
      <span class="score-value">9.0</span>
    </div>
    <div class="score-item">
      <span class="score-category">Ease of Use</span>
      <div class="score-bar">
        <div class="score-fill" style="width: 85%"></div>
      </div>
      <span class="score-value">8.5</span>
    </div>
  </div>
</div>
```

---

## Site-Specific Affiliate Styles

### For PulseGearReviews (Fitness - Energy Red)
```css
:root {
  --affiliate-accent: #FF4136;
  --affiliate-badge-bg: #FF4136;
  --affiliate-btn-bg: #FF4136;
  --affiliate-btn-hover: #D63030;
}
```

### For WearableGearReviews (Health - Teal)
```css
:root {
  --affiliate-accent: #1ABC9C;
  --affiliate-badge-bg: #1ABC9C;
  --affiliate-btn-bg: #1ABC9C;
  --affiliate-btn-hover: #16A085;
}
```

### For SmartHomeGearReviews (Trust - Blue)
```css
:root {
  --affiliate-accent: #0066CC;
  --affiliate-badge-bg: #0066CC;
  --affiliate-btn-bg: #0066CC;
  --affiliate-btn-hover: #004499;
}
```

---

## CTA Button Patterns

### Primary Affiliate CTA
```css
.btn-affiliate-primary {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: var(--affiliate-btn-bg);
  color: white;
  padding: 0.875rem 1.75rem;
  border-radius: var(--radius-md);
  font-weight: 600;
  font-size: 1rem;
  text-decoration: none;
  transition: all 0.2s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.btn-affiliate-primary:hover {
  background: var(--affiliate-btn-hover);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.btn-affiliate-primary::after {
  content: "→";
  transition: transform 0.2s ease;
}

.btn-affiliate-primary:hover::after {
  transform: translateX(4px);
}
```

### Amazon-Specific CTA
```css
.btn-amazon {
  background: #FF9900;
  color: #111;
}

.btn-amazon:hover {
  background: #E88B00;
}

.btn-amazon::before {
  content: "";
  display: inline-block;
  width: 20px;
  height: 20px;
  background: url('amazon-icon.svg') no-repeat center;
  margin-right: 0.5rem;
}
```

---

## Content Egg Integration

### Shortcode Examples
```
[content-egg module=Amazon template=grid]
[content-egg module=Amazon template=list]
[content-egg module=Amazon template=item next=1]
[content-egg-block module=Amazon template=custom/comparison]
```

### Custom Templates Location
```
/wp-content/themes/blocksy-child/content-egg-templates/
├── comparison.php
├── product-box.php
├── price-alert.php
└── quick-pick.php
```

---

## Schema Markup for Products

### Product Review Schema
```json
{
  "@context": "https://schema.org",
  "@type": "Review",
  "itemReviewed": {
    "@type": "Product",
    "name": "Product Name",
    "image": "https://example.com/image.jpg",
    "brand": {
      "@type": "Brand",
      "name": "Brand Name"
    }
  },
  "reviewRating": {
    "@type": "Rating",
    "ratingValue": "4.7",
    "bestRating": "5"
  },
  "author": {
    "@type": "Person",
    "name": "Review Author"
  }
}
```

### Aggregate Rating (for roundups)
```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Best Smart Speakers 2025",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.5",
    "reviewCount": "15"
  }
}
```

---

## Integration with n8n Workflows

### Price Update Workflow
```
Trigger: Daily at 6 AM
→ Get all products from database
→ Loop: Query Amazon PAAPI for each
→ Compare prices
→ Update WordPress if changed
→ Send alert if price dropped >10%
```

### Out of Stock Alert
```
Trigger: Daily
→ Check product availability via PAAPI
→ If out of stock → Update post status
→ Notify for manual review
```

---

## Files Reference

```
affiliate-content-system/
├── SKILL.md (this file)
├── templates/
│   ├── comparison-table.html
│   ├── product-review-block.html
│   ├── quick-pick-box.html
│   └── rating-breakdown.html
├── css/
│   ├── affiliate-base.css
│   ├── affiliate-pulsegear.css
│   ├── affiliate-wearable.css
│   └── affiliate-smarthome.css
├── php/
│   ├── amazon-paapi-helper.php
│   ├── content-egg-custom-templates/
│   └── schema-generator.php
└── n8n/
    └── price-update-workflow.json
```
