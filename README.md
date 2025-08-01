import java.util.ArrayList;
import java.util.Iterator;
import java.util.Scanner;

public class TaskSchedulerApp {

    // --- Task Class ---
    static class Task {
        private String name;
        private String time;

        public Task(String name, String time) {
            this.name = name;
            this.time = time;
        }

        public String getName() {
            return name;
        }

        public String getTime() {
            return time;
        }

        @Override
        public String toString() {
            return "- " + name + " (at " + time + ")";
        }
    }

    // --- TaskManager Class ---
    static class TaskManager {
        private ArrayList<Task> tasks;

        public TaskManager() {
            tasks = new ArrayList<>();
        }

        public void addTask(String name, String time) {
            tasks.add(new Task(name, time));
            System.out.println("Task '" + name + "' added successfully.");
        }

        public void viewTasks() {
            if (tasks.isEmpty()) {
                System.out.println("No tasks scheduled yet.");
            } else {
                System.out.println("\n--- Your Daily Tasks ---");
                for (int i = 0; i < tasks.size(); i++) {
                    System.out.println((i + 1) + ". " + tasks.get(i).toString());
                }
                System.out.println("------------------------\n");
            }
        }

        public void deleteTask(String identifier) {
            boolean found = false;
            // Try to delete by index first
            try {
                int index = Integer.parseInt(identifier) - 1; // User input is 1-based
                if (index >= 0 && index < tasks.size()) {
                    String taskName = tasks.get(index).getName();
                    tasks.remove(index);
                    System.out.println("Task '" + taskName + "' deleted successfully.");
                    found = true;
                }
            } catch (NumberFormatException e) {
                // Not a number, so try to delete by name
                Iterator<Task> iterator = tasks.iterator();
                while (iterator.hasNext()) {
                    Task task = iterator.next();
                    if (task.getName().equalsIgnoreCase(identifier)) {
                        iterator.remove();
                        System.out.println("Task '" + task.getName() + "' deleted successfully.");
                        found = true;
                        break; // Assuming you delete the first match
                    }
                }
            }

            if (!found) {
                System.out.println("Task '" + identifier + "' not found or invalid index.");
            }
        }
    }

    // --- Main Application Logic ---
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        TaskManager taskManager = new TaskManager();
        boolean running = true;

        System.out.println("Welcome to the Console Task Scheduler!");

        while (running) {
            System.out.println("\n--- Menu ---");
            System.out.println("1. Add Task");
            System.out.println("2. View Tasks");
            System.out.println("3. Delete Task");
            System.out.println("4. Exit");
            System.out.print("Enter your choice: ");

            String choiceStr = scanner.nextLine(); // Read choice as string to handle non-numeric input
            int choice = -1; // Default invalid choice

            try {
                choice = Integer.parseInt(choiceStr);
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Please enter a number between 1 and 4.");
                continue; // Skip to the next iteration of the loop
            }

            switch (choice) {
                case 1:
                    System.out.print("Enter task name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter task time (e.g., 10:00 AM, Evening): ");
                    String time = scanner.nextLine();
                    taskManager.addTask(name, time);
                    break;
                case 2:
                    taskManager.viewTasks();
                    break;
                case 3:
                    System.out.print("Enter task name or number to delete: ");
                    String identifierToDelete = scanner.nextLine();
                    taskManager.deleteTask(identifierToDelete);
                    break;
                case 4:
                    running = false;
                    System.out.println("Exiting Task Scheduler. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please enter a number between 1 and 4.");
            }
        }
        scanner.close(); // Close the scanner when done
    }
}# Tasksheduleapp
