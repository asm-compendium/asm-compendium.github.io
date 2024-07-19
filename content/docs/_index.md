---
title: Documentation
next: first-page
---

This is a demo of the theme's documentation layout.

## Hello, World!

<div class="columns">
  <div class="column">
    ```go {filename="main.go"}
    package main

    import "fmt"

    func main() {
        fmt.Println("Hello, World!")
    }
    ```
  </div>

  <div class="column">
    ```go {filename="main_2.go"}
    package main

    import "fmt"

    func main(a) {
        fmt.Println(a)
    }
    ```
  </div>
</div>

<style>
.columns {
  display: flex;
  justify-content: space-between;
}
.column {
  flex: 1;
  margin-right: 10px;
}
.column:last-child {
  margin-right: 0;
}
</style>
