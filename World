package com.company;

import java.util.Random;

/*
 * This class handles the World object and storing all data related to it
 */
public class World {
    private char[][] world;
    private int[][] neighbors;
    private final int worldSize;
    private int generation;
    private int alive;

    public World(int worldSize) {
        this.worldSize = worldSize;
        this.world = new char[worldSize][worldSize];
        this.neighbors = new int[worldSize][worldSize];

        Random random = new Random();

        int count = 0;

        for (int i = 0; i < worldSize; i++) {
            for (int j = 0; j < worldSize; j++) {
                world[i][j] = random.nextBoolean() ? 'O' : ' ';
                if (world[i][j] == 'O') {
                    count++;
                }
            }
        }

        for (int i = 0; i < worldSize; i++) {
            for (int j = 0; j < worldSize; j++) {
                neighbors[i][j] = getNumNeighbors(world, i, j);
            }
        }

        alive = count;
        generation++;
    }

    /*
     * Get the next generation of the world depending on their neighbors in the previous generation
     */
    public void nextGeneration() {
        char[][] nextGeneration = new char[worldSize][worldSize];
        int[][] nextGenNeighbors = new int[worldSize][worldSize];
        int count = 0;

        for (int y = 0; y < nextGeneration.length; y++) {
            for (int x = 0; x < nextGeneration[y].length; x++) {
                int numNeighbors = getNumNeighbors(world, y, x);
                nextGenNeighbors[y][x] = numNeighbors;

                if (numNeighbors == 3 || (world[y][x] == 'O' && numNeighbors == 2)) {
                    nextGeneration[y][x] = 'O';
                    count++;
                } else {
                    nextGeneration[y][x] = ' ';
                }
            }
        }

        alive = count;
        world = nextGeneration;
        neighbors = nextGenNeighbors;
        generation++;
    }

    private int getNumNeighbors(char[][] world, int y, int x) {
        int numNeighbors = 0;

        for (int i = -1; i < 2; i++) {
            int row = y + i < 0 ? worldSize - 1 : y + i;
            row = row >= worldSize ? 0 : row;
            for (int j = -1; j < 2; j++) {
                int col = x + j < 0 ? worldSize - 1 : x + j;
                col = col >= worldSize ? 0 : col;

                if (!(row == y && col == x) && world[row][col] == 'O') {
                    numNeighbors++;
                }
            }
        }

        return numNeighbors;
    }

    public char[][] getCharArray() {
        return world;
    }

    public int[][] getNeighborArray() {
        return neighbors;
    }

    public int getGeneration() {
        return generation;
    }

    public int getAlive() {
        return alive;
    }
}
