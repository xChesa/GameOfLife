package com.company;

import javax.swing.*;
import java.awt.*;
import java.util.Objects;

/*
 * This class handles the game state and the GUI displayed to the user
 */
public class GameOfLife extends JFrame {

    private World world;
    private boolean paused;
    private boolean reset;
    private JLabel generationLabel;
    private JLabel aliveLabel;
    private JPanel worldPanel;
    private JSlider speedSlider;
    private JToggleButton pauseToggleButton;
    private JTextField worldSizeField;
    private String mode;
    private JCheckBox heatMapSelector;
    private JCheckBox customColorSelector;
    private JComboBox<String> colorSelector;
    private JComboBox<String> backgroundColorSelector;

    /*
     * Set up the GUI
     */
    public GameOfLife() {
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(1000, 900);
        setLayout(new BorderLayout());

        //this.paused = false;
        this.reset = false;
        this.mode = "Default";

        initComponents();

        pauseToggleButton.setSelected(true);
        paused = true;

        setVisible(true);
    }

    /*
     * Creates two primary sections: one for the world, and one for the controls
     */
    private void initComponents() {
        add(initControlPanel(), BorderLayout.WEST);
        add(initWorldBox());
    }

    /*
     * Initializes the control panel in two parts: one for info, one for buttons
     */
    private JPanel initControlPanel() {
        JPanel controlPanel = new JPanel();
        controlPanel.setLayout(new BorderLayout());

        controlPanel.add(initControlButtons(), BorderLayout.NORTH);
        controlPanel.add(initInfoBox());

        return controlPanel;
    }

    private JPanel initControlButtons() {
        JPanel buttonPanel = new JPanel();

        pauseToggleButton = new JToggleButton("Pause");
        pauseToggleButton.addActionListener(e -> paused = pauseToggleButton.isSelected());

        JButton resetButton = new JButton("Reset");
        resetButton.addActionListener(e -> {
            pauseToggleButton.setSelected(true);
            paused = true;
            reset = true;
        });

        buttonPanel.add(pauseToggleButton);
        buttonPanel.add(resetButton);

        return buttonPanel;
    }

    private JPanel initInfoBox() {
        JPanel infoPanel = new JPanel();
        infoPanel.setLayout(new BoxLayout(infoPanel, BoxLayout.Y_AXIS));

        generationLabel = new JLabel("Generation #0");
        aliveLabel = new JLabel("Alive: 0");

        speedSlider = new JSlider(1, 10, 5);


        infoPanel.add(Box.createRigidArea(new Dimension(0, 20)));
        infoPanel.add(generationLabel);
        infoPanel.add(aliveLabel);
        infoPanel.add(Box.createRigidArea(new Dimension(0, 20)));
        infoPanel.add(new JLabel("Speed Mode:"));
        infoPanel.add(speedSlider);
        infoPanel.add(Box.createRigidArea(new Dimension(0, 20)));
        infoPanel.add(initWorldSizePanel());
        infoPanel.add(Box.createRigidArea(new Dimension(0, 20)));
        infoPanel.add(initColorOptions());


        return infoPanel;
    }

    private JPanel initWorldSizePanel() {
        JPanel worldSizePanel = new JPanel();
        worldSizePanel.setMaximumSize(new Dimension(300, 40));

        worldSizeField = new JTextField(3);

        worldSizePanel.add(new JLabel("World Size:"));
        worldSizePanel.add(worldSizeField);


        return worldSizePanel;
    }

    /*
     * Initialize the color options panel, which handles things like color mode selection and custom color selection
     */
    private JPanel initColorOptions() {
        JPanel colorOptionsPanel = new JPanel();
        colorOptionsPanel.setLayout(new BoxLayout(colorOptionsPanel, BoxLayout.Y_AXIS));

        heatMapSelector = new JCheckBox("Heatmap");
        heatMapSelector.addActionListener(e -> {
            mode = heatMapSelector.isSelected() ? "Heatmap" : "Default";
            customColorSelector.setSelected(false);
            displayWorld(world);
        });
        customColorSelector = new JCheckBox("Custom Color");
        customColorSelector.addActionListener(e -> {
            mode = customColorSelector.isSelected() ? "Custom" : "Default";
            heatMapSelector.setSelected(false);
            displayWorld(world);
        });
        String[] colors = {
                "Dark Gray", "Black", "Blue", "Cyan", "Gray", "Light Gray", "Green", "Magenta", "Orange", "Pink", "Red", "Yellow", "White"
        };
        colorSelector = new JComboBox<>(colors);
        colorSelector.setMaximumSize(new Dimension(250, 20));
        colorSelector.addActionListener(e -> {
            if (customColorSelector.isSelected()) {
                displayWorld(world);
            }
        });
        backgroundColorSelector = new JComboBox<>(colors);
        backgroundColorSelector.setMaximumSize(new Dimension(250, 20));
        backgroundColorSelector.setSelectedItem("White");
        backgroundColorSelector.addActionListener(e -> displayWorld(world));

        colorOptionsPanel.add(new JLabel("Color Options:"));
        colorOptionsPanel.add(Box.createRigidArea(new Dimension(0, 10)));
        colorOptionsPanel.add(heatMapSelector);
        colorOptionsPanel.add(customColorSelector);
        colorOptionsPanel.add(new JLabel("Foreground Color"));
        colorOptionsPanel.add(colorSelector);
        colorOptionsPanel.add(new JLabel("Background Color"));
        colorOptionsPanel.add(backgroundColorSelector);

        return colorOptionsPanel;
    }

    /*
     * Initializes the section of the GUI where the world will be represented
     */
    private JPanel initWorldBox() {
        worldPanel = new JPanel();
        worldPanel.setLayout(new GridLayout(10, 10, 1, 1));
        worldPanel.setBackground(Color.BLACK);

        return worldPanel;
    }

    public void displayWorld(World world) {
        this.world = world;
        worldPanel.removeAll();
        char[][] charArray = world.getCharArray();
        for (int i = 0; i < charArray.length; i++) {
            for (int j = 0; j < charArray[i].length; j++) {
                JPanel panel = new JPanel();
                Color color;
                char ch = charArray[i][j];
                switch (mode) {
                    case "Custom":
                        color = selectColorCustom(ch);
                        break;
                    case "Heatmap":
                        int neighbors = world.getNeighborArray()[i][j];
                        color = SelectColorHeatmap(ch == 'O' ? neighbors + 1 : neighbors);
                        break;
                    default:
                        color = selectColorDefault(ch);
                        break;
                }
                panel.setBackground(color);
                worldPanel.add(panel);
            }
        }
        generationLabel.setText("Generation #" + world.getGeneration());
        aliveLabel.setText("Alive: " + world.getAlive());
        worldPanel.revalidate();
        worldPanel.repaint();

        if (world.getAlive() <= 0) {
            pauseToggleButton.doClick();
        }
    }

    private Color selectColorDefault(char ch) {
        return ch == 'O' ? Color.DARK_GRAY : Color.WHITE;
    }

    private Color selectColorCustom(char ch) {
        Color color = Color.WHITE;
        //if (ch == 'O') {
            switch (ch == 'O' ? Objects.requireNonNull(colorSelector.getSelectedItem()).toString()
                    : Objects.requireNonNull(backgroundColorSelector.getSelectedItem()).toString()) {
                case "Blue":
                    color = Color.BLUE;
                    break;
                case "Cyan":
                    color = Color.CYAN;
                    break;
                case "Black":
                    color = Color.BLACK;
                    break;
                case "Gray":
                    color = Color.GRAY;
                    break;
                case "Dark Gray":
                    color = Color.DARK_GRAY;
                    break;
                case "Light Gray":
                    color = Color.LIGHT_GRAY;
                    break;
                case "Green":
                    color = Color.GREEN;
                    break;
                case "Magenta":
                    color = Color.MAGENTA;
                    break;
                case "Orange":
                    color = Color.ORANGE;
                    break;
                case "Pink":
                    color = Color.PINK;
                    break;
                case "Red":
                    color = Color.RED;
                    break;
                case "Yellow":
                    color = Color.YELLOW;
                    break;
                case "White":
                    color = Color.WHITE;
                    break;
            }
        //}
        return color;
    }

    /*
     * Heatmap color mode selects different colors based on the number of neighbors in an area
     */
    private Color SelectColorHeatmap(int neighbors) {
        Color color;
        switch (neighbors) {
            case 0:
                color = Color.WHITE;
                break;
            case 1:
                color = Color.BLUE;
                break;
            case 2:
                color = Color.GREEN;
                break;
            case 3:
                color = Color.YELLOW;
                break;
            case 4:
                color = Color.ORANGE;
                break;
            default:
                color = Color.RED;
                break;
        }

        return color;
    }

    public void setWorldSize(int worldSize) {
        worldPanel.setLayout(new GridLayout(worldSize, worldSize, 1, 1));
    }

    public boolean isPaused() {
        return paused;
    }

    public boolean isReset() {
        return reset;
    }

    public void setReset(boolean reset) {
        this.reset = reset;
    }

    public int getSpeed() {
        return speedSlider.getValue();
    }

    public int getWorldSize() {
        if (!worldSizeField.getText().isBlank() && worldSizeField.getText().matches("\\d*")) {
            int worldSize = Integer.parseInt(worldSizeField.getText());
            if (worldSize > 0) {
                return worldSize;
            }
        }

        return 10;
    }
}
