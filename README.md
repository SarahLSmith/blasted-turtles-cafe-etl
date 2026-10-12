# Cafe local ETL

A command-line app for managing a small cafe: menu browsing, product and
courier management, basket building, and order handling. Data lives in
CSV and JSON files, and a setup script can load the same tables into a
remote Postgres database.

Built as a team mini project on the DE-NAT4 data engineering bootcamp.
The app and data files are the team's; this fork keeps the team history.

## At a glance

- **Problem:** handle a cafe's products, couriers, orders and inventory
  from the command line.
- **Data:** CSV files for products, couriers, orders and inventory, plus a
  JSON menu definition (`menu/Menus.Json`).
- **Stack:** Python 3, `psycopg2` and `python-dotenv` for the database
  setup, SQL for the schema, Docker Compose for the remote database.

## How it's built

- `main.py` runs the `CafeApp` command-line menus (main menu, products,
  couriers, order management, basket).
- `utils.py` loads and saves the CSV and JSON files.
- `csv/` holds the source data. `menu/Menus.Json` defines the menu screens.
- `database/` has the SQL that creates the tables and loads the data
  (`01_create_tables.sql`, `02_insert_data.sql`), and `setup.py`, which
  starts the remote Docker stack, waits for Postgres, and runs both files.

## How to run

Local menu app:

```bash
python3 main.py
```

Database setup (needs a `.env` file with the connection settings, and
SSH access to the remote server; `.env` is not committed):

```bash
pip install -r database/requirements.txt
cd database && python setup.py
```

## Known gaps

- The main loop in `CafeApp.run` runs once, because of a `break` after
  the first call. Remove the `break` to keep the menu running.
- `run` loads `./csv/inventory.csv`, but the file is `csv/Inventory.csv`.
  This works on Windows, which ignores case, but not on Linux.
- `run` has a TODO to save products and couriers back to CSV on exit.
- `testing.py` is empty.
