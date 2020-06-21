package com.company;

/*
 * This class acts as an interface between the GUI and the World object that will be displayed
 */
public class Controller {
    private final GameOfLife gol;
    private World world;

    public Controller() {
        this.gol = new GameOfLife();
        this.world = newWorld();
    }

    /*
     * This method keeps the program running and handles things such as pausing and resetting the world
     */
    public void run() {
        while (true) {
            gol.displayWorld(world); // Send the World object to the GUI to display

            try { // Delay the thread depending on the speed setting of the GUI
                Thread.sleep(2000 / gol.getSpeed());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if (gol.isReset()) {
                world = newWorld();
                gol.setReset(false);
                gol.displayWorld(world);
            }

            while (gol.isPaused()) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            world.nextGeneration();
        }
    }

    private World newWorld() {
        int worldSize = gol.getWorldSize();
        World world = new World(worldSize);
        gol.setWorldSize(worldSize);
        return world;
    }
}
