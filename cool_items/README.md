# Cool Items Scraper

Scrapes product data from https://www.sheeel.com/ar/cool-items1.html

## Features

- **Full pagination support** - Automatically scrapes all pages
- **Multi-image download** - Downloads all product gallery images
- **AWS S3 integration** - Uploads data and images with date partitioning
- **Comprehensive data extraction** - 20+ fields per product including:
  - Product ID, name, SKU, prices, descriptions
  - Multiple product images
  - Features & specifications
  - Box contents & warranty info
  - Deal timers, availability, purchase counts

## Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
playwright install chromium
```

### 2. Configure AWS (Optional)

Set environment variables:

```bash
export AWS_ACCESS_KEY_ID="your_key"
export AWS_SECRET_ACCESS_KEY="your_secret"
export S3_BUCKET_NAME="your_bucket"
```

### 3. Run Scraper

```bash
python scraper.py
```

## Output

### Local Files

- **Excel**: `data/cool_items_YYYYMMDD_HHMMSS.xlsx`
- **Images**: `data/images/{product_id}_{index}.{ext}`

### S3 Structure

```
s3://{bucket}/sheeel_data/
  └── year=YYYY/
      └── month=MM/
          └── day=DD/
              └── cool_items/
                  ├── images/
                  │   ├── 12345_0.jpg
                  │   ├── 12345_1.jpg
                  │   └── ...
                  └── excel-files/
                      └── cool_items_YYYYMMDD_HHMMSS.xlsx
```

## Product Fields

| Field | Description |
|-------|-------------|
| `product_id` | Unique product identifier |
| `name` | Product title |
| `sku` | Stock Keeping Unit |
| `availability` | In stock / out of stock |
| `old_price` | Original price (KWD) |
| `special_price` | Discounted price (KWD) |
| `description` | Product overview |
| `image_urls` | Array of all product image URLs |
| `s3_image_paths` | Array of S3 URLs after upload |
| `features_specs` | Array of product features |
| `feature_spec_0`, `feature_spec_1`, ... | Flattened features |
| `box_contents` | Box contents info |
| `warranty` | Warranty information |
| `deal_time_left` | Flash deal countdown |
| `discount_badge` | Discount percentage |
| `times_bought` | Purchase count |
| `url` | Product detail page URL |
| `scraped_at` | Timestamp of scraping |

## Usage in GitHub Actions

Add to `.github/workflows/main.yml`:

```yaml
- name: cool_items
  display_name: Cool Items
```

## Technical Details

- **Browser**: Playwright Chromium (headless)
- **Wait strategy**: `networkidle` for SPA, selector-based for elements
- **Pagination**: Automatic detection and scraping
- **Error handling**: Continues on individual product failures
- **Image handling**: Content-Type detection for proper extensions
