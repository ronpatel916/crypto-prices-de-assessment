# Cryptocurrency Analysis Pipeline

This project is a data pipeline that tracks cryptocurrency prices using the CoinMarketCap API.

## Prerequisites

- Python 3.7 or later version
- Pipenv

## Setup

1. **Clone the repository:**

    ```sh
    git clone https://github.com/ronpatel916/crypto-prices-de-assessment.git
    cd crypto-prices-de-assessment
    ```

2. **Install Pipenv:**

    If Pipenv is not installed, install it using pip:

    ```sh
    pip install pipenv
    ```

3. **Install dependencies:**

    Navigate to the project directory and install the required packages using Pipenv:

    ```sh
    pipenv install
    ```

4. **Set up environment variables:**

    In the `.env` file, replace your_coinmarketcap_api_key and add your CoinMarketCap API key:

    ```env
    COINMARKETCAP_API_KEY=your_coinmarketcap_api_key
    ```

## Running the Pipeline

1. **Activate the Pipenv shell:**

    ```sh
    pipenv shell
    ```

2. **Run the orchestration script:**

    The orchestration script will run the entire pipeline, executing each script in sequence.

    ```sh
    python orchestrate_pipeline.py
    ```

## Project Structure

- `.env`: Environment variables file containing the CoinMarketCap API key.

- `orchestrate_pipeline.py`: Orchestrates the execution of the above scripts in sequence.

- `config/`
  - `coins_to_track.csv`: CSV file containing the list of coins to track. This file will serve as a configuration table for analysts to pick and choose which coins they want to follow.

- `scripts/`
  - `fetch_coin_universe.py`: Retrieves the entire universe of coins from the CoinMarketCap API
  - `process_pricing_data.py`: Cleans the coin universe data and filters for the coins we want to track in `config/coins_to_track.csv` as well as BTC if not included, generating a pricing file for these coins
  - `performance_analysis.py`: Analyzes the 24h price performance of the tracked coins relative to Bitcoin 24h price performance.
  - `average_performance.py`: Calculates the average 24-hour performance of each tracked coin relative to Bitcoin over each run.

- `data/`: Directory where the fetched and processed data is stored.


## Notes

- The pipeline is designed to stop if any script fails, preventing downstream scripts from running.
- I made a few design decisions for the sake of simplicity and ease of setup for this assignment that I would not make in a production environment.
    - I have stored the CoinMarketCap API Key in a .env file. In a production environment, I would not store an API Key or any other secret in an environment file, but would use a vault such as AWS Secrets Manager to store these secrets and retrieve the key from there.
    - For the sake of simplicity for this assignment, I chose to orchestrate simply with the subprocess library in orchestrate_pipeline.py. In a production environment, I would opt to a more suitable orchestrationt tool such as Airflow or Prefect. These tools utilize DAGs to organize dependencies and relationships between tasks and dictate how they should be run, and allow for better control over orchestration. The User Interface of these tools are also really convenient to allow for de-bugging data pipelines and manually triggering runs after failures.