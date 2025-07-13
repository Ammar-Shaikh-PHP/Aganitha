# PubMed Fetcher

A Python tool to fetch PubMed papers and identify non-academic authors from biotech/pharmaceutical companies.

## Features

- Search PubMed papers using the Entrez E-utilities API
- Identify papers with non-academic authors affiliated with biotech/pharma companies
- Export results to CSV format with detailed author and affiliation information
- Command-line interface with support for debug mode and custom output files
- Comprehensive logging and error handling

## Installation

### Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)

### Quick Start (Windows)

For Windows users, you can use the provided batch file for easy setup and execution:

1. Download or clone the repository
2. Navigate to the `pubmed-fetcher` directory
3. Double-click `run-fetcher.bat` to automatically:
   - Install dependencies with Poetry
   - Run the tool with example query "covid vaccine"
   - Save results to `results.csv`

### Manual Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd pubmed-fetcher
```

2. Install dependencies:
```bash
poetry install
```

3. Activate the virtual environment:
```bash
poetry shell
```

## Usage

### Quick Start (Windows Batch File)

The easiest way to get started is using the provided batch file:

```bash
# Simply double-click run-fetcher.bat or run from command line:
run-fetcher.bat
```

This will automatically:
- Install all dependencies
- Run a search for "covid vaccine" papers
- Save results to `results.csv`
- Enable debug mode for detailed logging

### Command Line Interface

The tool provides a command-line interface with the following options:

```bash
# Basic usage
get-papers-list "cancer immunotherapy"

# With custom output file
get-papers-list "CRISPR gene editing" --file results.csv

# With debug logging
get-papers-list "mRNA vaccine" --debug --file vaccine_papers.csv

# Limit number of results
get-papers-list "gene therapy" --max-results 50
```

### Arguments

- `query`: Search query for PubMed papers (required)
- `--file, -f`: Output CSV file path (default: `papers_with_non_academic_authors.csv`)
- `--debug, -d`: Enable debug logging
- `--max-results, -m`: Maximum number of papers to process (default: 100)
- `--help, -h`: Show help message

### Output Format

The CSV output contains the following columns:

- **PubmedID**: PubMed identifier
- **Title**: Paper title
- **Publication Date**: Publication date (YYYY-MM-DD format)
- **Non-academic Author(s)**: Names of authors with non-academic affiliations
- **Company Affiliation(s)**: Biotech/pharma company affiliations
- **Corresponding Author Email**: Email of the corresponding author (if available)

## API Usage

You can also use the `PubMedFetcher` class directly in your Python code:

```python
from pubmed_fetcher import PubMedFetcher

# Initialize the fetcher
fetcher = PubMedFetcher(debug=True)

# Search for papers with non-academic authors
papers = fetcher.fetch_papers_with_non_academic_authors(
    query="cancer immunotherapy",
    max_results=50
)

# Export to CSV
fetcher.export_to_csv(papers, "output.csv")

# Access paper details
for paper in papers:
    print(f"PMID: {paper.pubmed_id}")
    print(f"Title: {paper.title}")
    print(f"Non-academic authors: {len(paper.non_academic_authors)}")
```

## Data Models

### Paper
Represents a PubMed paper with the following attributes:
- `pubmed_id`: PubMed identifier
- `title`: Paper title
- `publication_date`: Publication date
- `authors`: List of authors

### Author
Represents an author with the following attributes:
- `name`: Author name
- `email`: Email address (if available)
- `affiliations`: List of institutional affiliations
- `is_corresponding`: Whether this is the corresponding author

### Affiliation
Represents an institutional affiliation with the following attributes:
- `name`: Institution/company name
- `is_academic`: Whether this is an academic institution
- `is_biotech_pharma`: Whether this is a biotech/pharma company

## Development

### Running Tests

```bash
poetry run pytest
```

### Code Formatting

```bash
poetry run black .
```

### Type Checking

```bash
poetry run mypy .
```

### Linting

```bash
poetry run flake8 .
```

### Example Script

You can also run the included example script:

```bash
poetry run python example.py
```

This will demonstrate the tool's functionality with a sample search query.

## API Rate Limits

The tool respects PubMed's E-utilities API rate limits by adding delays between requests. For production use, consider:

1. Using an API key for higher rate limits
2. Implementing more sophisticated rate limiting
3. Caching results to avoid repeated requests

## Limitations

- Email addresses are not typically available in PubMed XML data
- Affiliation detection relies on keyword matching and may not be 100% accurate
- The tool processes papers sequentially to respect API rate limits

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Run the test suite
6. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Project Structure

```
pubmed-fetcher/
├── pyproject.toml          # Poetry configuration
├── README.md              # This documentation
├── run-fetcher.bat        # Windows batch file for easy execution
├── example.py             # Example usage script
├── .gitignore             # Git ignore file
├── pubmed_fetcher/        # Main package
│   ├── __init__.py        # Package initialization
│   ├── models.py          # Data models (Paper, Author, Affiliation)
│   ├── fetcher.py         # Main PubMedFetcher class
│   └── cli.py            # Command-line interface
└── tests/                # Unit tests
    ├── test_models.py    # Tests for data models
    ├── test_fetcher.py   # Tests for PubMedFetcher
    └── test_cli.py       # Tests for CLI
```

## Acknowledgments

- PubMed E-utilities API for providing access to biomedical literature
- BeautifulSoup for XML parsing
- Poetry for dependency management 
