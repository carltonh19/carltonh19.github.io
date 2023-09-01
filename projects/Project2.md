---
layout: project
type: project
image: img/sudoku.png
title: "Sudoku"
date: 2022
published: true
labels:
  - Homework assignemnt
  - Java
  - ICS 211
summary: "I made a soduku program from the Homework assignment of ICS 211"
---

<div class="text-center p-4">
  <img width="200px" src="../img/sudoku.png" class="img-thumbnail" >
</div>

Here is my Sudoku homework assignment from ICS 211.
```java
package edu.ics211.h09;

import java.util.ArrayList;

/**
 * Class for recursively finding a solution to a Sudoku problem.
 *
 * @author Biagioni, Edoardo, Cam Moore
 * @date October 23, 2013
 * @missing solveSudoku and legalValues, to be implemented by the students in ICS 211
 */
public class Sudoku {

  /**
   * Find an assignment of values to sudoku cells that makes the sudoku valid.
   *
   * @param the sudoku to be solved
   * @return whether a solution was found if a solution was found, the sudoku is filled in with the solution if no
   * solution was found, restores the sudoku to its original value
   */
  public static boolean solveSudoku(int[][] sudoku) {
    // TODO: Implement this method recursively. You may use a recursive
    // scan for a 0
    // loop over the rows
    //    loop over the columns
    //      if cell is a 0
    //          get the legal values
    //          if null {return false
    // system.out.println row, col, legal vlaue
    //          loop over the legal values
    //            fill the cell with the value
    //            if solveSudoku return true
    //              return true
    //          set cell back to 0
    //          return false
    // return checkSudoku(sudoku) base case 1
    // helper method.
    for (int row = 0; row < 9; row++) {
      for (int column = 0; column < 9; column++) {
        if (sudoku[row][column] == 0) {
          ArrayList<Integer> legalVal = legalValues(sudoku, row, column);
          if (legalVal == null) {
            return false;
          }
          for (int val:legalVal) {
            sudoku[row][column] = val;
            if (solveSudoku(sudoku)) {
              return true;
            }
          }
          sudoku[row][column] = 0;
          return false;
        }
      }
    }
    return true;
  }


  /**
   * Find the legal values for the given sudoku and cell.
   *
   * @param sudoku, the sudoku being solved.
   * @param row the row of the cell to get values for.
   * @param col the column of the cell.
   * @return an ArrayList of the valid values.
   */
  public static ArrayList<Integer> legalValues(int[][] sudoku, int row, int column) {
    // TODO: Implement this method. You may want to look at the checkSudoku method
    // create the return ArrayList and fill it with the legal values 1 .. 9
    // remove the values in the row
    // remove the values in the column
    // remove the values in the 3x3 square
    // retVal.size() is 0 return null
    // to see how it finds conflicts.
    ArrayList<Integer> returnList = new ArrayList<Integer>();
    if (sudoku[row][column] == 0) {
      for (int i = 1; i < 10; i++) {
        sudoku[row][column] = i;
        if (checkSudoku(sudoku,true)) {
          returnList.add(i);
        }
      }
      sudoku[row][column] = 0;
      //System.out.println(returnList.toString());
      return returnList;
    }
    return null;
  }


  /**
   * checks that the sudoku rules hold in this sudoku puzzle. cells that contain 0 are not checked.
   *
   * @param the sudoku to be checked
   * @param whether to print the error found, if any
   * @return true if this sudoku obeys all of the sudoku rules, otherwise false
   */
  public static boolean checkSudoku(int[][] sudoku, boolean printErrors) {
    if (sudoku.length != 9) {
      if (printErrors) {
        System.out.println("sudoku has " + sudoku.length + " rows, should have 9");
      }
      return false;
    }
    for (int i = 0; i < sudoku.length; i++) {
      if (sudoku[i].length != 9) {
        if (printErrors) {
          System.out.println("sudoku row " + i + " has " + sudoku[i].length + " cells, should have 9");
        }
        return false;
      }
    }
    /* check each cell for conflicts */
    for (int i = 0; i < sudoku.length; i++) {
      for (int j = 0; j < sudoku.length; j++) {
        int cell = sudoku[i][j];
        if (cell == 0) {
          continue; /* blanks are always OK */
        }
        if ((cell < 1) || (cell > 9)) {
          if (printErrors) {
            System.out.println("sudoku row " + i + " column " + j + " has illegal value " + cell);
          }
          return false;
        }
        /* does it match any other value in the same row? */
        for (int m = 0; m < sudoku.length; m++) {
          if ((j != m) && (cell == sudoku[i][m])) {
            if (printErrors) {
              System.out.println("sudoku row " + i + " has " + cell + " at both positions " + j + " and " + m);
            }
            return false;
          }
        }
        /* does it match any other value it in the same column? */
        for (int k = 0; k < sudoku.length; k++) {
          if ((i != k) && (cell == sudoku[k][j])) {
            if (printErrors) {
              System.out.println("sudoku column " + j + " has " + cell + " at both positions " + i + " and " + k);
            }
            return false;
          }
        }
        /* does it match any other value in the 3x3? */
        for (int k = 0; k < 3; k++) {
          for (int m = 0; m < 3; m++) {
            int testRow = (i / 3 * 3) + k; /* test this row */
            int testCol = (j / 3 * 3) + m; /* test this col */
            if ((i != testRow) && (j != testCol) && (cell == sudoku[testRow][testCol])) {
              if (printErrors) {
                System.out.println("sudoku character " + cell + " at row " + i + ", column " + j
                    + " matches character at row " + testRow + ", column " + testCol);
              }
              return false;
            }
          }
        }
      }
    }
    return true;
  }


  /**
   * Converts the sudoku to a printable string
   *
   * @param the sudoku to be converted
   * @param whether to check for errors
   * @return the printable version of the sudoku
   */
  public static String toString(int[][] sudoku, boolean debug) {
    if ((!debug) || (checkSudoku(sudoku, true))) {
      String result = "";
      for (int i = 0; i < sudoku.length; i++) {
        if (i % 3 == 0) {
          result = result + "+-------+-------+-------+\n";
        }
        for (int j = 0; j < sudoku.length; j++) {
          if (j % 3 == 0) {
            result = result + "| ";
          }
          if (sudoku[i][j] == 0) {
            result = result + "  ";
          } else {
            result = result + sudoku[i][j] + " ";
          }
        }
        result = result + "|\n";
      }
      result = result + "+-------+-------+-------+\n";
      return result;
    }
    return "illegal sudoku";
  }
}

```
