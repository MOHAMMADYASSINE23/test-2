# test-2
class Driver:
    def __init__(self, worker_id, name, start_city):
        self.worker_id = worker_id
        self.name = name
        self.start_city = start_city

class City:
    def __init__(self, name, connected_cities):
        self.name = name
        self.connected_cities = connected_cities

class WeDeliverSystem:
    def __init__(self):
        self.drivers = []
        self.cities = []
        self.driver_id_counter = 1

        import json
        drivers = []
        cities = {}

        drivers = [
            {"id": "ID001", "name": "Max Verstappen", "start_city": "Akkar"},
            {"id": "ID002", "name": "Charles Leclerc", "start_city": "Saida"},
            {"id": "ID003", "name": "Lando Norris", "start_city": "Jbeil"}
        ]
        
        cities = {
            "Jbeil": ["Akkar", "Beirut"],
            "Akkar": ["Jbeil"],
            "Beirut": ["Jbeil"],
            "Saida": ["Zahle"],
            "Zahle": ["Saida"]
        }
        
        def main_menu():
            while True:
                print("Hello! Please enter:")
                print("1. To go to the drivers' menu")
                print("2. To go to the cities' menu")
                print("3. To exit the system")
                choice = input("Enter your choice: ")
                if choice == "1":
                    drivers_menu()
                elif choice == "2":
                    cities_menu()
                elif choice == "3":
                    print("Exiting the system.")
                    break
                else:
                    print("Invalid choice. Please try again.")
        
        def drivers_menu():
            while True:
                print("\nDRIVERS' MENU")
                print("1. To view all the drivers")
                print("2. To add a driver")
                print("3. To go back to main menu")
                choice = input("Enter your choice: ")
                if choice == "1":
                    view_all_drivers()
                elif choice == "2":
                    add_driver()
                elif choice == "3":
                    break
                else:
                    print("Invalid choice. Please try again.")
        
        def view_all_drivers():
            print("\nList of all drivers:")
            for driver in drivers:
                print(f"{driver['id']}, {driver['name']}, {driver['start_city']}")
        
        def add_driver():
            name = input("Enter the driver's name: ")
            start_city = input("Enter the start city of the driver: ")
            if start_city not in cities:
                add_city = input(f"{start_city} is not in the database. Do you want to add it? (yes/no): ").lower()
                if add_city == "yes":
                    cities[start_city] = []
                else:
                    print("Driver not added due to invalid city.")
                    return
            new_id = f"ID{len(drivers) + 1:03d}"
            drivers.append({"id": new_id, "name": name, "start_city": start_city})
            print(f"Driver {name} with ID {new_id} added successfully.")
        
        def cities_menu():
            while True:
                print("\nCITIES' MENU")
                print("1. Show cities")
                print("2. Print neighboring cities")
                print("3. Print drivers delivering to city")
                print("4. Go back to main menu")
                choice = input("Enter your choice: ")
                if choice == "1":
                    show_cities()
                elif choice == "2":
                    print_neighboring_cities()
                elif choice == "3":
                    print_drivers_delivering_to_city()
                elif choice == "4":
                    break
                else:
                    print("Invalid choice. Please try again.")
        
        def show_cities():
            print("\nList of all cities:")
            for city in cities.keys():
                print(city)
        
        def print_neighboring_cities():
            city = input("Enter the city name: ")
            if city in cities:
                print(f"Neighboring cities of {city}: {', '.join(cities[city])}")
            else:
                print(f"{city} is not in the database.")
        
        def print_drivers_delivering_to_city():
            city = input("Enter the city name: ")
            if city not in cities:
                print(f"{city} is not in the database.")
                return
            delivering_drivers = [driver for driver in drivers if can_reach(driver["start_city"], city)]
            print(f"Drivers delivering to {city}:")
            for driver in delivering_drivers:
                print(f"{driver['id']}, {driver['name']}, {driver['start_city']}")
        
        def can_reach(start, end, visited=None):
            if visited is None:
                visited = set()
            if start == end:
                return True
            visited.add(start)
            for neighbor in cities[start]:
                if neighbor not in visited and can_reach(neighbor, end, visited):
                    return True
            return False
        
        if __name__ == "__main__":
            main_menu()
