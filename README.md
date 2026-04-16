# PYTHON-PROJECT
import time
from datetime import datetime

class ParkingSystem:
    def __init__(self, total_slots):
        self.total_slots = total_slots
        self.slots = {i: None for i in range(1, total_slots + 1)}
        self.history = []
    def show_available_slots(self):
        available = [slot for slot, vehicle in self.slots.items() if vehicle is None]
        print(f"\nAvailable Slots: {available}")
        print(f"Total Free Slots: {len(available)}")
    def park_vehicle(self, vehicle_number):
        for slot, vehicle in self.slots.items():
            if vehicle is None:
                entry_time = datetime.now()
                self.slots[slot] = {
                    "vehicle_number": vehicle_number,
                    "entry_time": entry_time
                }
                print(f"\nVehicle {vehicle_number} parked at Slot {slot}")
                return
        print("\nParking Full!")
    def remove_vehicle(self, vehicle_number):
        for slot, vehicle in self.slots.items():
            if vehicle and vehicle["vehicle_number"] == vehicle_number:
                exit_time = datetime.now()
                entry_time = vehicle["entry_time"]
                duration = (exit_time - entry_time).seconds // 60
                 self.history.append({
                    "vehicle_number": vehicle_number,
                    "slot": slot,
                    "entry_time": entry_time,
                    "exit_time": exit_time,
                    "duration": duration
                })
                self.slots[slot] = None
                print(f"\nVehicle {vehicle_number} exited from Slot {slot}")
                print(f"Parking Duration: {duration} minutes")
                return
        print("\nVehicle not found!")
    def show_status(self):
        print("\n--- Parking Status ---")
        for slot, vehicle in self.slots.items():
            if vehicle:
                print(f"Slot {slot}: Occupied ({vehicle['vehicle_number']})")
            else:
                print(f"Slot {slot}: Free")
    def show_analytics(self):
        total_vehicles = len(self.history)
        total_time = sum([record["duration"] for record in self.history])
        avg_time = total_time / total_vehicles if total_vehicles > 0 else 0
        print("\n--- Analytics Report ---")
        print(f"Total Vehicles Served: {total_vehicles}")
        print(f"Total Parking Time: {total_time} minutes")
        print(f"Average Parking Time: {avg_time:.2f} minutes")
        print("\nVehicle History:")
        for record in self.history:
            print(f"{record['vehicle_number']} | Slot {record['slot']} | {record['duration']} mins")


# ---------------- MAIN PROGRAM ----------------

def main():
    print("SMART PARKING ALLOCATION & OCCUPANCY ANALYTICS SYSTEM")
    total_slots = int(input("Enter total parking slots: "))
     system = ParkingSystem(total_slots)
     while True:
        print("\n===== MENU =====")
        print("1. Show Available Slots")
        print("2. Park Vehicle")
        print("3. Remove Vehicle")
        print("4. Show Parking Status")
        print("5. Show Analytics")
        print("6. Exit")
        choice = input("Enter choice: ")
    if choice == "1":
            system.show_available_slots()
    elif choice == "2":
            vehicle_number = input("Enter vehicle number: ")
            system.park_vehicle(vehicle_number)
    elif choice == "3":
            vehicle_number = input("Enter vehicle number to remove: ")
            system.remove_vehicle(vehicle_number)
    elif choice == "4":
            system.show_status()
    elif choice == "5":
            system.show_analytics()
    elif choice == "6":
            print("Exiting system...")
            break
    else:
            print("Invalid choice! Try again.")


if __name__ == "__main__":
    main()
