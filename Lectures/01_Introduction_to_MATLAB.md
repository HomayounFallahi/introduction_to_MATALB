# Introduction to MATLAB

## Learning Objectives

By the end of this lecture, you should be able to:

- Navigate the MATLAB Desktop and understand the role of key interface components (Command Window, Workspace, Editor, Current Folder)
- Understand MATLAB's role and applications in chemical engineering
- Perform basic arithmetic calculations in MATLAB
- Create and assign values to variables
- Use built-in constants and functions
- Create and run MATLAB scripts
- Save and manage MATLAB programs
- Access help and documentation effectively
- Create, save, and run a script (`.m` file) and a Live Script (`.mlx`)
- Use `help`, `doc`, and `lookfor` to find documentation and function usage

---

## Why This Topic Matters

MATLAB (Matrix Laboratory) is one of the most widely used computational platforms in chemical engineering. Whether you're solving material balances, designing reactors, analyzing experimental data, or simulating processes, MATLAB provides a powerful environment for engineering calculations.

### Applications in Chemical Engineering

- **Process Modeling and Simulation**: MATLAB is extensively used to model reactor systems, distillation columns, and other unit operations
- **Data Analysis**: Process data from sensors, experiments, and simulations can be analyzed and visualized
- **Numerical Solutions**: Complex engineering equations are solved numerically
- **Control System Design**: PID controllers and advanced control strategies are developed and tested
- **Optimization**: Process conditions and designs are optimized for efficiency and cost
- **Visualization**: Engineering results are communicated through plots and graphics

Throughout this course, you'll develop MATLAB skills applied directly to chemical engineering problems. This first lecture introduces the MATLAB interface and basic operation.

---

## MATLAB Environment

### What is MATLAB?

MATLAB is an interactive computational environment designed for numerical computing, data analysis, visualization, and programming. It is particularly well-suited for engineers because it handles matrix operations naturally and provides extensive libraries (called toolboxes) for specialized applications.

### The MATLAB Desktop

When you open MATLAB, you see the **MATLAB Desktop**, which is the main working environment. The desktop contains several key windows:

#### 1. **Command Window**

The Command Window is where you type commands directly.

- MATLAB displays a **prompt** (`>>`), indicating readiness for input
- You type a command and press Enter to execute it
- The result appears immediately below the command
- This is useful for quick calculations and testing

Example of a simple calculation in the Command Window:

```
>> 2 + 3
ans = 5
```

#### 2. **Workspace**

The Workspace displays all variables you have created during the current session.

- Each variable is shown with its name, value, and data type
- You can modify variables by double-clicking them
- It acts as your "memory" for the current session
- When you close MATLAB, unsaved variables are lost

Useful commands:

```matlab
who      % list variable names
whos     % detailed list
clear T  % remove only T
```
- To clear the workspace: `clear` or `clearvars`.
- To clear the Command Window: `clc`.
- To close all figure windows: `close all`.

**Common mistake:** Forgetting `clear` and using an old value of `T` from a previous calculation. Always check Workspace or use `clear` at start of a script.

#### 3. **Current Folder**

The Current Folder window shows the files and folders in your working directory.

- MATLAB can only directly access files in the current folder (or folders on your path)
- You can navigate to different folders by double-clicking or using the navigation bar
- When you save MATLAB files (scripts), they should be in the current folder

#### 4. **Editor**

The Editor is a text editor designed specifically for writing MATLAB code.

- You can open the Editor by clicking the **New Script** button or typing `edit` in the Command Window
- The Editor provides syntax highlighting (keywords, comments, strings appear in different colors)
- It allows you to write programs (scripts and functions) line by line
- You can save your work as `.m` files (MATLAB files)

#### 5. **Command History**

The Command History window shows all commands you've typed previously.

- This is useful for recalling what you've done
- You can double-click a previous command to run it again
- You can drag a command from history to the Editor to reuse it

### **Help and Documentation**

MATLAB has excellent built-in help. Three essential commands:

```matlab
help sqrt        % short text help in Command Window
doc sqrt         % full documentation with examples (opens browser)
doc              % open the documentation home page
lookfor sqrt  % search all help for keyword "sqrt"
```

**Example walkthrough:**

```matlab
>> help exp
 exp    Exponential.
    exp(X) is the exponential of the elements of X, e^x.

>> doc exp  % opens detailed doc page
```

**Pro tip:** Press `F1` with cursor on a function name to open its doc. Use `docsearch` for engineering topics.

### MATLAB Online

In addition to the desktop version, MATLAB is available **online** in your web browser.

- MATLAB Online provides the same core functionality
- It is useful for accessing MATLAB without installing software
- Your files are stored in the cloud
- The interface is similar to the desktop version

For this course, either MATLAB Desktop or MATLAB Online is acceptable. If you're using MATLAB Online, the interface is slightly reorganized but contains the same components.

### Common Mistakes — MATLAB Environment

- Running code while Current Folder is wrong → `File not found` errors. Always check path.
- Typing in Editor but forgetting to save and run → old version executes.
- Over-relying on `ans` → it gets overwritten. Always assign to meaningful names like `Reynolds` not `ans`.

---

## Getting Started

### Basic Calculations

One of the simplest ways to use MATLAB is as a calculator. Let's perform some basic arithmetic operations.

#### Concept

MATLAB evaluates mathematical expressions using standard operators and follows the usual order of operations (PEMDAS/BODMAS).

#### MATLAB Syntax

```matlab
result = 5 + 3;
result = 10 - 4;
result = 6 * 7;
result = 20 / 4;
result = 2 ^ 3;
```

#### Syntax Breakdown

- `result` — variable name that will store the result
- `=` — assignment operator (assigns the value on the right to the variable on the left)
- `+`, `-`, `*`, `/` — basic arithmetic operators
- `^` — exponentiation (power) operator
- `;` — semicolon suppresses the output display (the calculation still happens, but the result is not shown in the Command Window)

#### Simple Example

Let's calculate the boiling point of water at sea level and at altitude.

```matlab
% At sea level (1 atm), water boils at 100°C
boiling_point_sea = 100;

% At high altitude (0.5 atm), water boils at a lower temperature
altitude_reduction = 15;
boiling_point_altitude = boiling_point_sea - altitude_reduction;
```

After entering these lines in the Command Window (or in the Editor and running), the variable `boiling_point_altitude` now stores the value **85**.

#### Example Walkthrough

1. We create a variable `boiling_point_sea` and assign it the value 100 (in °C)
2. We create a variable `altitude_reduction` and assign it the value 15 (the temperature drop in °C)
3. We perform subtraction: `boiling_point_altitude = 100 - 15 = 85`
4. The result is stored in the variable `boiling_point_altitude`

#### Chemical Engineering Example

In process engineering, we often calculate property changes. For example, the **density of water changes with temperature**:

```matlab
% Density of water at 20°C (reference)
rho_20 = 998.2;   % kg/m³

% Density of water at 60°C (approximate value)
rho_60 = 983.2;   % kg/m³

% Calculate the density difference
delta_rho = rho_20 - rho_60;

% Calculate the percentage change
percent_change = (delta_rho / rho_20) * 100;
```

In this example:
- We store the density at two temperatures
- We calculate the absolute change in density
- We calculate the percentage change
- Result: `delta_rho = 15`, `percent_change ≈ 1.5%`

This type of calculation is common when designing processes that must account for temperature-dependent properties.

#### Common Mistakes

**Mistake 1: Forgetting the semicolon**

```matlab
x = 5 + 3    % Without semicolon, MATLAB displays: x = 8
x = 5 + 3;   % With semicolon, the result is stored but not displayed
```

Why semicolon matters: Without `;`, MATLAB echoes every assignment — useful for debugging, noisy for scripts. Use `;` to keep Command Window clean, remove it when you want to inspect.

**Mistake 2: Confusing order of operations**

```matlab
result = 2 + 3 * 4;   % This gives 14, not 20
                       % Multiplication happens before addition
```

If you want a different order, use parentheses:

```matlab
result = (2 + 3) * 4;  % This gives 20
```

#### Key Takeaways

- MATLAB can perform arithmetic operations like a calculator
- Use `=` to assign values to variables
- Use `;` to suppress output display (optional but recommended)
- MATLAB follows standard mathematical order of operations

**Walkthrough tip:** Use parentheses for clarity, especially in engineering formulas.

---

## Variables and Assignment

### Concept

A **variable** is a named container that stores a value (a number, text, or more complex data). Once you assign a value to a variable, you can use that variable name in subsequent calculations.

### Explanation

In MATLAB, variables are created the moment you assign a value to them. Unlike some programming languages, you do not need to declare a variable before using it.

**Naming Rules for Variables**

Variable names in MATLAB must follow these rules:

1. Start with a letter (A–Z or a–z)
2. Can contain letters, digits (0–9), and underscores (_)
3. Are case-sensitive (`T` and `t` are different variables)
4. Cannot contain spaces or special characters (except underscore)
5. Cannot be MATLAB keywords (e.g., `for`, `if`, `while`)

### Good Variable Names

Use descriptive names that indicate what the variable represents:

```matlab
temperature = 350        % Preferred: clear meaning
T = 350                  % Acceptable: common abbreviation
t = 350                  % Confusing: unclear what t represents
x1 = 350                 % Poor: no indication of purpose
```


**Chemical Engineering Tip:** In engineering, use variable names that match your problem statement. If your material balance involves inlet flow rate, feed composition, and outlet temperature, use:

```matlab
F_in = 100      % mol/s, inlet flow rate
x_feed = 0.25   % mol fraction, feed composition
T_out = 325     % K, outlet temperature
```

This makes your code self-documenting and easier to verify against your hand calculations.

### MATLAB Syntax

```matlab
variable_name = value;
```

### Syntax Breakdown

- `variable_name` — the name you choose for the variable (must follow naming rules)
- `=` — the assignment operator
- `value` — a number, calculation, or other data
- `;` — suppresses output (optional)

### Simple Example

Let's calculate the volume of a cylindrical reactor using dimensional analysis.

```matlab
% Define reactor dimensions
diameter = 2.0;      % meters
height = 5.0;        % meters

% Calculate radius
radius = diameter / 2;

% Calculate volume using V = π * r² * h
pi_value = 3.14159;
volume = pi_value * radius^2 * height;
```

After running this code:
- `diameter = 2.0`
- `radius = 1.0`
- `volume ≈ 15.71` (cubic meters)

### Example Walkthrough

1. We assign the diameter value to the variable `diameter`
2. We assign the height value to the variable `height`
3. We calculate the radius and store it in `radius`
4. We use the formula for cylinder volume and store the result in `volume`
5. Now we can use `volume` in other calculations

### Chemical Engineering Example

In a **distillation column design**, we often need to track inlet and outlet conditions:

```matlab
% Inlet stream conditions
T_inlet = 300;       % K
P_inlet = 1.013;     % bar
composition_inlet = 0.40;  % mole fraction of light component

% Outlet stream conditions
T_outlet = 320;      % K
P_outlet = 1.000;    % bar
composition_outlet = 0.95;  % mole fraction of light component

% Calculate the temperature change across the column
delta_T = T_outlet - T_inlet;

% Calculate enrichment (improvement in composition)
enrichment = composition_outlet - composition_inlet;
```

Result:
- `delta_T = 20` K
- `enrichment = 0.55` (the light component was enriched by 55 percentage points)

### Common Mistakes

**Mistake 1: Using a variable before assigning it**

```matlab
result = x + 5;  % Error! x doesn't exist yet
x = 3;           % Now x is defined (too late)
```

MATLAB will display an error because `x` was not defined before being used.

**Mistake 2: Accidentally overwriting a variable**

```matlab
T = 300;  % Temperature at inlet
T = 350;  % Oops! Now the inlet temperature is lost
```

Be careful when reusing variable names. If you need multiple values, use different names.

**Mistake 3: Case sensitivity**

```matlab
temperature = 300;
result = Temperature + 50;  % Error! MATLAB won't find 'Temperature'
                             % (only 'temperature' exists)
```

#### Key Takeaways

- Variables store values and make your code more readable
- Use meaningful variable names that describe what the variable represents
- Variables are case-sensitive
- Assign values using the `=` operator

---

## Constants and Built-in Functions

### Concept

MATLAB provides **constants** (predefined values) and **built-in functions** (predefined operations) that make calculations easier and more accurate.

### Important MATLAB Constants

#### π (pi)

```matlab
result = pi * r^2;  % MATLAB knows the value of pi
```

`pi` in MATLAB is approximately 3.14159265358979. You don't need to type it manually.

#### e (exp constant)

```matlab
result = exp(1);    % e ≈ 2.71828
```

#### Infinity and Not-a-Number

```matlab
inf       % Positive infinity
-inf      % Negative infinity
nan       % Not-a-Number (represents undefined or missing data)
```

### Common MATLAB Functions

Functions in MATLAB have the syntax:

```matlab
output = function_name(input);
```

#### Example: Square Root

```matlab
result = sqrt(16);   % result = 4
```

Here:
- `sqrt` is the function name
- `16` is the input (argument)
- `result` receives the output

#### Trigonometric Functions

```matlab
sin_value = sin(pi/2);   % sine: returns 1
cos_value = cos(0);      % cosine: returns 1
tan_value = tan(pi/4);   % tangent: returns 1
```

Note: These functions expect input in **radians**, not degrees.
```
deg2rad(90)       % or use 90*pi/180
```

#### Exponential and Logarithmic Functions

```matlab
exp_value = exp(1);      % e^1 = 2.71828
log_value = log(10);     % Natural logarithm (base e)
log10_value = log10(100);% Logarithm base 10
```

#### Rounding Functions

```matlab
round_value = round(3.7);    % rounds to nearest integer: 4
floor_value = floor(3.7);    % rounds down: 3
ceil_value = ceil(3.2);      % rounds up: 4
```

#### Absolute Value

```matlab
abs_value = abs(-25);    % returns 25
```

Built-in functions cover engineering needs:

| Category | Functions |
|---|---|
| Exponential/Log | `exp`, `log`, `log10`, `sqrt` |
| Trigonometry | `sin`, `cos`, `tan`, `asin`, `acos` |
| Rounding | `round`, `ceil`, `floor`, `fix` |
| Complex | `abs`, `angle`, `real`, `imag` |

### Simple Example

Let's calculate the reaction rate using the Arrhenius equation:

$$\text{k} = A \cdot e^{-E_a / RT}$$

Where:
- k = reaction rate constant
- A = pre-exponential factor
- E_a = activation energy (J/mol)
- R = gas constant (8.314 J/mol·K)
- T = temperature (K)

```matlab
% Arrhenius equation calculation
A = 1e6;           % pre-exponential factor (1/s)
E_a = 50000;       % activation energy (J/mol)
R = 8.314;         % gas constant (J/mol·K)
T = 350;           % temperature (K)

% Calculate exponent
exponent = -E_a / (R * T);

% Calculate rate constant
k = A * exp(exponent);
```

After running:
- `exponent ≈ -17.18`
- `k ≈ 0.0345` (very small, because the exponent is very negative)

This shows that at 350 K, the reaction rate constant is essentially zero—the temperature is too low for the reaction to proceed at a significant rate.

### Example Walkthrough

1. We define the Arrhenius parameters (A, E_a, R, T)
2. We calculate the exponent in the exponential function
3. We use `exp()` to calculate e raised to that exponent
4. We multiply by A to get the final rate constant

### Chemical Engineering Example

**Vapor Pressure Calculation (Antoine Equation)**

The vapor pressure of a liquid can be estimated using:

$$\log_{10}(P) = A - \frac{B}{C + T}$$

Where P is in bar, T is in °C, and A, B, C are substance-specific constants.

For water:
- A = 4.40389
- B = 1738.86
- C = 233.06

```matlab
% Antoine equation for water vapor pressure
A = 5.08354;
B = 1663.125;
C = -45.622;
T = 100;           % Temperature in °C

% Calculate log10(P)
log10_P = A - B / (C + (T + 273.15));

% Calculate P (must convert from log10)
P_vapor = 10^log10_P;  % P in bar
```

Result at 100°C:
- `P_vapor ≈ 1` bar

This makes sense: at 100°C (atmospheric pressure), water boils, so its vapor pressure should be close to 1 atm (≈ 1.01325 bar). The calculation is approximate due to the Antoine equation's empirical nature.

### Common Mistakes

**Mistake 1: Using degrees instead of radians for trigonometric functions**

```matlab
result = sin(90);       % Returns -0.44 (not 1!)
result = sin(pi/2);     % Correct: returns 1
```

MATLAB's trig functions always use radians. If your input is in degrees, convert it first:

```matlab
angle_degrees = 90;
angle_radians = angle_degrees * pi / 180;
result = sin(angle_radians);  % Now returns 1
```

**Mistake 2: Confusing log and log10**

```matlab
result = log(100);      % Natural logarithm: ≈ 4.605
result = log10(100);    % Base-10 logarithm: 2
```

**Mistake 3: Forgetting parentheses in function calls**

```matlab
result = sqrt 16;       % Error! Syntax error
result = sqrt(16);      % Correct
```

#### Key Takeaways

- MATLAB has built-in constants like `pi` and `exp(1)`
- Functions are called with parentheses: `function_name(input)`
- Trigonometric functions use radians
- Common functions include `sqrt()`, `sin()`, `cos()`, `exp()`, `log()`, `round()`, etc.

---

## Scripts and Live Scripts

### Concept

So far, we've entered commands one at a time in the Command Window. For more complex calculations, it's better to write a **script**—a file containing a sequence of MATLAB commands that are executed in order.

### What is a Script?

A **script** is a text file containing MATLAB commands. The file has a `.m` extension (for example, `reactor_design.m`).

**Benefits of Scripts:**

1. **Reusability**: Write the code once, run it many times
2. **Debugging**: Easier to find and fix mistakes in multiple lines
3. **Documentation**: Add comments to explain what the code does
4. **Sharing**: Send the file to colleagues
5. **Version Control**: Keep track of changes over time

### Creating a Script

#### Step 1: Open the Editor

Click the **New Script** button in the toolbar, or type in the Command Window:

```matlab
edit
```

#### Step 2: Write Your Code

Type your MATLAB commands into the editor. For example:

```matlab
% Reactor Volume Calculation
% This script calculates the volume of a cylindrical reactor

% Define dimensions
diameter = 2.5;    % meters
height = 8.0;      % meters

% Calculate radius
radius = diameter / 2;

% Calculate volume
volume = pi * radius^2 * height;

% Display result
disp('Reactor volume:')
disp(volume)
disp('m³')
```

#### Step 3: Save the File

Click **Save** (or press Ctrl+S). Choose a meaningful filename like `reactor_volume.m`.

**Important**: Save the file in your Current Folder so MATLAB can find it.

#### Step 4: Run the Script

In the Command Window, type the filename without the `.m` extension:

```matlab
reactor_volume
```

MATLAB will execute all commands in the script in order.

### MATLAB Syntax for Scripts

#### Comments

Use `%` to write comments. Everything after `%` on that line is ignored by MATLAB.

```matlab
% This is a comment
x = 5;  % This line defines x and has a comment at the end
```

Comments are essential for making your code understandable to yourself and others.

#### Displaying Output

Use `disp()` to display results in the Command Window:

```matlab
disp('The volume is:')
disp(volume)
```

Or use `fprintf()` for formatted output (covered in a later lecture):

```matlab
fprintf('Temperature: %f °C\n', temperature)
```

## **fprintf**
In MATLAB, **`fprintf`** is used to display formatted text and values in the Command Window, or to write formatted data to a file.

### Basic syntax

```matlab
fprintf('Hello World\n')
```

Output:

```text
Hello World
```

`'\n'` means **new line**.

### Printing variables

```matlab
x = 25;
fprintf('The value of x is %d\n', x)
```

Output:

```text
The value of x is 25
```

Common format specifiers:

| Specifier | Meaning                 | Example    |
| --------- | ----------------------- | ---------- |
| `%d`      | Integer                 | `25`       |
| `%f`      | Floating-point number   | `3.141593` |
| `%.2f`    | Float with 2 decimals   | `3.14`     |
| `%e`      | Scientific notation     | `3.14e+00` |
| `%s`      | String/character vector | `Hello`    |
| `\n`      | New line                | —          |
| `\t`      | Tab                     | —          |

### Controlling decimal places

```matlab
T = 25.67891;
fprintf('Temperature = %.2f °C\n', T)
```

Output:

```text
Temperature = 25.68 °C
```

### Multiple variables

```matlab
T = 350;
P = 5.25;

fprintf('Temperature = %d K, Pressure = %.2f bar\n', T, P)
```

Output:

```text
Temperature = 350 K, Pressure = 5.25 bar
```

**Key idea:** `fprintf` uses a **format string** followed by the values that should replace the format specifiers.

For example:

```matlab
fprintf('x = %.3f, y = %d, name = %s\n', x, y, name)
```

means: print `x` as a 3-decimal number, `y` as an integer, and `name` as text.

---

### Simple Example: Material Balance Script

A **material balance** is a fundamental concept in chemical engineering. Let's write a script to perform a simple material balance on a mixing process.

```matlab
% Simple Material Balance for Mixing
% Calculate outlet concentration of a mixing process

% Stream 1: Pure component A
flow_1 = 100;      % kg/s
concentration_1 = 1.0;  % 100% pure

% Stream 2: Dilute solution
flow_2 = 50;       % kg/s
concentration_2 = 0.2;  % 20% A

% Calculate total flow (conservation of mass)
flow_total = flow_1 + flow_2;

% Calculate mass of component A in outlet
mass_A_outlet = flow_1 * concentration_1 + flow_2 * concentration_2;

% Calculate outlet concentration
concentration_outlet = mass_A_outlet / flow_total;

% Display results
disp('Material Balance Results:')
disp('------------------------')
fprintf('Stream 1 flow: %f kg/s\n', flow_1)
fprintf('Stream 2 flow: %f kg/s\n', flow_2)
fprintf('Total outlet flow: %f kg/s\n', flow_total)
fprintf('Outlet concentration of A: %f\n', concentration_outlet)
```

**Output:**
```
Material Balance Results:
------------------------
Stream 1 flow: 100.000000 kg/s
Stream 2 flow: 50.000000 kg/s
Total outlet flow: 150.000000 kg/s
Outlet concentration of A: 0.7333
```

### Live Scripts (Optional)

MATLAB also offers **Live Scripts**, which combine code, output, and formatted text in a single document. Live Scripts have a `.mlx` extension.

**Benefits of Live Scripts:**
- Results appear inline with the code
- You can format text with headers, bold, italics, etc.
- Useful for teaching, documentation, and reports
- Can include images and equations

For this course, regular scripts (`.m` files) are sufficient, but feel free to explore Live Scripts if interested.

### Saving and Running MATLAB Programs

#### Saving

1. Click **Save** or press Ctrl+S
2. Choose a meaningful filename (e.g., `heat_balance.m`)
3. Save in your Current Folder
4. The `.m` extension is added automatically

#### Running

In the Command Window, type the filename without the extension:

```matlab
heat_balance
```

#### Accessing Your Script Files

Use the **Current Folder** panel to navigate and manage your files:
- Double-click a `.m` file to open it in the Editor
- Right-click to rename, delete, or copy files
- Ensure your working scripts are in the Current Folder

### Common Mistakes

**Mistake 1: Saving in the wrong folder**

```
% You save the file in your Documents folder,
% but MATLAB's Current Folder is Desktop
% Running the script will fail with: "Undefined function or variable"
```

Always check the Current Folder and save scripts there.

**Mistake 2: Forgetting to save after editing**

If you make changes to a script but don't save, MATLAB will run the **old version**. Always save before running.

**Mistake 3: Not commenting your code**

```matlab
% Bad: No comments
x = 100;
y = 50;
z = x + y;

% Good: Clear comments
% Initial feed temperature (K)
T_inlet = 100;
% Outlet temperature (K)
T_outlet = 50;
% Temperature change
delta_T = T_inlet - T_outlet;
```

Comments make code much easier to understand and debug.

#### Key Takeaways

- Scripts are files (`.m`) containing multiple MATLAB commands
- Create scripts using the Editor
- Save scripts in your Current Folder
- Run a script by typing its filename in the Command Window
- Add comments with `%` to explain your code
- Use `disp()` and `fprintf()` to display results

---

#### 1. Documentation Browser

Type the **Help** in command window and select **Documentation**, or press F1. This opens a searchable help browser with detailed explanations, examples, and links to related functions.

#### 2. Function Hints

Start typing a function name in the Editor. MATLAB displays a tooltip showing the function syntax and arguments:

```matlab
plot(
```

A hint appears showing the syntax of the `plot` function.

#### 3. Web Resources

MathWorks (the company behind MATLAB) provides extensive online documentation at:

```
https://www.mathworks.com/help/matlab/
```

### Reading Help Output

When you type `help function_name`, MATLAB displays:

1. **Function Name and Syntax** — How to call the function
2. **Description** — What the function does
3. **Example** — Sample usage
4. **Related Functions** — Other useful functions

---

## Summary

### Main Concepts

1. **MATLAB Environment**: The MATLAB Desktop contains the Command Window, Workspace, Editor, Current Folder, and Command History
2. **Basic Calculations**: MATLAB can perform arithmetic operations using standard operators
3. **Variables**: Named containers that store values; use meaningful names
4. **Constants and Functions**: MATLAB provides built-in constants (pi, e) and functions (sqrt, sin, exp, log, etc.)
5. **Scripts**: Text files (`.m`) containing sequences of commands; saved and reused
6. **Comments and Documentation**: Use `%` to add comments; use `disp()` to display results

### Important MATLAB Operations

| Purpose | Syntax | Example |
|---|---|---|
| Addition | `a + b` | `5 + 3` |
| Subtraction | `a - b` | `10 - 4` |
| Multiplication | `a * b` | `6 * 7` |
| Division | `a / b` | `20 / 4` |
| Exponentiation | `a ^ b` | `2 ^ 3` (gives 8) |
| Assign variable | `var = value` | `T = 300` |
| Square root | `sqrt(x)` | `sqrt(16)` |
| Sine (radians) | `sin(x)` | `sin(pi/2)` |
| Exponential | `exp(x)` | `exp(1)` |
| Natural logarithm | `log(x)` | `log(10)` |
| Display output | `disp(x)` | `disp('Result:')` |

### Important Constants

| Constant | MATLAB Syntax | Approximate Value |
|---|---|---|
| Pi (π) | `pi` | 3.14159 |
| e | `exp(1)` | 2.71828 |
| Infinity | `inf` | ∞ |

### Key MATLAB Syntax Reference

| Purpose | MATLAB Syntax |
|---|---|
| Basic arithmetic | `result = 5 + 3; result = a * b;` |
| Assignment | `variable = value;` |
| Comment | `% This is a comment` |
| Square root | `sqrt(16)` |
| Power | `2^3` (gives 8) |
| Absolute value | `abs(-5)` |
| Sine (input in radians) | `sin(pi/2)` |
| Cosine | `cos(0)` |
| Exponential | `exp(x)` |
| Natural log | `log(x)` |
| Base-10 log | `log10(x)` |
| Round to nearest integer | `round(3.7)` |
| Round down | `floor(3.7)` |
| Round up | `ceil(3.2)` |
| Display text or value | `disp(variable)` or `disp('Text')` |

---

## Worked Example: Heat Balance on a Reactor

Let's combine the concepts from this lecture to solve a realistic chemical engineering problem.

### Problem Statement

A reactor receives a hot inlet stream at 400 K with a mass flow rate of 250 kg/s. Heat is removed from the reactor, and the outlet stream exits at 350 K. Calculate the heat duty (rate of heat removal) required.

**Given:**
- Inlet temperature: T_in = 400 K
- Outlet temperature: T_out = 350 K
- Mass flow rate: ṁ = 250 kg/s
- Specific heat capacity: C_p = 3.5 kJ/kg·K

**Find:** Heat duty Q (kW)

**Formula:** Q = ṁ × C_p × ΔT

### Solution Script

```matlab
% Heat Balance on a Reactor
% Calculates the rate of heat removal from a cooling process

% Define known values
T_inlet = 400;      % K
T_outlet = 350;     % K
mass_flow = 250;    % kg/s
C_p = 3.5;          % kJ/kg·K

% Calculate temperature change
delta_T = T_inlet - T_outlet;

% Calculate heat duty (rate of heat removal)
Q = mass_flow * C_p * delta_T;

% Display results
disp('========================================')
disp('Heat Balance Calculation')
disp('========================================')
fprintf('Inlet temperature:  %f K\n', T_inlet)
fprintf('Outlet temperature: %f K\n', T_outlet)
fprintf('Temperature change: %f K\n', delta_T)
fprintf('Mass flow rate:     %f kg/s\n', mass_flow)
fprintf('Specific heat:      %f kJ/kg·K\n', C_p)
fprintf('========================================\n')
fprintf('Heat duty (Q):      %f kW\n', Q)
fprintf('========================================\n')
```

### Explanation

1. **Define Variables**: We assign the inlet temperature, outlet temperature, mass flow rate, and specific heat capacity to variables
2. **Calculate ΔT**: We find the temperature difference between inlet and outlet
3. **Calculate Q**: We multiply mass flow × specific heat × temperature change
4. **Display Results**: We use `fprintf()` to display a formatted output table

### Expected Output

```
========================================
Heat Balance Calculation
========================================
Inlet temperature:  400.000000 K
Outlet temperature: 350.000000 K
Temperature change: 50.000000 K
Mass flow rate:     250.000000 kg/s
Specific heat:      3.500000 kJ/kg·K
========================================
Heat duty (Q):      43750.000000 kW
========================================
```

### Engineering Interpretation

The heat duty Q = 43,750 kW (or ≈ 43.75 MW) means:

- **43,750 kilowatts of thermal energy must be removed** from the reactor every second to cool the stream from 400 K to 350 K
- This is a large amount of cooling duty, typical for industrial-scale reactors
- In practice, this cooling would be achieved using a cooling jacket, a heat exchanger, or another cooling system
- The negative sign (heat removal, not addition) is implicitly understood from the temperature drop

This calculation is a fundamental step in reactor design and process engineering. Engineers use such calculations to size heat exchangers, determine utility requirements, and estimate operating costs.

---

## Homework

### Problem

A process streams carries a liquid mixture from a reactor to a cooler. The mixture has:

- **Inlet flow rate**: 500 kg/s
- **Inlet temperature**: 325 K
- **Outlet temperature**: 300 K
- **Specific heat capacity of the mixture**: 2.8 kJ/kg·K

Using MATLAB, calculate:

1. The mass flow rate of the outlet stream (assuming no accumulation in the cooler)
2. The temperature change across the cooler
3. The heat removal duty (in kW)

Then, discuss the engineering significance of your result.

### Required Tasks

1. Create a MATLAB script that performs the calculations
2. Define variables for inlet flow, temperatures, and specific heat
3. Calculate the outlet flow rate (conservation of mass)
4. Calculate the temperature change
5. Calculate the heat duty
6. Display results using `disp()` and `fprintf()`
7. Save the script as `cooler_heat_balance.m`

### Concepts Being Tested

- Variable assignment
- Arithmetic operations (subtraction, multiplication)
- Built-in functions if needed
- Script creation and execution
- Output formatting with `fprintf()`
- Application to a real chemical engineering scenario

### Hints

- At steady state, mass in = mass out (conservation of mass)
- ΔT = T_inlet - T_outlet
- Use the formula Q = ṁ × C_p × ΔT
- Pay attention to units: if C_p is in kJ/kg·K and flow is in kg/s, Q will be in kW

<details>
<summary><strong>Detailed Solution — Click to Reveal</strong></summary>

## Solution

### Concept

This problem applies **mass and energy conservation** to a cooling process. At steady state:

1. **Mass Balance**: The mass flow rate entering the cooler equals the mass flow rate leaving (no accumulation)
2. **Energy Balance**: The heat removed equals the enthalpy change of the stream: Q = ṁ × C_p × ΔT

These are fundamental principles in chemical engineering and are applied in the design of every process unit.

### Idea/Approach

1. Recognize that at steady state, inlet flow = outlet flow
2. Calculate the temperature drop across the cooler
3. Apply the heat balance equation to find the duty
4. Use a MATLAB script to organize the calculation and display results clearly

### MATLAB Code

```matlab
% Heat Balance on a Cooler
% Calculates heat removal duty for a cooling process
% Script: cooler_heat_balance.m

% ========== Define Variables ==========
% Inlet stream properties
F_inlet = 500;        % kg/s
T_inlet = 325;        % K
T_outlet = 300;       % K
C_p = 2.8;            % kJ/kg·K

% ========== Conservation of Mass ==========
% At steady state, mass in = mass out
F_outlet = F_inlet;

% ========== Calculate Temperature Change ==========
delta_T = T_inlet - T_outlet;

% ========== Calculate Heat Duty ==========
% Q = mass_flow * specific_heat * delta_T
Q = F_outlet * C_p * delta_T;

% ========== Display Results ==========
disp(' ')
disp('=== COOLER HEAT BALANCE ===')
disp(' ')
disp('Input Data:')
fprintf('  Inlet flow rate:           %8.2f kg/s\n', F_inlet)
fprintf('  Inlet temperature:         %8.2f K\n', T_inlet)
fprintf('  Outlet temperature:        %8.2f K\n', T_outlet)
fprintf('  Specific heat capacity:    %8.2f kJ/kg·K\n', C_p)
disp(' ')
disp('Calculated Results:')
fprintf('  Outlet flow rate:          %8.2f kg/s\n', F_outlet)
fprintf('  Temperature change (ΔT):   %8.2f K\n', delta_T)
fprintf('  Heat duty (Q):             %8.2f kW\n', Q)
disp(' ')
disp('===========================')
disp(' ')
```

### Line-by-Line Code Explanation

```matlab
F_inlet = 500;        % Define inlet mass flow (kg/s)
T_inlet = 325;        % Define inlet temperature (K)
T_outlet = 300;       % Define outlet temperature (K)
C_p = 2.8;            % Define specific heat (kJ/kg·K)
```

These lines define the known process parameters. Each variable has a clear name and a comment indicating the units.

```matlab
F_outlet = F_inlet;
```

By conservation of mass, assuming steady state and no accumulation, the outlet flow equals the inlet flow.

```matlab
delta_T = T_inlet - T_outlet;
```

Calculate the temperature drop. Since T_inlet > T_outlet, delta_T will be positive, indicating cooling (heat removal).

```matlab
Q = F_outlet * C_p * delta_T;
```

Apply the energy balance. The heat duty Q is the product of:
- Mass flow rate (kg/s)
- Specific heat capacity (kJ/kg·K)
- Temperature change (K)

The result is in kW (kilowatts).

```matlab
fprintf('  Heat duty (Q):             %8.2f kW\n', Q);
```

Use `fprintf()` to display the result with:
- `%8.2f` — format as a floating-point number with 8 total characters and 2 decimal places
- `\n` — newline (moves to the next line)

### Expected Output

```
=== COOLER HEAT BALANCE ===

Input Data:
  Inlet flow rate:               500.00 kg/s
  Inlet temperature:             325.00 K
  Outlet temperature:            300.00 K
  Specific heat capacity:          2.80 kJ/kg·K

Calculated Results:
  Outlet flow rate:              500.00 kg/s
  Temperature change (ΔT):        25.00 K
  Heat duty (Q):              35000.00 kW

===========================
```

### Numerical Explanation

Given:
- F = 500 kg/s
- T_inlet = 325 K
- T_outlet = 300 K
- C_p = 2.8 kJ/kg·K

Calculation:
- ΔT = 325 - 300 = 25 K
- Q = 500 × 2.8 × 25 = 35,000 kW = 35 MW

### Engineering Interpretation

**Result: Q = 35,000 kW = 35 Megawatts**

This means:

1. **Magnitude**: The cooler must remove 35 megawatts of thermal energy every second to cool the process stream by 25 K. This is a very large cooling duty, typical for industrial-scale operations.

2. **Practical Significance**:
   - **Cooling System Design**: The engineer must design or select a heat exchanger (shell-and-tube, plate-frame, air-cooled, etc.) capable of removing 35 MW
   - **Utility Requirements**: The plant must have sufficient cooling water, refrigerant, or air supply to remove this amount of heat
   - **Operating Cost**: Cooling a large industrial process stream is expensive; this duty contributes significantly to operating costs
   - **Equipment Sizing**: The cooler (heat exchanger) must be sized to handle this duty; undersizing will result in insufficient cooling

3. **Verification**: The calculation makes sense because:
   - Larger flow rates require more heat removal ✓
   - Larger temperature drops require more heat removal ✓
   - Higher specific heat requires more heat removal (more energy stored in each kilogram) ✓

4. **Real-World Context**: In a petroleum refinery or petrochemical plant, multiple coolers of this capacity would be in operation simultaneously, and the total cooling requirement would necessitate large cooling systems (cooling towers, chilled water systems, etc.)

</details>

---

## Key MATLAB Syntax Reference

| Purpose | MATLAB Syntax |
|---|---|
| Display output | `disp('text')` or `disp(variable)` |
| Formatted display | `fprintf('Text: %f\n', variable)` |
| Comment | `% Comment text` |
| Assignment | `var = value;` |
| Addition | `a + b` |
| Subtraction | `a - b` |
| Multiplication | `a * b` |
| Division | `a / b` |
| Power | `a ^ b` |
| Square root | `sqrt(x)` |
| Sine (radians) | `sin(x)` |
| Cosine (radians) | `cos(x)` |
| Tangent (radians) | `tan(x)` |
| Exponential (e^x) | `exp(x)` |
| Natural logarithm | `log(x)` |
| Base-10 logarithm | `log10(x)` |
| Absolute value | `abs(x)` |
| Round to nearest integer | `round(x)` |
| Round down | `floor(x)` |
| Round up | `ceil(x)` |
| Pi constant | `pi` |
| Infinity | `inf` |
| Not-a-Number | `nan` |
| Get help | `help function_name` |

