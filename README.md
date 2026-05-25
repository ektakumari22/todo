//
// Source code recreated from a .class file by IntelliJ IDEA
//

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Color;
import java.awt.Dialog;
import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.Frame;
import java.awt.Label;
import java.awt.List;
import java.awt.Panel;
import java.awt.TextField;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class TodoApp extends Frame implements ActionListener {
    private TextField taskField;
    private Button addButton;
    private Button deleteButton;
    private Button doneButton;
    private Button saveButton;
    private List taskList;
    private ArrayList<String> tasks = new ArrayList();
    private final String FILE_NAME = "tasks.txt";

    public TodoApp() {
        this.setTitle("Professional To-Do Manager");
        this.setSize(600, 500);
        this.setLayout(new BorderLayout());
        this.setBackground(new Color(240, 240, 240));
        Panel topPanel = new Panel();
        topPanel.setLayout(new FlowLayout());
        Label heading = new Label("Enter Task:");
        heading.setFont(new Font("Arial", 1, 16));
        this.taskField = new TextField(30);
        this.addButton = new Button("Add Task");
        this.deleteButton = new Button("Delete Task");
        this.doneButton = new Button("Mark Done");
        this.saveButton = new Button("Save");
        topPanel.add(heading);
        topPanel.add(this.taskField);
        topPanel.add(this.addButton);
        topPanel.add(this.deleteButton);
        topPanel.add(this.doneButton);
        topPanel.add(this.saveButton);
        this.taskList = new List();
        this.taskList.setFont(new Font("Arial", 0, 16));
        this.add(topPanel, "North");
        this.add(this.taskList, "Center");
        this.addButton.addActionListener(this);
        this.deleteButton.addActionListener(this);
        this.doneButton.addActionListener(this);
        this.saveButton.addActionListener(this);
        this.addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent we) {
                TodoApp.this.saveTasksToFile();
                TodoApp.this.dispose();
            }
        });
        this.loadTasksFromFile();
        this.setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == this.addButton) {
            String task = this.taskField.getText().trim();
            if (!task.isEmpty()) {
                this.tasks.add(task);
                this.taskList.add(task);
                this.taskField.setText("");
            }
        } else if (e.getSource() == this.deleteButton) {
            int selectedIndex = this.taskList.getSelectedIndex();
            if (selectedIndex != -1) {
                this.tasks.remove(selectedIndex);
                this.taskList.remove(selectedIndex);
            }
        } else if (e.getSource() == this.doneButton) {
            int selectedIndex = this.taskList.getSelectedIndex();
            if (selectedIndex != -1) {
                String oldTask = this.taskList.getItem(selectedIndex);
                if (!oldTask.startsWith("✔ ")) {
                    String updatedTask = "✔ " + oldTask;
                    this.tasks.set(selectedIndex, updatedTask);
                    this.taskList.replaceItem(updatedTask, selectedIndex);
                }
            }
        } else if (e.getSource() == this.saveButton) {
            this.saveTasksToFile();
            Dialog dialog = new Dialog(this, "Saved", true);
            dialog.setLayout(new FlowLayout());
            dialog.add(new Label("Tasks Saved Successfully!"));
            Button okButton = new Button("OK");
            okButton.addActionListener((ae) -> dialog.dispose());
            dialog.add(okButton);
            dialog.setSize(250, 100);
            dialog.setVisible(true);
        }

    }

    private void saveTasksToFile() {
        try {
            BufferedWriter writer = new BufferedWriter(new FileWriter("tasks.txt"));

            for(String task : this.tasks) {
                writer.write(task);
                writer.newLine();
            }

            writer.close();
        } catch (IOException var4) {
            System.out.println("Error saving tasks.");
        }

    }

    private void loadTasksFromFile() {
        try {
            BufferedReader reader = new BufferedReader(new FileReader("tasks.txt"));

            String line;
            while((line = reader.readLine()) != null) {
                this.tasks.add(line);
                this.taskList.add(line);
            }

            reader.close();
        } catch (IOException var3) {
            System.out.println("No previous tasks found.");
        }

    }

    public static void main(String[] args) {
        new TodoApp();
    }
}
