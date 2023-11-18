# aicp-assign-2
class ElectricMountainRailway:
    def __init__(self):
        self.departure_times = ["09:00", "11:00", "13:00", "15:00"]
        self.return_times = ["10:00", "12:00", "14:00", "16:00"]
        self.coaches = 6
        self.seats_per_coach = 80
        self.ticket_price = 25
        self.group_discount_threshold = 10
        self.extra_coaches_last_train = 2

        # Initialize data structures for passenger counts and total money for each journey
        self.passengers_up = [0] * len(self.departure_times)
        self.passengers_down = [0] * len(self.return_times)
        self.money_up = [0] * len(self.departure_times)
        self.money_down = [0] * len(self.return_times)

        # Initialize screen display
        self.display_tickets_available()

    def display_tickets_available(self):
        print("\nTickets Available:")
        for i, time in enumerate(self.departure_times):
            print(f"Train {time} (Up): {self.get_tickets_available('up', i)} available")
        for i, time in enumerate(self.return_times):
            print(f"Train {time} (Down): {self.get_tickets_available('down', i)} available")

    def get_tickets_available(self, direction, index):
        if direction == 'up':
            return self.coaches * self.seats_per_coach - self.passengers_up[index]
        elif direction == 'down':
            return (self.coaches + self.extra_coaches_last_train) * self.seats_per_coach - self.passengers_down[index]

    def purchase_tickets(self, num_passengers, up_index, down_index):
        # Check if tickets are available
        if self.get_tickets_available('up', up_index) >= num_passengers and \
           self.get_tickets_available('down', down_index) >= num_passengers:
            # Calculate total price
            total_price = self.calculate_total_price(num_passengers)

            # Update data structures
            self.passengers_up[up_index] += num_passengers
            self.passengers_down[down_index] += num_passengers
            self.money_up[up_index] += total_price
            self.money_down[down_index] += total_price

            # Display updated screen
            self.display_tickets_available()

            return total_price
        else:
            print("Tickets not available for the selected train journeys.")
            return 0

    def calculate_total_price(self, num_passengers):
        total_price = num_passengers * self.ticket_price

        # Check for group discount
        if num_passengers >= self.group_discount_threshold:
            num_free_tickets = num_passengers // self.group_discount_threshold
            total_price -= num_free_tickets * self.ticket_price

        return total_price

    def end_of_day_stats(self):
        # Display number of passengers and total money for each train journey
        print("\nEnd-of-Day Statistics:")
        for i, time in enumerate(self.departure_times):
            print(f"Train {time} (Up): {self.passengers_up[i]} passengers, ${self.money_up[i]:.2f}")
        for i, time in enumerate(self.return_times):
            print(f"Train {time} (Down): {self.passengers_down[i]} passengers, ${self.money_down[i]:.2f}")

        # Calculate and display total number of passengers and total money taken for the day
        total_passengers = sum(self.passengers_up) + sum(self.passengers_down)
        total_money = sum(self.money_up) + sum(self.money_down)
        print(f"\nTotal Passengers for the Day: {total_passengers}")
        print(f"Total Money Taken for the Day: ${total_money:.2f}")

        # Find and display the train journey with the most passengers
        max_passengers_index = self.passengers_up.index(max(self.passengers_up))
        max_passengers_time = self.departure_times[max_passengers_index]
        print(f"\nTrain Journey with the Most Passengers: {max_passengers_time} (Up) - {max(self.passengers_up)} passengers")


# Test the program
railway = ElectricMountainRailway()

# Task 2 - Purchasing tickets
total_price = railway.purchase_tickets(15, 0, 0)  # Example purchase for 15 passengers
print(f"\nTotal Price: ${total_price:.2f}")

# Task 3 - End of the day statistics
railway.end_of_day_stats()
