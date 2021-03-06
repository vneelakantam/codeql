# Improvements to C/C++ analysis

The following changes in version 1.25 affect C/C++ analysis in all applications.

## General improvements

## New queries

| **Query**                   | **Tags**  | **Purpose**                                                        |
|-----------------------------|-----------|--------------------------------------------------------------------|

## Changes to existing queries

| **Query**                  | **Expected impact**    | **Change**                                                       |
|----------------------------|------------------------|------------------------------------------------------------------|

## Changes to libraries

* The data-flow library has been improved, which affects most security queries by potentially
  adding more results. Flow through functions now takes nested field reads/writes into account.
  For example, the library is able to track flow from `taint()` to `sink()` via the method
  `getf2f1()` in
  ```c
  struct C {
      int f1;
  };

  struct C2
  {
      C f2;

      int getf2f1() {
          return f2.f1; // Nested field read
      }

      void m() {
          f2.f1 = taint();
          sink(getf2f1()); // NEW: taint() reaches here
      }
  };
  ```
