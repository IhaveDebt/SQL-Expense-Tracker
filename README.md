# expenses.py
import psycopg2

DB_PARAMS = {
    "dbname": "expenses_db",
    "user": "postgres",
    "password": "password",
    "host": "localhost",
    "port": 5432
}

def add_expense(category, amount, note):
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("INSERT INTO expenses(category, amount, note) VALUES (%s, %s, %s);", (category, amount, note))
    conn.commit()
    cur.close()
    conn.close()
    print(f"Added expense {category}: {amount}")

def total_spent():
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("SELECT SUM(amount) FROM expenses;")
    total = cur.fetchone()[0]
    cur.close()
    conn.close()
    return total or 0

if __name__ == "__main__":
    add_expense("Food", 12.50, "Lunch")
    print("Total spent:", total_spent())
