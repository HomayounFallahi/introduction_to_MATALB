# MATLAB Operators and Logic

## Learning Objectives

By the end of this lecture, you should be able to:

- Identify and use arithmetic, relational, and logical operators in MATLAB
- Understand operator precedence and how to control evaluation order
- Distinguish between matrix operations (`*`, `/`, `^`) and element-wise operations (`.*`, `./`, `.^`)
- List and use MATLAB arithmetic operators (`+ - * / \ ^ .* ./ .\ .^ .'`), relational operators (`== ~= < <= > >=`), and logical operators (`& | ~ && ||`)
- Apply logical operators (AND, OR, NOT, XOR, XNOR) to create complex conditional expressions
- Use truth tables to understand logical behavior
- Implement logical indexing to filter and extract data from arrays
- Apply logical programming to engineering problems such as process monitoring and data quality control

---

## Why This Topic Matters

In chemical engineering, decisions drive calculations:

- **Process Control:** Does the reactor temperature exceed a safety limit?
- **Data Quality:** Which experimental measurements are valid?
- **Process Optimization:** Should the feed rate increase or decrease?
- **Equipment Safety:** Has the pressure in the tank exceeded the design maximum?

Operators and logical expressions allow you to automate these decisions in MATLAB. They are the foundation of conditional logic and are essential for writing code that responds to real-world engineering data.

Without mastery of operators and logic, you cannot write effective MATLAB programs for engineering applications.

---

## MATLAB Operators

### Concept

An **operator** is a symbol that tells MATLAB to perform a calculation or comparison. Operators take one or more inputs (called operands) and produce a result.

MATLAB has several categories of operators:

1. **Arithmetic operators** – perform mathematical calculations
2. **Relational operators** – compare values
3. **Logical operators** – combine or modify boolean conditions

### Arithmetic Operators

**What are they?**

Arithmetic operators perform standard mathematical operations: addition, subtraction, multiplication, division, and exponentiation.

**MATLAB Syntax:**

| Operation | Symbol | Example | Result |
|-----------|--------|---------|--------|
| Addition | `+` | `5 + 3` | `8` |
| Subtraction | `-` | `10 - 4` | `6` |
| Multiplication (matrix) | `*` | `[1 2] * [3; 4]` | `11` |
| Multiplication (element-wise) | `.*` | `[1 2] .* [3 4]` | `[3 8]` |
| Division (matrix) | `/` | `A / B` | Solves system |
| Division (element-wise) | `./` | `[8 4] ./ [2 2]` | `[4 2]` |
| Exponentiation (matrix) | `^` | `A ^ 2` | `A * A` |
| Exponentiation (element-wise) | `.^` | `[2 3] .^ 2` | `[4 9]` |

**Simple Example:**

```matlab
% Temperature conversions
T_celsius = 25;
T_fahrenheit = T_celsius * 9/5 + 32;    % Result: 77

% Reactor feed calculation
F_inlet = 2.5;                          % mol/s
x_A = 0.35;                             % mole fraction of A
F_A = F_inlet * x_A;                    % Result: 0.875 mol/s
```


### Matrix vs. Element-Wise Operations

**What is the difference?**

This is one of the most important distinctions in MATLAB.

**Matrix Operations** follow the rules of linear algebra:

- `A * B` multiplies matrices using the mathematical definition: the number of columns in A must equal the number of rows in B
- `A / B` solves the system A = X * B (matrix division)
- `A ^ 2` computes A * A (matrix multiplication)

**Element-Wise Operations** apply the operator to each corresponding pair of elements:

- `A .* B` multiplies each element in A by the corresponding element in B
- `A ./ B` divides each element in A by the corresponding element in B
- `A .^ 2` squares each element in A

**When should each be used?**

Use **matrix operations** when:
- You need to solve systems of linear equations
- You are performing mathematical operations on vectors or matrices where linear algebra applies

Use **element-wise operations** when:
- You want to apply a calculation to every element independently
- You have arrays of the same size and want to combine them element-by-element
- You are working with experimental data that does not follow linear algebra rules

**Example Comparing Matrix and Element-Wise:**

```matlab
% Two vectors
A = [2 4 6];
B = [1 2 3];

% Element-wise multiplication
result_elementwise = A .* B;     % [2 8 18]

% Matrix multiplication (not appropriate here)
% A * B would cause an error because A and B are both row vectors
% Matrix multiplication requires compatible dimensions

% For matrix multiplication to work with these row vectors:
A_row = [2 4 6];
B_col = [1; 2; 3];              % Column vector
result_matrix = A_row * B_col;   % 2*1 + 4*2 + 6*3 = 28
```

**Syntax Breakdown:**

- `A .* B` — the dot (`.`) before the operator means element-wise operation
- `A * B` — no dot means matrix operation
- The dimensions must be compatible for both operations, but the rules differ

**Chemical Engineering Example:**

Suppose you have concentration data from multiple reactors:

```matlab
% Concentration measurements (mol/L)
C_reactor1 = [0.5 1.2 0.8 1.1];      % Four time points
C_reactor2 = [0.6 1.1 0.9 1.3];      % Same time points

% Calculate conversion at each time point
% Assuming initial concentration C0 = 1.5 mol/L
C0 = 1.5;

% Element-wise subtraction gives change in concentration
delta_C1 = C0 - C_reactor1;          % [1.0 0.3 0.7 0.4]
delta_C2 = C0 - C_reactor2;          % [0.9 0.4 0.6 0.2]

% Element-wise division gives fractional conversion
conversion1 = delta_C1 ./ C0;        % [0.667 0.2 0.467 0.267]
conversion2 = delta_C2 ./ C0;        % [0.6 0.267 0.4 0.133]

% Percentage conversion
conversion1_percent = conversion1 .* 100;  % [66.7 20 46.7 26.7]
```

**Common Mistakes:**

1. **Forgetting the dot in element-wise operations:**

   ```matlab
   % WRONG
   result = A * B;              % Tries matrix multiplication (may error)
   
   % CORRECT
   result = A .* B;             % Element-wise multiplication
   ```

2. **Using element-wise when matrix operation is needed:**

   ```matlab
   % Solving Ax = b for x
   % WRONG
   x = A .\ b;                  % Wrong (element-wise)
   
   % CORRECT
   x = A \ b;                   % Correct (matrix division)
   ```

3. **Incompatible dimensions:**

   ```matlab
   % WRONG
   A = [1 2 3];
   B = [4 5];                   % Different sizes
   C = A .* B;                  % Error: incompatible dimensions
   
   % CORRECT
   A = [1 2 3];
   B = [4 5 6];                 % Same size
   C = A .* B;                  % [4 10 18]
   ```

### Relational Operators

**What are they?**

Relational operators compare two values and return a logical result: `true` (1) or `false` (0).

**MATLAB Syntax:**

| Comparison | Symbol | Example | Result |
|------------|--------|---------|--------|
| Equal to | `==` | `5 == 5` | `true` (1) |
| Not equal to | `~=` | `5 ~= 3` | `true` (1) |
| Less than | `<` | `3 < 5` | `true` (1) |
| Less than or equal | `<=` | `5 <= 5` | `true` (1) |
| Greater than | `>` | `5 > 3` | `true` (1) |
| Greater than or equal | `>=` | `3 >= 5` | `false` (0) |

**Important Note:**

- Use `==` for comparison (returns true/false)
- Use `=` for assignment (stores a value)

**Simple Example:**

```matlab
% Chemical composition check
T_actual = 85;                  % Actual reactor temperature (°C)
T_setpoint = 80;                % Target temperature (°C)
T_tolerance = 2;                % Tolerance (°C)

% Check if temperature is within tolerance
is_too_high = T_actual > (T_setpoint + T_tolerance);  % true (1)
is_too_low = T_actual < (T_setpoint - T_tolerance);   % false (0)
is_acceptable = ~(is_too_high | is_too_low);          % false (0)
```

**Syntax Breakdown:**

```matlab
is_too_high = T_actual > (T_setpoint + T_tolerance);
```

- `is_too_high` – variable storing the result (logical)
- `=` – assignment
- `T_actual` – left operand (85)
- `>` – relational operator (greater than)
- `(T_setpoint + T_tolerance)` – right operand (82)

**When working with arrays:**

```matlab
% Compare each element to a threshold
concentrations = [0.8 1.2 0.95 1.1];  % mol/L
threshold = 1.0;                       % mol/L

% Which concentrations exceed threshold?
exceeds = concentrations > threshold;  % [false true false true]
                                       % or [0 1 0 1]
```

Result is a logical array with the same size as the input.


### Operator Precedence

**What is operator precedence?**

When multiple operators appear in an expression, MATLAB evaluates them in a specific order, called **precedence**. Without understanding precedence, you may get unexpected results.

**MATLAB Operator Precedence (highest to lowest):**

| Precedence | Operators | Type |
|------------|-----------|------|
| 1 (highest) | `()` | Parentheses |
| 2 | `.^`, `^` | Exponentiation |
| 3 | `*`, `/`, `.*`, `./` | Multiplication and division |
| 4 | `+`, `-` | Addition and subtraction |
| 5 | `:` | Colon (array creation) |
| 6 | `==`, `~=`, `<`, `<=`, `>`, `>=` | Relational operators |
| 7 | `~` | Logical NOT |
| 8 | `&` | Logical AND |
| 9 (lowest) | `\|`, `xor` | Logical OR, XOR |

**Example showing precedence:**

```matlab
% Without considering precedence, this is ambiguous:
result = 2 + 3 * 4;

% Multiplication has higher precedence than addition
% So MATLAB evaluates as: 2 + (3 * 4) = 2 + 12 = 14
% NOT as: (2 + 3) * 4 = 5 * 4 = 20

% Use parentheses to make intent clear
result_clear = (2 + 3) * 4;     % 20
result_clear2 = 2 + (3 * 4);    % 14
```

**Chemical Engineering Example:**

```matlab
% Heat duty calculation: Q = m * cp * ΔT
m = 1000;                       % kg
cp = 4.18;                      % kJ/(kg·K)
T_in = 25;                      % °C
T_out = 65;                     % °C

% Without parentheses (multiplication before subtraction)
Q1 = m * cp * T_out - T_in;     % 1000 * 4.18 * 65 - 25 = 271,725 kJ (WRONG)

% With correct parentheses (subtraction first)
deltaT = T_out - T_in;          % 40 K
Q2 = m * cp * deltaT;           % 1000 * 4.18 * 40 = 167,200 kJ (CORRECT)

% Or in one line with parentheses
Q3 = m * cp * (T_out - T_in);   % 167,200 kJ (CORRECT)
```

**Key Takeaway:**

Always use parentheses to make the order of operations explicit, especially in engineering calculations where mistakes can have significant consequences.

---

## Logical Programming

### Concept

**Logical programming** uses true/false values (called **booleans**) to make decisions and control program flow. It involves:

1. Creating boolean conditions (true or false statements)
2. Combining conditions using logical operators
3. Using the results to filter data or make decisions

### Logical Operators

**What are they?**

Logical operators combine or modify boolean values. Unlike arithmetic operators, logical operators work with true/false values instead of numbers.

**MATLAB Logical Operators:**

| Operator | Symbol | Meaning | Example |
|----------|--------|---------|---------|
| Logical AND | `&` | Both conditions true | `(x > 0) & (x < 10)` |
| Logical OR | `\|` | At least one condition true | `(x < 0) \| (x > 10)` |
| Logical NOT | `~` | Negates/inverts condition | `~(x == 5)` |
| Logical XOR | `xor()` | One (but not both) true | `xor(x > 5, y < 3)` |
| Logical XNOR | `~xor()` | Both true or both false | `~xor(x, y)` |

Note:
- `&` and `|` work on arrays (element-wise).
- `&&` and `||` are short-circuit operators that work only on scalar logical expressions; they evaluate the second operand only when necessary.

**Simple Examples:**

```matlab
% Temperature safety check
T = 85;                         % °C
P = 12;                         % bar

% Both conditions must be true
safe = (T < 100) & (P < 15);    % true & true = true

% At least one condition must be true
emergency = (T > 120) | (P > 20);  % false | false = false

% Negation
not_emergency = ~emergency;     % ~false = true
```

**Syntax Breakdown:**

```matlab
safe = (T < 100) & (P < 15);
```

- `safe` – variable storing the result (logical: true or false)
- `=` – assignment
- `(T < 100)` – first condition (relational expression)
- `&` – logical AND operator
- `(P < 15)` – second condition

### Truth Tables

**What is a truth table?**

A truth table shows all possible combinations of input values and the resulting output for a logical operator.

**AND (&) Truth Table:**

| A | B | A & B |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

*AND is true only when both inputs are true.*

**OR (|) Truth Table:**

| A | B | A \| B |
|---|---|--------|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

*OR is true when at least one input is true.*

**NOT (~) Truth Table:**

| A | ~A |
|---|-----|
| T | F |
| F | T |

*NOT inverts the input.*

**XOR Truth Table:**

| A | B | xor(A,B) |
|---|---|----------|
| T | T | F |
| T | F | T |
| F | T | T |
| F | F | F |

*XOR is true when inputs are different.*

**XNOR Truth Table:**

| A | B | ~xor(A,B) |
|---|---|-----------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

*XNOR is true when inputs are the same.*

### Logical Indexing

**What is logical indexing?**

**Logical indexing** uses a logical array (true/false values) to select elements from another array. It is one of the most powerful features in MATLAB for data filtering and extraction.

**Concept:**

Instead of using numeric indices (1, 2, 3, ...), you use a logical array where:
- `true` (or 1) means "include this element"
- `false` (or 0) means "exclude this element"

**Simple Example:**

```matlab
% Temperature measurements from a reactor
T = [45 52 48 55 50 58 49 61];   % °C

% Which measurements are above 50°C?
above_50 = T > 50;               % [false true false true false true false true]

% Extract the high temperatures
high_temps = T(above_50);        % [52 55 58 61]
```

**Syntax Breakdown:**

```matlab
high_temps = T(above_50);
```

- `T(above_50)` – indexing operation
- `T` – the data array
- `( )` – indexing parentheses
- `above_50` – logical array (true where T > 50)
- Result: only elements where `above_50` is true

**Multi-Condition Logical Indexing:**

```matlab
% Reactor data: temperature and pressure
T = [45 52 48 55 50 58 49 61];   % °C
P = [8 12 9 14 10 16 11 18];     % bar

% Safe operating region: 48°C ≤ T ≤ 60°C and P ≤ 15 bar
safe_region = (T >= 48) & (T <= 60) & (P <= 15);
% [false true true true true true false true]

% Extract safe measurements
T_safe = T(safe_region);         % [52 48 55 50 58 49]
P_safe = P(safe_region);         % [12 9 14 10 16 11]
```

**Finding Indices of True Values:**

Sometimes you need the position (index) where a condition is true, not just the values.

```matlab
% Find positions where temperature exceeds 55°C
T = [45 52 48 55 50 58 49 61];

above_55 = T > 55;               % [false false false false false true false true]

% Use find() to get indices
indices = find(above_55);        % [6 8]

% Verify
T(indices);                      % [58 61]
```

**Syntax Breakdown:**

```matlab
indices = find(above_55);
```

- `find()` – function that returns indices
- `above_55` – logical array
- Result: numeric array [6 8] indicating positions

**Chemical Engineering Example — Data Quality Check:**

```matlab
% Experimental data from a reactor
time = [0 10 20 30 40 50 60];           % minutes
conversion = [0 0.12 0.28 0.42 0.55 0.71 0.82];  % fraction
yield = [0.00 0.10 0.26 0.38 0.51 0.68 0.88];    % fraction

% Remove invalid measurements (where yield > conversion, physically impossible)
valid = yield <= conversion;              % Check validity
% [true true true true true true false]  % Last point seems anomalous

% Extract only valid data
time_valid = time(valid);                % [0 10 20 30 40 50]
conversion_valid = conversion(valid);    % [0 0.12 0.28 0.42 0.55 0.71]
yield_valid = yield(valid);              % [0 0.10 0.26 0.38 0.51 0.68]

% Now proceed with analysis using only clean data
fprintf('Removed %d anomalous data points\n', sum(~valid));  % Removed 1 anomalous data points
```

### Common Mistakes

| Mistake                          | Why it is wrong                        | Correct form                     |
|----------------------------------|----------------------------------------|----------------------------------|
| `T = 350` inside a condition     | Assignment, not comparison             | `T == 350`                       |
| `T > 300 & < 400`                | Incomplete second comparison           | `(T > 300) & (T < 400)`          |
| Using `*` instead of `.*`        | Dimension mismatch or wrong math       | `.*` for element-wise            |
| Relying on precedence without () | Hard-to-read, error-prone code         | Explicit parentheses             |
| Using `&&` on arrays             | `&&` requires scalar operands          | Use `&` for arrays               |

---

## Worked Example: Complete Reactor Data Analysis

**Sensor Fault Detection**

```matlab
% Pressure readings bar, valid range 0-10 bar, plus not NaN
P_raw = [2.5 3.0 -1 12.5 NaN 2.8];  % includes faulty -1, 12.5, NaN

isValid = (P_raw >= 0) & (P_raw <= 10) & ~isnan(P_raw);
P_valid = P_raw(isValid)  % [2.5 3.0 2.8]
P_invalid = P_raw(~isValid)

% Flag
if any(~isValid)
    fprintf('Warning: %d invalid readings\n', sum(~isValid));
end
```

---

## Chemical Engineering Application: Process Monitoring System

A chemical plant operates a CSTR for oxidation of organic feedstock. The control system logs temperature, pressure, and inlet flow rate every 30 seconds. You need to write MATLAB code that:

1. **Identifies normal operation**
2. **Flags parameters exceeding safety limits**
3. **Suggests corrective action**

**Process Specifications:**

- Optimal temperature: 80–100°C
- Safe pressure limit: ≤ 20 bar (absolute)
- Safe feed rate: 500–800 L/min

**Implementation:**

```matlab
% Simulated data (15 measurements over 7.5 minutes)
T = [72 78 85 92 95 98 102 105 98 95 88 82 79 76 73];  % °C
P = [5 8 12 15 18 20 22 23 20 17 14 11 9 8 7];          % bar
F = [520 540 580 620 650 680 690 720 700 680 650 600 560 540 520];  % L/min

% Define safe operating limits
T_low = 80;
T_high = 100;
P_limit = 20;
F_low = 500;
F_high = 800;

% Identify normal operation
normal_T = (T >= T_low) & (T <= T_high);
normal_P = P <= P_limit;
normal_F = (F >= F_low) & (F <= F_high);

% All conditions must be met
normal_op = normal_T & normal_P & normal_F;

% Count events
n_startup = sum((~normal_T) & (T < T_low));   % Temperature too low
n_overheat = sum((~normal_T) & (T > T_high));  % Temperature too high
n_pressure = sum(~normal_P);                   % Pressure exceeded
n_feed_low = sum((~normal_F) & (F < F_low));   % Feed rate too low
n_feed_high = sum((~normal_F) & (F > F_high)); % Feed rate too high

fprintf('=== PROCESS MONITORING SUMMARY ===\n');
fprintf('Normal operation: %d / %d measurements (%.1f%%)\n', sum(normal_op), length(normal_op), 100*mean(normal_op));
fprintf('\nIssues Detected:\n');
fprintf('Temperature below optimal (< %d°C): %d events\n', T_low, n_startup);
fprintf('Temperature above optimal (> %d°C): %d events\n', T_high, n_overheat);
fprintf('Pressure exceeded limit (> %d bar): %d events\n', P_limit, n_pressure);
fprintf('Feed rate below minimum (< %d L/min): %d events\n', F_low, n_feed_low);
fprintf('Feed rate above maximum (> %d L/min): %d events\n', F_high, n_feed_high);

% Recommend action
if n_overheat > 5
    fprintf('\n CRITICAL: Persistent overheating detected. Reduce inlet feed rate or increase cooling.\n');
elseif n_overheat > 0
    fprintf('\n WARNING: Temperature spikes detected. Monitor cooling system.\n');
end

if n_pressure > 0
    fprintf('\n CRITICAL: Pressure exceeded safety limit. Reduce throughput.\n');
end

if n_startup > 0
    fprintf('\n INFO: Temperature too low (likely startup phase). Normal if at beginning of run.\n');
end
```

**Output:**

```
=== PROCESS MONITORING SUMMARY ===
Normal operation: 8 / 15 measurements (53.3%)

Issues Detected:
Temperature below optimal (< 80°C): 5 events
Temperature above optimal (> 100°C): 2 events
Pressure exceeded limit (> 20 bar): 2 events
Feed rate below minimum (< 500 L/min): 0 events
Feed rate above maximum (> 800 L/min): 0 events

 WARNING: Temperature spikes detected. Monitor cooling system.

 CRITICAL: Pressure exceeded safety limit. Reduce throughput.

 INFO: Temperature too low (likely startup phase). Normal if at beginning of run.
```

---

## Key MATLAB Syntax Reference

| Purpose | MATLAB Syntax | Example |
|---------|---------------|---------|
| Addition | `+` | `A + B` |
| Subtraction | `-` | `A - B` |
| Matrix multiplication | `*` | `A * B` |
| Element-wise multiplication | `.*` | `A .* B` |
| Matrix division | `/` | `A / B` |
| Element-wise division | `./` | `A ./ B` |
| Matrix exponentiation | `^` | `A ^ 2` |
| Element-wise exponentiation | `.^` | `A .^ 2` |
| Equal to | `==` | `x == 5` |
| Not equal to | `~=` | `x ~= 5` |
| Less than | `<` | `x < 10` |
| Greater than | `>` | `x > 0` |
| Less than or equal | `<=` | `x <= 5` |
| Greater than or equal | `>=` | `x >= 0` |
| Logical AND | `&` | `(x > 0) & (x < 10)` |
| Logical OR | `\|` | `(x < 0) \| (x > 10)` |
| Logical NOT | `~` | `~(x == 5)` |
| Logical XOR | `xor()` | `xor(A, B)` |
| Logical XNOR | `~xor()` | `~xor(A, B)` |
| Logical indexing | `A(logical_array)` | `A(A > 5)` |
| Find indices | `find()` | `find(A > 5)` |
| Mean | `mean()` | `mean(A)` |
| Count elements | `sum()` | `sum(condition)` |

---

## Homework

### Problem

A continuous reactor is monitored by four sensors. The following vectors contain one hour of measurements (sampled every 10 minutes, i.e., 7 points each):

```matlab
T = [442 448 455 461 458 452 445];          % temperature, K
P = [3.8  3.9  4.1  4.3  4.2  4.0  3.9];   % pressure, bar
F = [95  98  102 105 103 100  97];          % feed flow, mol/s
CA = [0.85 0.82 0.78 0.75 0.76 0.79 0.83]; % outlet concentration of A, mol/L
```

**Safety and quality limits:**

- Temperature must stay between 440 K and 460 K (inclusive).
- Pressure must stay ≤ 4.2 bar.
- Flow must stay ≥ 96 mol/s.
- Outlet concentration of A must stay ≤ 0.80 mol/L.

**Required Tasks**

1. Create a logical vector `safeT` that is true when temperature is inside its limits.
2. Create analogous logical vectors for pressure, flow, and concentration.
3. Create a combined logical vector `allSafe` that is true only when **all four** conditions are satisfied simultaneously.
4. Determine how many sampling instants are fully safe and list their indices.
5. Compute the average temperature of the safe points only.
6. (Optional) Create a logical vector that is true when **any** of the four limits is violated (i.e., the alarm condition).

### Concepts Being Tested

- Relational operators
- Combining multiple conditions with `&` and `|`
- Logical indexing and the functions `sum` / `find`
- Vectorized calculation of a conditional average

### Hints

- Temperature window: `(T >= 440) & (T <= 460)`
- Combined safety: `safeT & safeP & safeF & safeCA`
- Average of selected elements: `mean(T(allSafe))`

<details>
<summary><strong>Detailed Solution — Click to Reveal</strong></summary>

### Concept

The exercise trains the construction of multiple relational tests, their combination with logical AND, and the subsequent use of the resulting mask for counting, indexing, and conditional statistics—core skills for process-data screening.

### Approach / Idea

1. Write a clear relational expression for each individual constraint.
2. Combine the four logical vectors with `&`.
3. Use `sum` and `find` on the combined mask.
4. Apply the mask to the temperature vector to obtain the conditional average.
5. (Optional) The alarm vector is simply the negation of `allSafe` or the OR of the individual violation flags.

### Syntax

```matlab
safeT = (T >= 440) & (T <= 460);
allSafe = safeT & safeP & safeF & safeCA;
nSafe = sum(allSafe);
idx = find(allSafe);
T_avg_safe = mean(T(allSafe));
```

### Syntax Breakdown

- Each relational comparison produces a logical array of the same size as the data.
- `&` performs element-wise conjunction.
- `sum` on a logical array counts the number of `true` values.
- `find` returns the numeric indices where the condition is true.
- Logical indexing `T(allSafe)` extracts only the safe temperatures.

### MATLAB Code

```matlab
% Sensor data (7 samples)
T  = [442 448 455 461 458 452 445];          % K
P  = [3.8  3.9  4.1  4.3  4.2  4.0  3.9];   % bar
F  = [95  98  102 105 103 100  97];          % mol/s
CA = [0.85 0.82 0.78 0.75 0.76 0.79 0.83];  % mol/L

% Individual safety masks
safeT  = (T  >= 440) & (T  <= 460);
safeP  = (P  <= 4.2);
safeF  = (F  >= 96);
safeCA = (CA <= 0.80);

% All constraints satisfied
allSafe = safeT & safeP & safeF & safeCA;

% Results
nSafe = sum(allSafe);
idxSafe = find(allSafe);
T_avg_safe = mean(T(allSafe));

fprintf('Number of fully safe samples: %d\n', nSafe);
fprintf('Indices of safe samples: ');
disp(idxSafe);
fprintf('Average temperature of safe samples: %.1f K\n', T_avg_safe);

% Optional alarm (any violation)
alarm = ~allSafe;
fprintf('Alarm at samples: ');
disp(find(alarm));
```

### Line-by-Line Explanation

- Four logical vectors are created, each expressing one engineering limit.
- `allSafe` is true only at the sampling instants that satisfy every limit.
- `sum` and `find` give the count and the locations of the safe points.
- Logical indexing extracts the corresponding temperatures for the average.
- The alarm vector is the logical complement of `allSafe`.

### Expected Result

```
Number of fully safe samples: 3
Indices of safe samples:      3     5     6

Average temperature of safe samples: 455.0 K
Alarm at samples:      1     2     4     7
```

</details>

---