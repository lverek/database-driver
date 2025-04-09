# database-driver
import sqlite3

class DatabaseDriver:
    def __init__(self, db_name="database.db"):
        self.db_name = db_name
        self.connection = None
        self.cursor = None
        self.connect()

    def connect(self):
        """Establish a connection to the database."""
        self.connection = sqlite3.connect(self.db_name)
        self.cursor = self.connection.cursor()
        print("Connected to database")
    
    def create_table(self, table_name, columns):
        """Create a table with specified columns."""
        columns_definition = ", ".join([f"{col} {dtype}" for col, dtype in columns.items()])
        query = f"CREATE TABLE IF NOT EXISTS {table_name} ({columns_definition})"
        self.cursor.execute(query)
        self.connection.commit()
        print(f"Table '{table_name}' created or already exists.")
    
    def insert_data(self, table_name, data):
        """Insert data into the specified table."""
        placeholders = ", ".join(["?" for _ in data])
        query = f"INSERT INTO {table_name} VALUES ({placeholders})"
        self.cursor.execute(query, tuple(data))
        self.connection.commit()
        print("Data inserted successfully.")
    
    def fetch_data(self, table_name):
        """Fetch all data from the specified table."""
        query = f"SELECT * FROM {table_name}"
        self.cursor.execute(query)
        return self.cursor.fetchall()
    
    def close(self):
        """Close the database connection."""
        if self.connection:
            self.connection.close()
            print("Database connection closed.")

# Example usage
def main():
    db = DatabaseDriver()
    db.create_table("users", {"id": "INTEGER PRIMARY KEY", "name": "TEXT", "age": "INTEGER"})
    db.insert_data("users", (1, "Alice", 25))
    db.insert_data("users", (2, "Bob", 30))
    users = db.fetch_data("users")
    print("Users:", users)
    db.close()

if __name__ == "__main__":
    main()
67889
