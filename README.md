# car-booking-system
/* this is a simple car booking system based on java oops concepts. where a user can book a car, return a booked car etc. */

import java.util.*;
class Car{
    private String Model;
    private String carId;
    private String brand;
    private double basePricePerDay;
    private boolean isAvailable;
    public Car(String Model, String carId, String brand, double basePricePerDay){
        this.carId=carId;
        this.Model=Model;
        this.brand=brand;
        this.basePricePerDay=basePricePerDay;
        this.isAvailable=true;
    }
    public String getCarId(){
        return  carId;
    }
    public String getModel(){
        return Model;
    }
    public String getbrand(){
        return brand;
    }


    public double getBasePricePerDay() {
        return basePricePerDay;
    }

    public double calculatePrice(int rentalDays){
        return basePricePerDay = rentalDays;
    }
    public boolean isAvailable(){
        return isAvailable;
    }
    public void rent(){
        isAvailable=true;
    }
}
class Customer{
    private String customerId;
    private String name;
    public Customer(String customerId, String name){
        this.customerId=customerId;
        this.name=name;
    }

    public String getCustomerId() {
        return customerId;
    }

    public String getName() {
        return name;
    }
}

class Rental{
    private Car car;
    private Customer customer;
    private int days;
    public Rental(Car car, Customer customer, int days){
        this.car=car;
        this.customer=customer;
        this.days=days;
    }

    public Car getCar() {
        return car;
    }

    public Customer getCustomer() {
        return customer;
    }

    public int getDays() {
        return days;
    }
}
class CarRentalSystem{
    private List<Car> cars;
    private List<Customer> customers;
    private List<Rental> rentals;
    public CarRentalSystem(){
        cars=new ArrayList<>();
        customers= new ArrayList<>();
        rentals=new ArrayList<>();
    }
    public void addCar(Car car){
        cars.add(car);
    }
    public void addCustomer(Customer customer){
        customers.add(customer);
    }
    public void rentCar(Car car, Customer customer, int days){
        if(car.isAvailable()){
            car.rent();
            rentals.add(new Rental(car, customer, days));
        } else {
            System.out.println("car is not available for rent");
        }
    }
    public void returnCar(Car car) {

        Rental rentaltoRemove = null;
        for (Rental rental : rentals) {
            if (rental.getCar() == car) {
                rentaltoRemove = rental;
                break;
            }
        }
        if (rentaltoRemove != null) {
            rentals.remove(rentaltoRemove);
            System.out.println("car returned successfully.");
        } else {
            System.out.println("car want not rented");
        }
    }
    public void menu(){
        Scanner sc= new Scanner(System.in);
        while(true) {
            System.out.println("====== CAR RENTAL SYSTEM ======");
            System.out.println("1. Rent a Car");
            System.out.println("2. Return a Car");
            System.out.println("3. exit");
            System.out.println("Enter your choice: ");
            int choice = sc.nextInt();

            if (choice == 1) {
                System.out.println("RENT A CAR");
                System.out.println("enter your name: ");
                String customerName = sc.next();
                System.out.println("Available cars:");
                for (Car car : cars) {
                    if (car.isAvailable()) {
                        System.out.println(car.getCarId() + "-" + car.getbrand() + " " + car.getModel());
                    }
                }
                System.out.println("enter car id you want to rent: ");
                String carId = sc.nextLine();
                System.out.println("enter the number of days you want to rent: ");
                int rentalDays = sc.nextInt();
                sc.nextLine();
                Customer newCustomer = new Customer("cus" + (customers.size() + 1), customerName);
                addCustomer(newCustomer);

                Car selected = null;
                for (Car car : cars) {
                    if (car.getCarId().equals(carId) && car.isAvailable()) {
                        selected = car;
                        break;
                    }
                }
                if (selected != null) {
                    double totalPrice;
                    System.out.println("retnal information: ");
                    System.out.println("customer id: " + newCustomer.getCustomerId());

                    System.out.println("customer name: " + newCustomer.getName());
                    System.out.println("car:totalPrice=rentalDays*; " + selected.getbrand() + " " + selected.getModel());
                    System.out.println("retnal days: " + rentalDays);
                    totalPrice=rentalDays* selected.getBasePricePerDay();
                    System.out.println("total price: " + totalPrice);
                    System.out.println("conferm rent (Y/N): ");
                    String conferm = sc.next();
                    if (conferm.equalsIgnoreCase("y")) {
                        rentCar(selected, newCustomer, rentalDays);
                        System.out.println("car rented successfully");
                    } else {
                        System.out.println("invalid car selection or car not available for rent:");
                    }
                }
            } else if (choice == 2) {
                System.out.println("return a car ");
                System.out.println("enter the car id you want to return : ");
                String carId = sc.nextLine();
                Car carToReturn = null;
                for (Car car : cars) {
                    if (car.getCarId().equals(carId) && car.isAvailable()) {
                        carToReturn = car;
                        break;
                    }
                }
                if (carToReturn != null) {
                    Customer customer = null;
                    for (Rental rental : rentals) {
                        if (rental.getCar() == carToReturn) {
                            customer = rental.getCustomer();
                            break;
                        }
                    }
                    if (customer != null) {
                        returnCar(carToReturn);
                        System.out.println("car returned successfully by " + customer.getName());
                    } else {
                        System.out.println("car was not rented or rental information is missing: ");
                    }
                } else {
                    System.out.println("invaliid car id or car is not retned ");
                }
            } else if (choice == 3) {
                break;
            } else {
                System.out.println("invalid choice enter a valid option");
            }
        }
            System.out.println("thank you for visiting ");
    }
}
public class Main {
    public static void main(String[] args) {
        CarRentalSystem rentalsystem=new CarRentalSystem();
        Car car1= new Car("c001","toyota", "carry",60);
        Car car2= new Car("c002","toyota", "carry",450);
        Car car3 = new Car("c003", "toyota", "carry", 40);
        rentalsystem.addCar(car1);
        rentalsystem.addCar(car2);
        rentalsystem.addCar(car3);
        rentalsystem.menu();
    }
}
