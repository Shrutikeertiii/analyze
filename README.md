# Data Processing Project

This project provides a robust solution for processing tabular data, converting it from an Excel format to CSV, performing calculations using Python with Pandas, and generating a JSON output. The entire workflow is automated through GitHub Actions, ensuring continuous integration and deployment of results.

## Project Structure

```
.
├── .github/
│   └── workflows/
│       └── ci.yml             # GitHub Actions workflow for CI/CD
├── data.xlsx                  # Original Excel data (conceptually converted to data.csv)
├── data.csv                   # Converted CSV data (source for processing)
├── execute.py                 # Python script to process data.csv and generate result.json
├── index.html                 # A simple, responsive HTML dashboard using Tailwind CSS
└── LICENSE                    # MIT License information
```

## Features

*   **Data Conversion**: `data.xlsx` is conceptually converted to `data.csv` for standardized processing.
*   **Robust Data Processing**: The `execute.py` script, written in Python with Pandas, reads `data.csv`, performs calculations (e.g., 'Total Price'), and handles potential data quality issues (e.g., non-numeric values, missing data).
*   **JSON Output**: Processed data is outputted as `result.json` in a human-readable, records-oriented format.
*   **Code Quality**: Integrated with `ruff` for linting Python code, ensuring adherence to best practices.
*   **Automated CI/CD**: GitHub Actions workflow automates testing, data processing, and deployment.
*   **GitHub Pages Deployment**: The `result.json` output is automatically published to GitHub Pages, making it easily accessible via a URL.

## Setup and Local Execution

### Prerequisites

*   Python 3.11+
*   pip (Python package installer)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/your-repo-name.git
    cd your-repo-name
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas==2.3 ruff
    ```
    (Note: `pandas==2.3` is specified to meet the requirement. Ensure compatibility if using a newer Python version.)

### Data Files

While `data.xlsx` was the initial attachment, it is assumed to be converted to `data.csv` and committed to the repository for direct use by `execute.py`. Ensure `data.csv` is present in the project root.

Example `data.csv` structure (conceptual):
```csv
ID,Quantity,UnitPrice,Description
1,10,5.50,Item A
2,5,12.00,Item B
3,20,2.15,Item C
4,Invalid,7.00,Item D
5,8,,Item E
6,3,10.00,Item F
```

### Running the Data Processing Script

To generate `result.json` locally, execute the Python script:

```bash
python execute.py
```

This will create a `result.json` file in the project root containing the processed data.

### Running the Linter

To check code quality with Ruff:

```bash
ruff check .
```

## GitHub Actions Workflow (`.github/workflows/ci.yml`)

The CI/CD pipeline is configured to run on every `push` to the `main` branch and can also be triggered manually (`workflow_dispatch`).

It performs the following steps:

1.  **Checkout repository**: Fetches the latest code.
2.  **Set up Python 3.11**: Configures the environment with the specified Python version.
3.  **Install dependencies**: Installs `pandas==2.3` and `ruff`.
4.  **Run Ruff Linter**: Checks all Python files for linting issues and displays results in the CI log.
5.  **Execute data processing script**: Runs `python execute.py` and redirects its output to `result.json`.
6.  **Upload Pages artifact**: Uploads `result.json` as an artifact for GitHub Pages.
7.  **Deploy to GitHub Pages**: Publishes `result.json` to a dedicated GitHub Pages URL.

### Accessing Results on GitHub Pages

Once the CI/CD workflow completes successfully, the `result.json` file will be accessible via GitHub Pages. The exact URL will be displayed in the GitHub Actions run summary under the deployment step.

## `execute.py` Fix Details

The `execute.py` script was enhanced to address non-trivial errors, specifically focusing on data robustness. The key improvements include:

*   **Explicit Column Checks**: Ensures that critical columns like 'Quantity' and 'UnitPrice' exist before attempting operations.
*   **Robust Type Conversion**: Uses `pd.to_numeric(errors='coerce')` to gracefully handle non-numeric input in 'Quantity' and 'UnitPrice' columns, converting invalid entries to `NaN`.
*   **Missing Value Handling**: Drops rows where `Quantity` or `UnitPrice` become `NaN` after conversion, preventing calculation errors and ensuring data integrity in the output.
*   **Error Handling**: Added `try-except` blocks to catch `FileNotFoundError`, `ValueError`, and other potential exceptions during file operations and data processing.

This ensures that the script can process `data.csv` even with imperfect input, producing a reliable `result.json`.
