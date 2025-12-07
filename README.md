# Receipt Analyzer

A FastAPI-based web application that analyzes receipt images using Azure's Document Intelligence AI to extract detailed spending information and provide comprehensive analytics.
![Landing Page](images/landing_page.png)

## Introduction

The application allows users to upload receipt images, which are automatically analyzed using Azure's Document Intelligence service to extract key information such as:

- Merchant name and transaction date
- Total amount, subtotal, and tax information
- Individual line items with quantities and prices
- Discount detection and analysis
- Receipt type and country/region information
![Sample Receipt](images/ikea.png)


All receipts and extracted data are stored in a MongoDB database, enabling users to:

- Build a comprehensive receipt archive
- Track spending patterns over time
- Analyze expenses by merchant and receipt type
- Identify discounts and savings opportunities
- View detailed analytics and insights through an interactive dashboard

The application features both a user-friendly web interface for uploading and viewing receipts, as well as a complete analytics dashboard with spending trends, merchant analysis, and detailed expense breakdowns.

## Installation

### Prerequisites

- Python 3.8 or higher
- MongoDB account (free tier available at https://cloud.mongodb.com)
- Azure account with Document Intelligence resource (free trial available at https://portal.azure.com)

### Step 1: Clone or Download the Project

Download the project files and navigate to the project directory:

```bash
cd receipt-analyzer
```

### Step 2: Install Python Dependencies

Install all required Python packages using pip:

```bash
pip install -r requirements.txt
```

The project requires the following main dependencies:
- **FastAPI**: Modern web framework for building APIs
- **Uvicorn**: ASGI server for running FastAPI applications
- **Motor**: Async MongoDB driver for Python
- **Azure Document Intelligence**: AI service for receipt analysis
- **Pydantic**: Data validation and serialization
- **Python-dotenv**: Environment variable management

### Step 3: Set Up MongoDB

1. Visit https://cloud.mongodb.com and create a free account
2. Create a new cluster (select the free tier)
3. Click "Connect" and select "Connect your application"
4. Copy the connection string (format: `mongodb+srv://username:)

### Step 4: Set Up Azure Document Intelligence

1. Go to https://portal.azure.com and create an account (free trial available)
2. Search for "Document Intelligence" in the search bar
3. Click "Create" and fill in the required details
4. Once created, go to "Keys and Endpoint" in the resource menu
5. Copy:
   - Endpoint URL (format: `https://your-resource.cognitiveservices.azure.com/`)
   - Key 1 (a long alphanumeric string)

### Step 5: Configure Environment Variables

Create a `.env` file in the project root directory with the following content:

```env
# MongoDB connection string
DATABASE_URL=mongodb+srv://username:password@cluster.mongodb.net/receipts_db

# Azure Document Intelligence endpoint
AZURE_DOCUMENT_INTELLIGENCE_ENDPOINT=https://your-resource.cognitiveservices.azure.com/

# Azure Document Intelligence API key
AZURE_DOCUMENT_INTELLIGENCE_KEY=your-api-key-here
```

Replace the placeholder values with your actual credentials.

## Usage

### Running the Application

Start the application using the startup script:

```bash
python startup.py
```

Or run directly with Uvicorn:

```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

The application will be available at `http://localhost:8000`

### Web Interface

1. **Main Upload Page** (`http://localhost:8000`):
   - Drag and drop receipt images or click to upload
   - Supported formats: JPEG, PNG, BMP, TIFF, HEIF
   - Maximum file size: 10MB
   - View all previously uploaded receipts
   - Delete receipts from your history

2. **Analytics Dashboard** (`http://localhost:8000/analytics`):
   - View overall spending summary
   - Analyze monthly spending trends
   - See spending breakdown by receipt type
   - Identify top merchants by spending
   - Review discount analysis and savings
   - Monitor recent receipt activity

### API Endpoints

The application provides the following REST API endpoints:

- `POST /upload` - Upload and analyze a receipt image
- `GET /documents` - Retrieve all receipts (with pagination)
- `GET /documents/{id}` - Retrieve a specific receipt
- `DELETE /documents/{id}` - Delete a receipt
- `GET /analytics/summary` - Get overall spending summary
- `GET /analytics/monthly` - Get monthly spending breakdown
- `GET /analytics/receipt-types` - Get spending by receipt type
- `GET /analytics/dashboard` - Get complete analytics data
- `GET /health` - Health check endpoint

## Code Overview

### Project Structure

```
receipt-analyzer/
├── app.py                  # Main FastAPI application with all endpoints and business logic
├── startup.py              # Application startup script
├── index.html              # Main web interface for receipt upload and viewing
├── analytics.html          # Analytics dashboard with charts and insights
├── requirements.txt        # Python package dependencies
├── .env                    # Environment variables (not included in repo)
├── images/                 # Project images and logos
├── receipt_images/         # Storage for uploaded receipt images
├── uploads/                # Temporary upload directory
├── test_endpoints.py       # API endpoint tests
├── azure.yaml              # Azure deployment configuration
├── web.config              # IIS web server configuration
└── README.md               # This documentation file
```

### Core Components

**app.py** - The main application file containing:

1. **Database Models**:
   - `ReceiptResponse`: Structure for receipt data
   - `ReceiptItemResponse`: Structure for individual line items
   - `SpendingSummary`, `MonthlySpending`, `ReceiptTypeAnalysis`: Analytics models

2. **Analysis Functions**:
   - `analyze_receipt()`: Uses Azure Document Intelligence to extract data from receipt images
   - `extract_receipt_data()`: Parses Azure response into structured format
   - `process_item_pricing()`: Calculates discounts and validates item pricing
   - `save_receipt_image()`: Persists uploaded images to disk

3. **Analytics Functions**:
   - `get_spending_summary()`: Calculates total spending, average per receipt, and tax analysis
   - `get_monthly_spending()`: Groups receipts by month for trend analysis
   - `get_receipt_type_analysis()`: Analyzes spending by receipt category
   - `get_top_merchants()`: Identifies and ranks merchants by spending
   - `get_discount_analysis()`: Calculates discount information and savings
   - `get_recent_activity()`: Retrieves latest receipt uploads

4. **API Endpoints**:
   - Receipt management (upload, view, delete)
   - Analytics endpoints
   - Health check and database testing utilities

**index.html** - Web interface featuring:
   - Drag-and-drop receipt upload area
   - Receipt gallery with image previews
   - Detailed receipt information viewer
   - Receipt deletion functionality
   - Navigation to analytics dashboard

**analytics.html** - Analytics dashboard with:
   - Summary statistics cards
   - Monthly spending line chart
   - Receipt type breakdown pie chart
   - Top merchants bar chart
   - Discount analysis metrics
   - Recent activity timeline

### Key Technologies

- **FastAPI**: Provides high-performance async API endpoints
- **MongoDB**: Stores receipt data and items for long-term access
- **Azure Document Intelligence**: Leverages AI to extract receipt information
- **Motor**: Async MongoDB driver for non-blocking database operations
- **Bootstrap 5**: Responsive web UI framework
- **Chart.js**: Creates interactive analytics visualizations

## Images Folder

All images used in the project are stored in the `images/` folder. This includes:

- Logo files and branding assets
- Sample receipt images for testing
- Merchant logos and icons
- UI assets and icons

When working with images in the project:
- Always reference images from the `images/` folder using relative paths
- Update image paths if you reorganize or rename files
- Add new project images to this folder to keep assets organized
- Note: User-uploaded receipt images are stored separately in the `receipt_images/` folder

To reference images in HTML or CSS:
```html
<!-- Example: referencing an image from the images folder -->
<img src="images/logo.png" alt="Logo">
```

If you add new images or modify existing ones, ensure all references in the HTML and CSS files are updated accordingly.

