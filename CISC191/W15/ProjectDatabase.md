# Project Database

### Armaan Shamsaasef
### Date: 12/14/2025
### CISC191 Week 15 Assignments


# Here is the flowchart!

<img width="2601" height="1925" alt="image" src="https://github.com/user-attachments/assets/91680ed6-b9b7-41b3-bc9e-59b7cf651ba7" />



## Challenges

### Technical/Setup Challenges:
#### MySQL Installation & Path Configuration

#### JDBC Driver Issues

#### Database Connection Problems


### Data Processing Challenges:


#### Data Parsing & Insertion
#### Data Format Complexities
###  SQL & Query Challenges:
#### SQL Syntax Errors
#### Query Interface Design


### Java Programming Challenges:

#### GUI Development Issues

#### Exception Handling

#### Data Display Challenges

### Integration Challenges:

#### Multi-component Integration

#### Development Environment Issues




## This is my code!

```.java

import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.FileReader;
import java.sql.*;
import javax.swing.event.ChangeEvent;
import javax.swing.event.ChangeListener;

public class AutoMPGDatabase {
    // Database connection parameters
    private static final String DB_URL = "jdbc:mysql://localhost:3306/Auto";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "Armaan2017#";

    // Data string for initial testing (small sample)
    private static final String DATA = """
            18.0   8.   307.0      130.0      3504.      12.0   70.  1.	"chevrolet chevelle malibu"
            15.0   8.   350.0      165.0      3693.      11.5   70.  1.	"buick skylark 320"
            18.0   8.   318.0      150.0      3436.      11.0   70.  1.	"plymouth satellite"
            16.0   8.   304.0      150.0      3433.      12.0   70.  1.	"amc rebel sst"
            17.0   8.   302.0      140.0      3449.      10.5   70.  1.	"ford torino"
            15.0   8.   429.0      198.0      4341.      10.0   70.  1.	"ford galaxie 500"
            14.0   8.   454.0      220.0      4354.       9.0   70.  1.	"chevrolet impala"
            14.0   8.   440.0      215.0      4312.       8.5   70.  1.	"plymouth fury iii"
            14.0   8.   455.0      225.0      4425.      10.0   70.  1.	"pontiac catalina"
            15.0   8.   390.0      190.0      3850.       8.5   70.  1.	"amc ambassador dpl"
            """;//These are just string data for an example!

    public static void main(String[] args) {
        try {
            // Load MySQL JDBC Driver
            Class.forName("com.mysql.cj.jdbc.Driver");
            System.out.println("MySQL JDBC Driver Loaded Successfully!");
        } catch (ClassNotFoundException e) {
            System.err.println("ERROR: MySQL JDBC Driver not found!");
            System.err.println("Add mysql-connector-java.jar to your project!");
            JOptionPane.showMessageDialog(null,
                    "MySQL JDBC Driver not found!\n" +
                            "Please add mysql-connector-java.jar to your project.",
                    "Driver Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        // Initialize database - using string data first
        if (!initializeDatabase()) {
            JOptionPane.showMessageDialog(null,
                    "Failed to initialize database.\n" +
                            "Check your MySQL connection and credentials.",
                    "Database Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        // Create and show GUI
        SwingUtilities.invokeLater(() -> createAndShowGUI());
    }

    private static boolean initializeDatabase() {
        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
            System.out.println("Connected to database successfully!");

            // Create table if it doesn't exist
            String createTableSQL = "CREATE TABLE IF NOT EXISTS auto_mpg (" +
                    "id INT AUTO_INCREMENT PRIMARY KEY, " +
                    "mpg FLOAT, " +
                    "cylinders INT, " +
                    "displacement FLOAT, " +
                    "horsepower FLOAT, " +
                    "weight FLOAT, " +
                    "acceleration FLOAT, " +
                    "model_year INT, " +
                    "origin INT, " +
                    "car_name VARCHAR(255)" +
                    ")";

            Statement stmt = conn.createStatement();
            stmt.executeUpdate(createTableSQL);
            System.out.println("Table 'auto_mpg' created or already exists.");

            // Clear existing data
            stmt.executeUpdate("DELETE FROM auto_mpg");
            System.out.println("Cleared existing data.");

            // Insert data from string for initial testing
            insertDataFromString(conn);

            System.out.println("Data inserted successfully!");
            return true;

        } catch (SQLException e) {
            System.err.println("Database connection failed!");
            System.err.println("Error: " + e.getMessage());
            e.printStackTrace();
            return false;
        }
    }

    private static void insertDataFromString(Connection conn) throws SQLException {
        BufferedReader reader = new BufferedReader(new java.io.StringReader(DATA));
        String line;
        int count = 0;

        String sql = "INSERT INTO auto_mpg (mpg, cylinders, displacement, horsepower, weight, acceleration, model_year, origin, car_name) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)";
        PreparedStatement pstmt = conn.prepareStatement(sql);

        try {
            while ((line = reader.readLine()) != null) {
                line = line.trim();
                if (line.isEmpty()) continue;

                try {
                    // Split by whitespace
                    String[] parts = line.split("\\s+");

                    if (parts.length < 8) {
                        System.err.println("Skipping invalid line: " + line);
                        continue;
                    }

                    // Parse each field
                    String mpgStr = parts[0];
                    String cylindersStr = parts[1];
                    String displacementStr = parts[2];
                    String horsepowerStr = parts[3];
                    String weightStr = parts[4];
                    String accelerationStr = parts[5];
                    String modelYearStr = parts[6];
                    String originStr = parts[7];

                    // Car name might be split into multiple parts
                    StringBuilder carNameBuilder = new StringBuilder();
                    for (int i = 8; i < parts.length; i++) {
                        carNameBuilder.append(parts[i]).append(" ");
                    }
                    String carName = carNameBuilder.toString().trim().replace("\"", "");

                    // Set parameters
                    setParameter(pstmt, 1, mpgStr);      // mpg
                    pstmt.setInt(2, parseInteger(cylindersStr));  // cylinders
                    setParameter(pstmt, 3, displacementStr);  // displacement
                    setParameter(pstmt, 4, horsepowerStr);   // horsepower
                    setParameter(pstmt, 5, weightStr);       // weight
                    setParameter(pstmt, 6, accelerationStr); // acceleration
                    pstmt.setInt(7, parseInteger(modelYearStr)); // model_year
                    pstmt.setInt(8, parseInteger(originStr));    // origin
                    pstmt.setString(9, carName);            // car_name

                    pstmt.executeUpdate();
                    count++;

                } catch (Exception e) {
                    System.err.println("Error parsing line: " + line);
                    System.err.println("Error: " + e.getMessage());
                }
            }

            System.out.println("Successfully inserted " + count + " records from string!");

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            pstmt.close();
        }
    }

    private static void setParameter(PreparedStatement pstmt, int index, String value) throws SQLException {
        if (value.equals("NA") || value.equals("NA.")) {
            pstmt.setNull(index, Types.FLOAT);
        } else {
            try {
                pstmt.setFloat(index, Float.parseFloat(value));
            } catch (NumberFormatException e) {
                pstmt.setNull(index, Types.FLOAT);
            }
        }
    }

    private static int parseInteger(String value) {
        // Remove any trailing dots
        value = value.replace(".", "");
        try {
            return Integer.parseInt(value);
        } catch (NumberFormatException e) {
            return 0;
        }
    }

    private static void createAndShowGUI() {
        JFrame frame = new JFrame("Auto MPG Database Query");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(1400, 800);

        // Main panel with BorderLayout
        JPanel mainPanel = new JPanel(new BorderLayout());

        // TOP PANEL: Input Controls
        JPanel topPanel = new JPanel(new BorderLayout());

        // Query input panel (north part of top panel)
        JPanel queryPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);

        // Instructions
        JLabel instructionLabel = new JLabel("<html><b>Enter query:</b> Type 'ALL' for all records, a WHERE clause, or car name</html>");
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 3;
        queryPanel.add(instructionLabel, gbc);

        // Query input
        JTextField queryField = new JTextField(40);
        gbc.gridy = 1;
        gbc.gridwidth = 2;
        gbc.fill = GridBagConstraints.HORIZONTAL;
        queryPanel.add(queryField, gbc);

        // Buttons
        JButton executeButton = new JButton("Execute Query");
        JButton clearButton = new JButton("Clear");
        JButton helpButton = new JButton("Help");
        JButton loadFileButton = new JButton("Load from File");

        gbc.gridx = 2;
        gbc.gridwidth = 1;
        gbc.fill = GridBagConstraints.NONE;
        queryPanel.add(executeButton, gbc);

        gbc.gridx = 3;
        queryPanel.add(clearButton, gbc);

        gbc.gridx = 4;
        queryPanel.add(helpButton, gbc);

        gbc.gridx = 5;
        queryPanel.add(loadFileButton, gbc);

        // SLIDERS PANEL (Center of top panel)
        JPanel slidersPanel = new JPanel(new GridLayout(2, 2, 20, 10));
        slidersPanel.setBorder(BorderFactory.createTitledBorder("Filter by Range (adjust sliders to filter)"));

        // Min MPG Slider
        JPanel minMpgPanel = new JPanel(new BorderLayout());
        JLabel minMpgLabel = new JLabel("Minimum MPG: 0");
        JSlider minMpgSlider = new JSlider(0, 50, 0);
        minMpgSlider.setPaintTicks(true);
        minMpgSlider.setPaintLabels(true);
        minMpgSlider.setMajorTickSpacing(10);
        minMpgSlider.setMinorTickSpacing(5);
        minMpgPanel.add(minMpgLabel, BorderLayout.NORTH);
        minMpgPanel.add(minMpgSlider, BorderLayout.CENTER);

        // Max MPG Slider
        JPanel maxMpgPanel = new JPanel(new BorderLayout());
        JLabel maxMpgLabel = new JLabel("Maximum MPG: 50");
        JSlider maxMpgSlider = new JSlider(0, 50, 50);
        maxMpgSlider.setPaintTicks(true);
        maxMpgSlider.setPaintLabels(true);
        maxMpgSlider.setMajorTickSpacing(10);
        maxMpgSlider.setMinorTickSpacing(5);
        maxMpgPanel.add(maxMpgLabel, BorderLayout.NORTH);
        maxMpgPanel.add(maxMpgSlider, BorderLayout.CENTER);

        // Min Horsepower Slider
        JPanel minHpPanel = new JPanel(new BorderLayout());
        JLabel minHpLabel = new JLabel("Minimum Horsepower: 0");
        JSlider minHpSlider = new JSlider(0, 250, 0);
        minHpSlider.setPaintTicks(true);
        minHpSlider.setPaintLabels(true);
        minHpSlider.setMajorTickSpacing(50);
        minHpSlider.setMinorTickSpacing(10);
        minHpPanel.add(minHpLabel, BorderLayout.NORTH);
        minHpPanel.add(minHpSlider, BorderLayout.CENTER);

        // Max Horsepower Slider
        JPanel maxHpPanel = new JPanel(new BorderLayout());
        JLabel maxHpLabel = new JLabel("Maximum Horsepower: 250");
        JSlider maxHpSlider = new JSlider(0, 250, 250);
        maxHpSlider.setPaintTicks(true);
        maxHpSlider.setPaintLabels(true);
        maxHpSlider.setMajorTickSpacing(50);
        maxHpSlider.setMinorTickSpacing(10);
        maxHpPanel.add(maxHpLabel, BorderLayout.NORTH);
        maxHpPanel.add(maxHpSlider, BorderLayout.CENTER);

        slidersPanel.add(minMpgPanel);
        slidersPanel.add(maxMpgPanel);
        slidersPanel.add(minHpPanel);
        slidersPanel.add(maxHpPanel);

        // APPLY SLIDERS BUTTON
        JPanel applyPanel = new JPanel(new FlowLayout());
        JButton applySlidersButton = new JButton("Apply Filters");
        JCheckBox useSlidersCheckBox = new JCheckBox("Use slider filters", true);
        applyPanel.add(useSlidersCheckBox);
        applyPanel.add(applySlidersButton);

        // Top panel adding
        topPanel.add(queryPanel, BorderLayout.NORTH);
        topPanel.add(slidersPanel, BorderLayout.CENTER);
        topPanel.add(applyPanel, BorderLayout.SOUTH);

        // CENTER PANEL: Results Display
        JTextArea resultsArea = new JTextArea();
        resultsArea.setEditable(false);
        resultsArea.setFont(new Font("Monospaced", Font.PLAIN, 12));
        JScrollPane scrollPane = new JScrollPane(resultsArea);

        // Table for displaying results
        JTable resultsTable = new JTable();
        JScrollPane tableScrollPane = new JScrollPane(resultsTable);

        // Tabbed pane
        JTabbedPane tabbedPane = new JTabbedPane();
        tabbedPane.addTab("Text Results", scrollPane);
        tabbedPane.addTab("Table View", tableScrollPane);

        // ==================== BOTTOM PANEL: Sample Queries ====================
        JPanel samplePanel = new JPanel(new FlowLayout(FlowLayout.LEFT));
        samplePanel.setBorder(BorderFactory.createTitledBorder("Sample Queries (click to try):"));

        // Sample queries
        String[] sampleQueries = {
                "ALL",
                "mpg > 20",
                "cylinders = 4",
                "origin = 1",
                "model_year >= 75",
                "horsepower > 100",
                "car_name LIKE '%ford%'",
                "weight < 3000",
                "toyota",
                "chevrolet"
        };

        for (String sample : sampleQueries) {
            JButton sampleBtn = new JButton(sample);
            sampleBtn.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    queryField.setText(sample);
                    executeQueryWithSliders(sample, minMpgSlider, maxMpgSlider, minHpSlider, maxHpSlider,
                            useSlidersCheckBox.isSelected(), resultsArea, resultsTable);
                }
            });
            samplePanel.add(sampleBtn);
        }

        // ASSEMBLE MAIN PANEL
        mainPanel.add(topPanel, BorderLayout.NORTH);
        mainPanel.add(tabbedPane, BorderLayout.CENTER);
        mainPanel.add(samplePanel, BorderLayout.SOUTH);

        frame.add(mainPanel);

        // ACTION LISTENERS

        // Execute Query Button
        executeButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String query = queryField.getText().trim();
                executeQueryWithSliders(query, minMpgSlider, maxMpgSlider, minHpSlider, maxHpSlider,
                        useSlidersCheckBox.isSelected(), resultsArea, resultsTable);
            }
        });

        // Apply Sliders Button
        applySlidersButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String currentQuery = queryField.getText().trim();
                if (currentQuery.isEmpty() || currentQuery.equalsIgnoreCase("ALL")) {
                    executeQueryWithSliders("", minMpgSlider, maxMpgSlider, minHpSlider, maxHpSlider,
                            true, resultsArea, resultsTable);
                } else {
                    executeQueryWithSliders(currentQuery, minMpgSlider, maxMpgSlider, minHpSlider, maxHpSlider,
                            useSlidersCheckBox.isSelected(), resultsArea, resultsTable);
                }
            }
        });

        // Slider Change Listeners
        ChangeListener sliderChangeListener = new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                if (!((JSlider) e.getSource()).getValueIsAdjusting()) {
                    if (useSlidersCheckBox.isSelected()) {
                        applySlidersButton.doClick();
                    }
                }
            }
        };

        minMpgSlider.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                if (!minMpgSlider.getValueIsAdjusting()) {
                    int value = minMpgSlider.getValue();
                    minMpgLabel.setText("Minimum MPG: " + value);
                    if (useSlidersCheckBox.isSelected()) {
                        applySlidersButton.doClick();
                    }
                }
            }
        });

        maxMpgSlider.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                if (!maxMpgSlider.getValueIsAdjusting()) {
                    int value = maxMpgSlider.getValue();
                    maxMpgLabel.setText("Maximum MPG: " + value);
                    if (useSlidersCheckBox.isSelected()) {
                        applySlidersButton.doClick();
                    }
                }
            }
        });

        minHpSlider.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                if (!minHpSlider.getValueIsAdjusting()) {
                    int value = minHpSlider.getValue();
                    minHpLabel.setText("Minimum Horsepower: " + value);
                    if (useSlidersCheckBox.isSelected()) {
                        applySlidersButton.doClick();
                    }
                }
            }
        });

        maxHpSlider.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                if (!maxHpSlider.getValueIsAdjusting()) {
                    int value = maxHpSlider.getValue();
                    maxHpLabel.setText("Maximum Horsepower: " + value);
                    if (useSlidersCheckBox.isSelected()) {
                        applySlidersButton.doClick();
                    }
                }
            }
        });

        // Clear Button
        clearButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                queryField.setText("");
                resultsArea.setText("");
                resultsTable.setModel(new DefaultTableModel());

                // Reset sliders
                minMpgSlider.setValue(0);
                maxMpgSlider.setValue(50);
                minHpSlider.setValue(0);
                maxHpSlider.setValue(250);
            }
        });

        // Help Button
        helpButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String helpText = "=== HOW TO USE ===\n\n" +
                        "1. Type 'ALL' to see all records\n" +
                        "2. Type SQL WHERE clauses without 'WHERE':\n" +
                        "   - mpg > 30\n" +
                        "   - cylinders = 4\n" +
                        "   - origin = 1 AND model_year >= 80\n" +
                        "   - car_name LIKE '%ford%'\n" +
                        "3. Type car names to search automatically:\n" +
                        "   - toyota (searches for toyota in car names)\n" +
                        "   - ford mustang\n" +
                        "   - chevrolet\n" +
                        "4. Use sliders to filter by MPG and Horsepower:\n" +
                        "   - Adjust min/max MPG sliders (0-50)\n" +
                        "   - Adjust min/max Horsepower sliders (0-250)\n" +
                        "   - Check 'Use slider filters' to auto-apply\n" +
                        "   - Click 'Apply Filters' to manually apply\n\n" +
                        "5. Click 'Load from File' to load data from a text file\n\n" +
                        "Click sample buttons for examples.";

                resultsArea.setText(helpText);
                tabbedPane.setSelectedIndex(0);
            }
        });

        // Load File Button
        loadFileButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                loadDataFromFile(resultsArea);
            }
        });

        // Enter key support in query field
        queryField.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String query = queryField.getText().trim();
                executeQueryWithSliders(query, minMpgSlider, maxMpgSlider, minHpSlider, maxHpSlider,
                        useSlidersCheckBox.isSelected(), resultsArea, resultsTable);
            }
        });

        // Show initial help
        resultsArea.setText("Welcome! Type a query, use sliders, or click 'Help' for instructions.\n" +
                "Initial data loaded from string. Click 'Load from File' to load more data.");

        frame.setVisible(true);
    }

    private static void executeQueryWithSliders(String query, JSlider minMpgSlider, JSlider maxMpgSlider,
                                                JSlider minHpSlider, JSlider maxHpSlider, boolean useSliders,
                                                JTextArea resultsArea, JTable resultsTable) {
        try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
            Statement stmt = conn.createStatement();
            String sql;

            // Clean the query
            query = query.trim();

            // Get slider values
            int minMpg = minMpgSlider.getValue();
            int maxMpg = maxMpgSlider.getValue();
            int minHp = minHpSlider.getValue();
            int maxHp = maxHpSlider.getValue();

            // Build the base SQL
            StringBuilder whereClause = new StringBuilder();

            // Handle special cases
            if (query.equalsIgnoreCase("ALL") || query.isEmpty()) {
                whereClause.append("1=1"); // Always true condition
            }
            // If it looks like a WHERE clause
            else if (query.contains("=") || query.contains(">") || query.contains("<") ||
                    query.contains("LIKE") || query.contains("BETWEEN") || query.contains("IN") ||
                    query.contains("AND") || query.contains("OR") || query.contains("NOT")) {
                whereClause.append("(").append(query).append(")");
            }
            // text=assume it's a car name search
            else {
                whereClause.append("car_name LIKE '%").append(query).append("%'");
            }

            // Add slider filters if enabled
            if (useSliders) {
                // Add MPG filter (handle NULL values)
                whereClause.append(" AND (mpg IS NULL OR (mpg >= ").append(minMpg).append(" AND mpg <= ").append(maxMpg).append("))");

                // Add Horsepower filter (handle NULL values)
                whereClause.append(" AND (horsepower IS NULL OR (horsepower >= ").append(minHp).append(" AND horsepower <= ").append(maxHp).append("))");
            }

            // Build final SQL
            sql = "SELECT * FROM auto_mpg WHERE " + whereClause.toString() + " ORDER BY id";

            System.out.println("Executing SQL: " + sql);

            try {
                ResultSet rs = stmt.executeQuery(sql);
                ResultSetMetaData rsmd = rs.getMetaData();
                int columnCount = rsmd.getColumnCount();

                // Display in text area
                StringBuilder sb = new StringBuilder();
                sb.append("Query: ").append(query.isEmpty() ? "ALL" : query).append("\n");

                if (useSliders) {
                    sb.append("Filters: MPG ").append(minMpg).append("-").append(maxMpg);
                    sb.append(", Horsepower ").append(minHp).append("-").append(maxHp).append("\n");
                }

                sb.append("SQL: ").append(sql).append("\n\n");

                // Building header
                for (int i = 1; i <= columnCount; i++) {
                    sb.append(String.format("%-15s", rsmd.getColumnName(i)));
                }
                sb.append("\n");

                for (int i = 1; i <= columnCount; i++) {
                    sb.append("---------------");
                }
                sb.append("\n");

                // Building table model for JTable
                DefaultTableModel tableModel = new DefaultTableModel();

                // Adding column names to table model
                for (int i = 1; i <= columnCount; i++) {
                    tableModel.addColumn(rsmd.getColumnName(i));
                }

                int rowCount = 0;
                while (rs.next()) {
                    rowCount++;
                    Object[] row = new Object[columnCount];

                    // Build text row
                    for (int i = 1; i <= columnCount; i++) {
                        Object value = rs.getObject(i);
                        sb.append(String.format("%-15s", value == null ? "NULL" : value.toString()));
                        row[i-1] = value;
                    }
                    sb.append("\n");

                    // Adding row to table model
                    tableModel.addRow(row);
                }

                sb.append("\nTotal records found: ").append(rowCount);

                resultsArea.setText(sb.toString());
                resultsTable.setModel(tableModel);

                // Auto-adjust column widths
                for (int i = 0; i < resultsTable.getColumnModel().getColumnCount(); i++) {
                    resultsTable.getColumnModel().getColumn(i).setPreferredWidth(100);
                }

            } catch (SQLException e) {
                // Show user-friendly error message
                String errorMessage = "Error executing query!\n\n";
                errorMessage += "Your input: '" + query + "'\n";
                errorMessage += "Error: " + e.getMessage() + "\n\n";
                errorMessage += "Try one of these formats:\n";
                errorMessage += "1. ALL (to see all records)\n";
                errorMessage += "2. car_name LIKE '%ford%'\n";
                errorMessage += "3. mpg > 30\n";
                errorMessage += "4. cylinders = 4\n";
                errorMessage += "5. origin = 1\n";
                errorMessage += "6. model_year >= 80\n";
                errorMessage += "7. weight < 2000\n";
                errorMessage += "8. toyota (will search for toyota in car names)\n";
                errorMessage += "9. ford mustang (will search for 'ford mustang' in car names)\n";

                resultsArea.setText(errorMessage);
                resultsTable.setModel(new DefaultTableModel());
            }

        } catch (SQLException e) {
            resultsArea.setText("Database connection error: " + e.getMessage());
            e.printStackTrace();
        }
    }

    private static void loadDataFromFile(JTextArea resultsArea) {
        JFileChooser fileChooser = new JFileChooser();
        fileChooser.setDialogTitle("Select Auto MPG Data File");

        int result = fileChooser.showOpenDialog(null);
        if (result != JFileChooser.APPROVE_OPTION) {
            return;
        }

        try {
            FileReader file = new FileReader(fileChooser.getSelectedFile());
            BufferedReader reader = new BufferedReader(file);

            try (Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
                // Clear existing data
                Statement stmt = conn.createStatement();
                stmt.executeUpdate("DELETE FROM auto_mpg");

                // Insert new data
                String sql = "INSERT INTO auto_mpg (mpg, cylinders, displacement, horsepower, weight, acceleration, model_year, origin, car_name) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)";
                PreparedStatement pstmt = conn.prepareStatement(sql);

                String line;
                int count = 0;

                while ((line = reader.readLine()) != null) {
                    line = line.trim();
                    if (line.isEmpty()) continue;

                    try {
                        String[] parts = line.split("\\s+");

                        if (parts.length < 8) continue;

                        String mpgStr = parts[0];
                        String cylindersStr = parts[1];
                        String displacementStr = parts[2];
                        String horsepowerStr = parts[3];
                        String weightStr = parts[4];
                        String accelerationStr = parts[5];
                        String modelYearStr = parts[6];
                        String originStr = parts[7];

                        StringBuilder carNameBuilder = new StringBuilder();
                        for (int i = 8; i < parts.length; i++) {
                            carNameBuilder.append(parts[i]).append(" ");
                        }
                        String carName = carNameBuilder.toString().trim().replace("\"", "");

                        // Setting parameters
                        setParameter(pstmt, 1, mpgStr);
                        pstmt.setInt(2, parseInteger(cylindersStr));
                        setParameter(pstmt, 3, displacementStr);
                        setParameter(pstmt, 4, horsepowerStr);
                        setParameter(pstmt, 5, weightStr);
                        setParameter(pstmt, 6, accelerationStr);
                        pstmt.setInt(7, parseInteger(modelYearStr));
                        pstmt.setInt(8, parseInteger(originStr));
                        pstmt.setString(9, carName);

                        pstmt.executeUpdate();
                        count++;

                    } catch (Exception e) {
                        System.err.println("Error parsing line: " + line);
                    }
                }

                resultsArea.setText("Successfully loaded " + count + " records from file!\n" +
                        "Use the query field and sliders to filter the data.");
                reader.close();

            } catch (SQLException e) {
                resultsArea.setText("Database error: " + e.getMessage());
            }

        } catch (Exception e) {
            resultsArea.setText("File error: " + e.getMessage());
        }
    }

    private static void executeQuery(String query, JTextArea resultsArea, JTable resultsTable) {
        // This method is kept for compatibility, but the code runs through executeQueryWithSliders instead
        executeQueryWithSliders(query, new JSlider(0, 50, 0), new JSlider(0, 50, 50),
                new JSlider(0, 250, 0), new JSlider(0, 250, 250),
                false, resultsArea, resultsTable);
    }
}

```

## This is my result!

<img width="3826" height="2087" alt="image" src="https://github.com/user-attachments/assets/82c02afc-b249-4eec-984e-bc6e711466b7" />


# VIDEO EXPLANATION!



https://app.screencastify.com/watch/o9v4TY33Z1sVNhjxDfo2?checkOrg=30498c8e-8a1a-49cb-811f-fc81a0db6696




