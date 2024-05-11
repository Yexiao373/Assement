import uk.ac.leedsbeckett.oop.OOPGraphics;

import javax.imageio.ImageIO;
import javax.swing.*;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.*;
import java.util.ArrayList;

/**
 * TurtleGraphics class extends OOPGraphics and provides additional functionalities
 * for drawing shapes, saving/loading images, and saving/loading commands.
 */
public class TurtleGraphics extends OOPGraphics {
    private boolean imageSaved;
    private boolean cmdSaved;
    private ArrayList<String> commands;

    /**
     * Main method to create an instance of TurtleGraphics.
     * @param args Command-line arguments.
     */
    public static void main(String[] args) {
        new TurtleGraphics();
    }

    /**
     * Constructor for TurtleGraphics.
     * Initializes flags and sets up the GUI.
     */
    public TurtleGraphics() {
        imageSaved = false;
        cmdSaved = false;
        commands = new ArrayList<>();

        JFrame MainFrame = new JFrame();
        MainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        MainFrame.setLayout(new FlowLayout());
        MainFrame.add(this);
        MainFrame.pack();
        MainFrame.setVisible(true);
        about();
    }

    /**
     * Processes the input command.
     * @param s The input command.
     */
    @Override
    public void processCommand(String s) {
        s = s.trim();
        if (s.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Error: Command is empty.");
            return;
        }

        String[] part = s.replaceAll("\\s+", " ").split(" ");
        switch (part[0].toLowerCase()) {
            case "about":
                about();
                imageSaved = false;
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "penup":
                penUp();
                imageSaved = false;
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "pendown":
                penDown();
                imageSaved = false;
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "turnleft":
                cmdTurnLeft(part);
                break;
            case "turnright":
                cmdTurnRight(part);
                break;
            case "forward":
                cmdForward(part, true);
                break;
            case "backward":
                cmdForward(part, false);
                break;
            case "black":
                setPenColour(Color.BLACK);
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "green":
                setPenColour(Color.GREEN);
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "red":
                setPenColour(Color.RED);
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "white":
                setPenColour(Color.WHITE);
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "reset":
                reset();
                imageSaved = false;
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "clear":
                clear();
                imageSaved = false;
                cmdSaved = false;
                commands.add(part[0].toLowerCase());
                break;
            case "saveimage":
                saveImage();
                break;
            case "loadimage":
                loadImage();
                break;
            case "savecmd":
                saveCmd();
                break;
            case "loadcmd":
                loadCmd();
                break;
            case "square":
                square(part);
                break;
            case "pencolour":
                pencolour(part);
                break;
            case "penwidth":
                penwidth(part);
                break;
            case "triangle":
                triangle(part);
                break;
            default:
                JOptionPane.showMessageDialog(this, "Error: Invalid command.");
        }
    }

    /**
     * Displays information about the application.
     */
    @Override
    public void about() {
        super.about();
        getGraphicsContext().drawString("Yingrui Peng", 228, 223);
    }

    /**
     * Draws a triangle with specified sides.
     * @param part The command parts.
     */
    private void triangle(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        int size1 = 0, size2 = 0, size3 = 0;
        try {
            int size = Integer.parseInt(part[1]);
            size1 = size;
            size2 = size;
            size3 = size;
        } catch (NumberFormatException e) {
            String[] part1Strs = part[1].split(",");
            if (part1Strs.length != 3) {
                JOptionPane.showMessageDialog(this, "Error: Invalid parameters of RGB.");
                return;
            }

            try {
                size1 = Integer.parseInt(part1Strs[0]);
                size2 = Integer.parseInt(part1Strs[1]);
                size3 = Integer.parseInt(part1Strs[2]);
            } catch (NumberFormatException e1) {
                JOptionPane.showMessageDialog(this, e1.getMessage());
                return;
            }
        }

        drawTriangle(size1, size2, size3);

        imageSaved = false;
        cmdSaved = false;
        commands.add(String.join(" ", part));
    }

    /**
     * draw triangle
     * @param side1
     * @param side2
     * @param side3
     */
    private void drawTriangle(int side1, int side2, int side3) {
        if (side1 + side2 > side3 && side2 + side3 > side1 && side1 + side3 > side2) {
            forward(side1);
            turnRight((int) (180 - angle(side1, side2, side3)));
            forward(side2);
            turnRight((int) (180 - angle(side2, side3, side1)));
            forward(side3);
            turnRight((int) (180 - angle(side3, side1, side2)));
        } else {
            JOptionPane.showMessageDialog(this, "Error: Invalid side lengths for a triangle.");
        }
    }

    /**
     * help method
     * @param a
     * @param b
     * @param c
     * @return
     */
    private double angle(int a, int b, int c) {
        return Math.toDegrees(Math.acos((a * a + b * b - c * c) / (2.0 * a * b)));
    }

    /**
     * set pen width
     * @param part
     */
    private void penwidth(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            int width = Integer.parseInt(part[1]);
            setStroke(width);

            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }

    /**
     * set pen color
     * @param part
     */
    private void pencolour(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            String[] part1Strs = part[1].split(",");
            if (part1Strs.length != 3) {
                JOptionPane.showMessageDialog(this, "Error: Invalid parameters of RGB.");
                return;
            }

            Color color = new Color(Float.parseFloat(part1Strs[0]), Float.parseFloat(part1Strs[1]), Float.parseFloat(part1Strs[2]));
            setPenColour(color);

            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }

    /**
     * draw square
     * @param part
     */
    private void square(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            for (int i = 0; i < 4; i++) {
                forward(part[1]);
                turnRight();
            }

            imageSaved = false;
            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }

    /**
     * save commands
     */
    private void saveCmd() {
        JFileChooser fileChooser = new JFileChooser();
        int returnVal = fileChooser.showSaveDialog(this);
        if (returnVal == JFileChooser.APPROVE_OPTION) {
            File file = fileChooser.getSelectedFile();
            try {
                PrintWriter writer = new PrintWriter(file);
                for (String command : commands) {
                    writer.println(command);
                }
                cmdSaved = true;
                commands.clear();
                JOptionPane.showMessageDialog(this, "Commands saved successfully.");
            } catch (IOException e) {
                JOptionPane.showMessageDialog(this, "Error saving commands: " + e.getMessage());
            }
        }
    }

    /**
     * load commands
     */
    private void loadCmd() {
        if (!cmdSaved) {
            int result = JOptionPane.showConfirmDialog(this, "There are unsaved changes. Do you want to save them first?", "Unsaved Changes", JOptionPane.YES_NO_CANCEL_OPTION);
            if (result == JOptionPane.YES_OPTION) {
                saveCmd();
            } else if (result == JOptionPane.CANCEL_OPTION) {
                return;
            }
        }

        JFileChooser fileChooser = new JFileChooser();
        int returnVal = fileChooser.showOpenDialog(this);
        if (returnVal == JFileChooser.APPROVE_OPTION) {
            File file = fileChooser.getSelectedFile();
            try {
                BufferedReader reader = new BufferedReader(new FileReader(file));
                String line;
                while ((line = reader.readLine()) != null) {
                    if (!line.isEmpty()) {
                        System.out.println(line);
                        processCommand(line);
                    }
                }
                JOptionPane.showMessageDialog(this, "Commands loaded and executed successfully.");
            } catch (IOException e) {
                JOptionPane.showMessageDialog(this, "Error loading commands: " + e.getMessage());
            }
        }
    }

    /**
     * save image
     */
    private void saveImage() {
        JFileChooser fileChooser = new JFileChooser();
        int returnVal = fileChooser.showSaveDialog(this);
        if (returnVal == JFileChooser.APPROVE_OPTION) {
            File file = fileChooser.getSelectedFile();
            try {
                ImageIO.write(getBufferedImage(), "png", file);
                imageSaved = true;
                JOptionPane.showMessageDialog(this, "Image saved successfully.");
            } catch (IOException e) {
                JOptionPane.showMessageDialog(this, "Error saving image: " + e.getMessage());
            }
        }
    }

    /**
     * load image
     */
    private void loadImage() {
        if (!imageSaved) {
            int result = JOptionPane.showConfirmDialog(this, "There are unsaved changes. Do you want to save them first?", "Unsaved Changes", JOptionPane.YES_NO_CANCEL_OPTION);
            if (result == JOptionPane.YES_OPTION) {
                saveImage();
            } else if (result == JOptionPane.CANCEL_OPTION) {
                return;
            }
        }

        JFileChooser fileChooser = new JFileChooser();
        int returnVal = fileChooser.showOpenDialog(this);
        if (returnVal == JFileChooser.APPROVE_OPTION) {
            File file = fileChooser.getSelectedFile();
            try {
                BufferedImage loadedImage = ImageIO.read(file);
                setBufferedImage(loadedImage);
                imageSaved = false;
                JOptionPane.showMessageDialog(this, "Image loaded successfully.");
            } catch (IOException e) {
                JOptionPane.showMessageDialog(this, "Error loading image: " + e.getMessage());
            }
        }
    }

    /**
     * turn left
     * @param part
     */
    private void cmdTurnLeft(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            turnLeft(part[1]);
            imageSaved = false;
            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }

    /**
     * turn right
     * @param part
     */
    private void cmdTurnRight(String[] part) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            turnRight(part[1]);
            imageSaved = false;
            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }

    /**
     * forward or backward
     * @param part
     * @param forward
     */
    private void cmdForward(String[] part, boolean forward) {
        if (part.length != 2) {
            JOptionPane.showMessageDialog(this, "Error: Command parameters must 2.");
            return;
        }

        try {
            if (!forward) {
                turnRight(180);
            }

            forward(part[1]);
            imageSaved = false;
            cmdSaved = false;
            commands.add(String.join(" ", part));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, e.getMessage());
        }
    }
}
