### **Day 48: Coverage-Driven Verification – Basics**

---

### **Objective:**

Understand the fundamentals of **coverage-driven verification (CDV)** in SystemVerilog. Learn how coverage is used to measure test completeness and improve verification quality.

---

## ✅ 1. What is Coverage in SystemVerilog?

**Coverage** helps determine how thoroughly a design has been exercised by your testbench. It tells you **what has been tested**, and more importantly, **what has not**.

Two primary types:

1. **Code coverage** (handled by simulator): Checks if lines, conditions, FSMs, etc. were executed.
2. **Functional coverage** (written by user): Checks if all desired input combinations or functional scenarios occurred.

This topic focuses on **functional coverage**.

---

## ✅ 2. Coverage Groups

A **covergroup** defines the set of conditions or variables you want to track.

### Syntax:

```systemverilog
covergroup covgrp;
    coverpoint variable_name;
endgroup
```

You can also define it inside a class or module.

---

## ✅ 3. Coverpoints

A **coverpoint** is a variable or expression you want to monitor.

### Example:

```systemverilog
covergroup cg;
    coverpoint opcode;  // covers all values of opcode
endgroup
```

---

## ✅ 4. Bins

Each coverpoint is broken into **bins**, which represent **value ranges or events** you're interested in.

### Example with bins:

```systemverilog
covergroup cg;
    coverpoint opcode {
        bins op_read  = {0};
        bins op_write = {1};
        bins others   = {[2:15]};
    }
endgroup
```

You can define:

* **Explicit bins**: named bins for specific values or ranges
* **Implicit bins**: created automatically for all values
* **Illegal bins**: define forbidden conditions

---

## ✅ 5. Creating and Sampling a Covergroup

You must create a covergroup instance and call `.sample()` to record values:

```systemverilog
class Transaction;
    rand bit [3:0] opcode;

    covergroup cg;
        coverpoint opcode;
    endgroup

    function new();
        cg = new(); // construct covergroup
    endfunction

    function void sample();
        cg.sample(); // sample current values
    endfunction
endclass
```

---

## ✅ 6. Testbench Example

```systemverilog
module tb_coverage;

    Transaction t = new();

    initial begin
        repeat (10) begin
            t.randomize();
            $display("Opcode = %0d", t.opcode);
            t.sample(); // logs the value into coverage bins
        end
    end

endmodule
```

---

## ✅ 7. Viewing Coverage

* Most simulators (like ModelSim, VCS, Questa, etc.) provide GUI or reports to view coverage.
* You can also use `$coverage_get` functions to print it from code.

---

## 🧠 Summary

| Feature            | Description                               |
| ------------------ | ----------------------------------------- |
| `covergroup`       | Defines what to cover                     |
| `coverpoint`       | Variable or expression to track           |
| `bins`             | Values or ranges to record                |
| `.sample()`        | Records current values into coverage      |
| Code vs Functional | Code: automatic, Functional: user-defined |

