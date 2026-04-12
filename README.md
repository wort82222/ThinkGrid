# ThinkGrid - Sheeel.com Scrapers

Web scraping collection for ThinkGrid-managed categories on **sheeel.com** (Kuwait e-commerce platform).

## Categories

### Cool Items
- **URL**: https://www.sheeel.com/ar/cool-items1.html
- **Scraper**: [cool_items/scraper.py](cool_items/scraper.py)
- **Status**: ✅ Production Ready
- **Features**: Full pagination, multi-image download, S3 upload

## Quick Start

### Run Locally

```bash
cd cool_items
pip install -r requirements.txt
playwright install chromium

# Set environment variables
export AWS_ACCESS_KEY_ID="your_key"
export AWS_SECRET_ACCESS_KEY="your_secret"
export S3_BUCKET_NAME="your_bucket"

python scraper.py
```

### GitHub Actions

The workflow runs automatically:
- **Scheduled**: Every 2 days at 1:00 AM UTC
- **Manual**: Actions tab → "Daily Scrapers - ThinkGrid Categories" → Run workflow

## Output Structure

### S3 Path
```
s3://{bucket}/sheeel_data/
  └── year=YYYY/
      └── month=MM/
          └── day=DD/
              └── cool_items/
                  ├── images/
                  │   └── {product_id}_{index}.{ext}
                  └── excel-files/
                      └── cool_items_YYYYMMDD_HHMMSS.xlsx
```

### Local Path
```
cool_items/data/
  ├── images/
  │   └── {product_id}_{index}.{ext}
  └── cool_items_YYYYMMDD_HHMMSS.xlsx
```

## Data Fields

Each product includes 20+ fields:
- Product ID, name, SKU, prices, availability
- Multiple product images (as arrays)
- Features & specifications (with flattening)
- Box contents & warranty info
- Deal timers, discount badges, purchase counts
- Full product URLs

## Requirements

- Python 3.11
- Playwright 1.40.0
- Ubuntu 22.04 (for GitHub Actions)
- AWS S3 access (optional for local testing)

## GitHub Secrets Required

Set these in your repository settings:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `S3_BUCKET_NAME`

## Adding New Categories

1. Create new category folder: `mkdir {category_name}`
2. Copy scraper template from `cool_items/scraper.py`
3. Update `base_url` and `category` variables
4. Add to workflow matrix in [.github/workflows/main.yml](.github/workflows/main.yml)
5. Test locally, then commit

## Technical Stack

- **Scraping**: Playwright (Chromium headless)
- **Data Processing**: pandas, numpy, openpyxl
- **Cloud Storage**: boto3 (AWS S3)
- **Automation**: GitHub Actions

## Performance

- ~12 seconds per 100 product links
- Parallel image downloads
- Date-partitioned S3 storage
- 7-day artifact retention