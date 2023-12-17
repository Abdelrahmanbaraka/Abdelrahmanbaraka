import java.util.ArrayList;
import java.util.Scanner;

class Dish {
        private String name;
        private double price;
        private String description;

        public Dish(String name, double price, String description) {
                this.name = name;
                this.price = price;
                this.description = description;
        }

        public String getName() {
                return name;
        }

        public double getPrice() {
                return price;
        }

        public String getDescription() {
                return description;
        }

        @Override
        public String toString() {
                return name + " - " + price + "€\n" + description;
        }
}

class Topping {
        private String name;
        private double price;

        public Topping(String name, double price) {
                this.name = name;
                this.price = price;
        }

        public String getName() {
                return name;
        }

        public double getPrice() {
                return price;
        }
}

class Order {
        private Dish dish;
        private ArrayList<Topping> toppings;

        public Order(Dish dish, ArrayList<Topping> toppings) {
                this.dish = dish;
                this.toppings = toppings;
        }

        public double calculateTotal() {
                double total = dish.getPrice();
                for (Topping topping : toppings) {
                        total += topping.getPrice();
                }
                return total;
        }

        public Dish getDish() {
                return dish;
        }

        public ArrayList<Topping> getToppings() {
                return toppings;
        }

        @Override
        public String toString() {
                StringBuilder orderDetails = new StringBuilder();
                orderDetails.append("Order:\n");
                orderDetails.append(dish).append("\n");

                if (!toppings.isEmpty()) {
                        orderDetails.append("Toppings:\n");
                        for (Topping topping : toppings) {
                                orderDetails.append("- ").append(topping.getName()).append("\n");
                        }
                }

                orderDetails.append("Total: ").append(calculateTotal()).append("€\n");

                return orderDetails.toString();
        }
}

public class RestaurantApp {

        public static void main(String[] args) {
                ArrayList<Dish> dishes = new ArrayList<>();
                // Add dishes to the menu
                dishes.add(new Dish("The Classic Hamburger", 9.50, "180 g homemade Beef Patty | Salat | Gurke | rote Zwiebeln | Home"));
                dishes.add(new Dish("The American Cheeseburger", 10.50, "180 g homemade Beef Patty | Cheddar | Salat | Gurke | rote Zwiebeln | Home"));
                // Add the remaining dishes similarly

                ArrayList<Topping> toppings = new ArrayList<>();
                // Add toppings to the menu
                toppings.add(new Topping("Extra Beef Patty", 5.90));
                toppings.add(new Topping("Cheddar", 1.00));
                // Add the remaining toppings similarly

                ArrayList<Order> orders = new ArrayList<>();

                Scanner scanner = new Scanner(System.in);

                // Accepting customer orders
                while (true) {
                        System.out.println("Enter dish name (type 'done' to finish ordering):");
                        String dishName = scanner.nextLine();

                        if (dishName.equalsIgnoreCase("done")) {
                                break;
                        }

                        Dish selectedDish = findDishByName(dishes, dishName);

                        if (selectedDish != null) {
                                System.out.println("Enter toppings (comma-separated, press enter for no toppings):");
                                String toppingInput = scanner.nextLine();
                                ArrayList<Topping> selectedToppings = findToppingsByNames(toppings, toppingInput);

                                Order order = new Order(selectedDish, selectedToppings);
                                orders.add(order);
                        } else {
                                System.out.println("Invalid dish name. Please try again.");
                        }
                }

                // Printing orders
                System.out.println("\nCustomer Orders:");
                for (Order order : orders) {
                        System.out.println(order);
                }

                // Display chosen dishes along with toppings
                System.out.println("\nChosen Dishes:");
                for (Order order : orders) {
                        System.out.println(order.getDish());
                        if (!order.getToppings().isEmpty()) {
                                System.out.println("Toppings:");
                                for (Topping topping : order.getToppings()) {
                                        System.out.println("- " + topping.getName());
                                }
                        }
                        System.out.println();
                }

                scanner.close();
        }

        private static Dish findDishByName(ArrayList<Dish> dishes, String dishName) {
                for (Dish dish : dishes) {
                        if (dish.getName().equalsIgnoreCase(dishName)) {
                                return dish;
                        }
                }
                return null;
        }

        private static ArrayList<Topping> findToppingsByNames(ArrayList<Topping> toppings, String toppingNames) {
                String[] toppingArray = toppingNames.split(",");
                return toppings;
        }
}
