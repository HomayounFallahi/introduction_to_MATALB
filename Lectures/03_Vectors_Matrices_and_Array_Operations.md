# Vectors, Matrices, and Array Operations

## Learning Objectives

By the end of this lecture, you will be able to:

- Create scalars, row vectors, column vectors, and matrices.
- Initialize arrays with functions such as `zeros`, `ones`, `eye`, `linspace`, and `logspace`.
- Index, slice, and reshape arrays using parentheses and colon notation.
- Apply logical indexing to extract or modify selected elements.
- Perform matrix addition, multiplication, and element-wise operations.
- Distinguish between matrix operators (`*`, `/`, `^`) and element-wise operators (`.*`, `./`, `.^`).
- Compute matrix inverse, determinant, rank, and simple gradients.
- Apply these skills to chemical-engineering calculations such as material balances and property vectors.

---

## Why This Topic Matters

Almost every quantitative task in chemical engineering involves collections of numbers:

- Component flow rates in a stream,
- Temperature or concentration profiles along a reactor,
- Stoichiometric coefficients,
- Experimental measurements at multiple conditions.

MATLAB was designed around matrices. Mastering vector and matrix creation, indexing, and arithmetic is the foundation for all later numerical work (linear systems, differential equations, optimization, data analysis).

---

## Vectors and Matrices

### Concept

A **scalar** is a single number. A **vector** is an ordered list of numbers arranged either horizontally (row vector) or vertically (column vector). A **matrix** is a two-dimensional array of numbers organized in rows and columns. Higher-dimensional arrays extend this concept.

In MATLAB:
- **Scalar**: `x = 5` (1×1)
- **Row vector**: `x = [1 2 3 4]` (1×4)
- **Column vector**: `x = [1; 2; 3; 4]` (4×1)
- **Matrix**: A rectangular array with multiple rows and columns

### Explanation

Vectors and matrices are the fundamental data containers in MATLAB. Unlike many other programming languages, MATLAB treats everything as an array. This "array-first" design makes it naturally suited to scientific and engineering calculations where data typically comes in ordered lists or grids.

### MATLAB Syntax

**Creating a row vector** (values separated by spaces or commas):

```matlab
v = [1 2 3 4];           % spaces

v = [1, 2, 3, 4];        % commas (same result)
```

**Creating a column vector** (values separated by semicolons):

```matlab
P = [1; 2; 3; 4; 5];
```

**Creating a matrix** (rows separated by semicolons, elements within rows separated by spaces):

```matlab
data = [1 2 3; 4 5 6; 7 8 9];
```

**Creating vectors using the colon operator**:

```matlab
x = 1:5;           % [1 2 3 4 5]
y = 0:0.1:1;       % [0 0.1 0.2 ... 0.9 1]
```

**Creating vectors using `linspace`** (evenly spaced values):

```matlab
T = linspace(300, 400, 11);  % 11 points from 300 to 400
```

**Creating vectors using `logspace`** (logarithmically spaced values):

```matlab
freq = logspace(0, 3, 50);   % 50 points from 10^0 to 10^3
```

**Creating matrices of specific values**:

```matlab
zeros_matrix = zeros(3, 4);   % 3×4 matrix of zeros
ones_matrix = ones(2, 5);     % 2×5 matrix of ones
eye_matrix = eye(4);          % 4×4 identity matrix
random_matrix = rand(3, 3);   % 3×3 matrix of random numbers [0,1)
```

### Array Initialization

MATLAB provides convenient functions for creating arrays of a given size:

| Function              | Result                                      | Typical use                          |
|-----------------------|---------------------------------------------|--------------------------------------|
| `zeros(m,n)`          | m×n matrix of zeros                         | Pre-allocation                       |
| `ones(m,n)`           | m×n matrix of ones                          | Initialization                       |
| `eye(n)`              | n×n identity matrix                         | Linear algebra                       |
| `diag(v)`             | Diagonal matrix from vector v               | Stoichiometry, scaling               |
| `linspace(a,b,n)`     | n points linearly spaced from a to b        | Independent variable grids           |
| `logspace(a,b,n)`     | n points logarithmically spaced 10^a to 10^b| Wide-range parameters                |
| `rand(m,n)`           | Uniform random numbers in (0,1)             | Testing, Monte-Carlo                 |
| `randn(m,n)`          | Standard normal random numbers              | Noise simulation                     |


### Syntax Breakdown

For the `linspace` command:

```matlab
T = linspace(start, end, n);
```

- `T` — output vector
- `linspace` — MATLAB function that creates linearly spaced vectors
- `start` — beginning value
- `end` — ending value
- `n` — number of points (including start and end)
- `;` — suppresses command-window output

For the colon operator:

```matlab
x = start:step:end;
```

- `start` — first value
- `step` — increment between values
- `end` — last value (may not be exactly reached if step doesn't divide evenly)

### Simple Example

Suppose you're collecting temperature data from a chemical reactor at regular intervals:

```matlab
% Create a row vector of temperatures measured over time
T = [298 305 312 318 325 330];

% Create a column vector of time points (in minutes)
time = [0; 1; 2; 3; 4; 5];

% Create a matrix of pressure readings at different temperatures and times
% Rows: different time points; Columns: different temperatures
P = [1.0 1.2 1.5; 
     1.1 1.3 1.6; 
     1.2 1.4 1.7];
```

After executing these commands, `T` is a 1×6 row vector, `time` is a 6×1 column vector, and `P` is a 3×3 matrix.

### Example Walkthrough

Let's trace through the creation of the temperature matrix:

```matlab
% Using linspace to create evenly spaced reactor temperatures
T_reactor = linspace(50, 150, 5);  % 5 temperatures from 50°C to 150°C
```

This creates:

```
T_reactor = 50    75   100   125   150   
```

### Chemical Engineering Example

In a multi-component distillation column, we might track the mole fraction of each component at different stages:

```matlab
% Mole fractions of methane (C1), ethane (C2), propane (C3) at 5 stages
% Each row represents a stage; each column represents a component
composition = [0.85 0.12 0.03;    % Stage 1 (reboiler)
               0.75 0.18 0.07;    % Stage 2
               0.60 0.25 0.15;    % Stage 3
               0.40 0.35 0.25;    % Stage 4
               0.15 0.50 0.35];   % Stage 5 (condenser)

% Extract the top of column (stage 5)
top_product = composition(5, :);

% Extract C2 mole fraction across all stages
c2_profile = composition(:, 2);
```

The `composition` matrix has 5 rows (stages) and 3 columns (components).

### Common Mistakes

**Mistake 1: Confusing row and column vectors**
```matlab
% Row vector
x = [1 2 3];        % 1×3

% Column vector
y = [1; 2; 3];      % 3×1

% These are NOT the same!
% x + y will fail because dimensions don't match for addition
```

**Mistake 2: Forgetting semicolons in matrix creation**
```matlab
% CORRECT:
M = [1 2 3; 4 5 6];

% WRONG (creates a different structure):
M = [1 2 3 4 5 6];  % This is a row vector, not a 2×3 matrix
```

**Mistake 3: Using `linspace` incorrectly**
```matlab
% Correct: 11 points from 0 to 10
x = linspace(0, 10, 11);

% Wrong: missing the third argument (forgot number of points)
x = linspace(0, 10);  % Default is 100 points, not what we wanted
```

---

## Indexing and Manipulation

### Concept

**Indexing** is the process of accessing specific elements or groups of elements from an array. **Logical indexing** uses a logical condition to select elements. **Manipulation** includes slicing (extracting subarrays), concatenation (combining arrays), reshaping, and transposing.

### Explanation

Once you have created an array, you need to work with its elements. MATLAB uses **1-based indexing**, meaning the first element is at index 1, not 0 (unlike Python or C). This is both a feature and a source of confusion for programmers switching languages.

### MATLAB Syntax

**Accessing a single element**:

```matlab
x = [10 20 30 40 50];
value = x(3);        % Access the 3rd element: 30
```

**Accessing a matrix element**:

```matlab
M = [1 2 3; 4 5 6; 7 8 9];
element = M(2, 3);   % Row 2, Column 3: 6
```

**Accessing multiple elements (slicing)**:

```matlab
x = [10 20 30 40 50];
subset = x(2:4);     % Elements 2 through 4: [20 30 40]
every_other = x(1:2:5);  % Every other element starting at 1: [10 30 50]
```

**Accessing entire row or column**:

```matlab
M = [1 2 3; 4 5 6; 7 8 9];
row2 = M(2, :);      % Entire row 2: [4 5 6]
col3 = M(:, 3);      % Entire column 3: [3; 6; 9]
```

**Logical indexing**:

```matlab
x = [10 20 30 40 50];
indices = x > 25;    % [false false true true true]
large = x(x > 25);   % [30 40 50]
```

**Modifying elements**:

```matlab
x = [10 20 30 40 50];
x(2) = 25;           % Change 2nd element to 25: [10 25 30 40 50]
x(3:5) = 0;          % Set elements 3-5 to 0: [10 25 0 0 0]
```

**Transposing** (flip rows and columns):

```matlab
x = [1 2 3];         % Row vector: 1×3
y = x';              % Column vector: 3×1
```

**Concatenation**:

```matlab
x = [1 2 3];
y = [4 5];
z = [x y];           % Horizontal concatenation: [1 2 3 4 5]

a = [1; 2];
b = [3; 4];
c = [a; b];          % Vertical concatenation: [1; 2; 3; 4]
```

**Reshaping**:

```matlab
x = 1:12;
M = reshape(x, 3, 4);  % Reshape into 3×4 matrix
```

### Syntax Breakdown

For the slicing operation `x(start:step:end)`:

- `x` — the array
- `start` — beginning index (default 1)
- `step` — increment (default 1)
- `end` — ending index (default length or use the keyword `end`)

For logical indexing `x(condition)`:

- `x` — the array
- `condition` — a logical expression (returns true/false for each element)
- Elements where condition is `true` are selected

### Simple Example

Working with reactor feed streams:

```matlab
% Molar flow rates of components (mol/s)
feed = [10.5 5.2 3.1 0.8];   % C1, C2, C3, C4

% Access individual components
c1_flow = feed(1);    % 10.5 mol/s
c3_flow = feed(3);    % 3.1 mol/s

% Access heavy components (C3 and C4)
heavy = feed(3:4);    % [3.1 0.8]

% Find which components have flow > 5 mol/s
major = feed(feed > 5);  % [10.5 5.2]
```

### Example Walkthrough

Suppose we have experimental data from a batch reactor:

```matlab
% Time (hours) and conversion (%) data
time = [0 0.5 1 1.5 2 2.5 3];
conversion = [0 12 23 32 40 47 52];

% Extract conversion at 1.5 hours (index 4)
conv_at_1_5 = conversion(4);  % 32%

% Extract all conversions after 1 hour (indices 3 onward)
later_conv = conversion(3:end);  % [23 32 40 47 52]

% Find time points where conversion exceeded 40%
high_conv_idx = conversion >= 40;  % [false false false false true true true]
time_high_conv = time(high_conv_idx);  % [2 2.5 3]
```

Step-by-step:

1. `conversion > 40` creates a logical array: `[false false false false true true true]`
2. `time(high_conv_idx)` uses this logical array to select only elements where the condition is true
3. Result: times where conversion was above 40% are returned

### Chemical Engineering Example

Managing multi-phase reactor data:

```matlab
% Temperature profile through a 10-stage reactor (K)
T = [300 305 310 315 320 325 330 335 340 345];

% Pressure at each stage (kPa)
P = [200 205 210 215 220 225 230 235 240 245];

% Reactor residence time distribution (RSD) data
conversion_data = [0.05 0.12 0.22 0.31 0.40 0.47 0.55 0.62 0.68 0.74];

% Extract conditions in the middle section (stages 4-7)
T_middle = T(4:7);
P_middle = P(4:7);
conv_middle = conversion_data(4:7);

% Find stages with temperature above 320 K
hot_stages = T > 320;         % Logical array
T_hot = T(hot_stages);        % Temperatures > 320 K

% Create a combined profile of stages where both T>320 K and conversion>0.5
optimal_stages = (T > 320) & (conversion_data > 0.5);  % AND logic
T_optimal = T(optimal_stages);
conv_optimal = conversion_data(optimal_stages);
```

### Common Mistakes

**Mistake 1: Forgetting that MATLAB uses 1-based indexing**
```matlab
% In Python or C, first element is at index 0
% In MATLAB, first element is at index 1

x = [10 20 30];
value = x(0);   % ERROR! No element at index 0
value = x(1);   % CORRECT: returns 10
```

**Mistake 2: Confusing dimensions in matrix indexing**
```matlab
M = [1 2 3; 4 5 6];  % 2×3 matrix

M(1, 2);   % CORRECT: row 1, column 2 → 2
M(2, 1);   % Different: row 2, column 1 → 4
M(2);      % Single index: accesses in column-major order → 4
```

**Mistake 3: Dimension mismatch in concatenation**
```matlab
x = [1 2 3];      % 1×3
y = [4; 5];       % 2×1
z = [x y];        % ERROR! Cannot concatenate horizontally

% Correct:
z = [x; y'];      % Transpose y first: [1 2 3; 4 5]
```

**Mistake 4: Modifying an array during iteration**
```matlab
% This can cause unexpected behavior:
x = [1 2 3 4 5];
x(x > 2) = 0;   % Fine for simple assignment

% But be careful with more complex operations
% It's generally safer to create a new array
```

### Key Takeaways

- MATLAB uses 1-based indexing (first element is index 1)
- Use `:` for ranges and `end` keyword for the last element
- Logical indexing selects elements based on a condition
- The colon `:` alone means **"all" rows or columns**
- Transpose with `'` flips rows and columns

---

## Matrix Mathematics

### Concept

**Matrix mathematics** includes operations on entire matrices (or vectors as special cases): addition, subtraction, multiplication, division, and specialized operations like finding the inverse or determinant. **Element-wise operations** perform the same calculation on each element independently.

### Explanation

There is a critical distinction in MATLAB between **matrix operations** (which follow the rules of linear algebra) and **element-wise operations** (which apply the operation to each element).

- **Matrix multiplication** (`*`) follows linear algebra rules: an (m×n) matrix times an (n×p) matrix yields an (m×p) result
- **Element-wise multiplication** (`.*`) multiplies corresponding elements and requires arrays of the same size

This distinction is one of the most important concepts in MATLAB.

### MATLAB Syntax

**Matrix addition and subtraction** (element-wise):

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];
C = A + B;        % Addition
D = A - B;        % Subtraction
```
### Sum and Mean


```matlab

data = [10, 20, 30, 40];


total = sum(data);          % 100

average = mean(data);       % 25

```

### Min and Max


```matlab

data = [10, 50, 30, 40, 20];


minimum = min(data);        % 10

maximum = max(data);        % 50


[max_value, index] = max(data);     % Also returns the index: 50, 2

```


**Element-wise multiplication**:

```matlab
A = [1 2; 3 4];
B = [2 2; 2 2];
C = A .* B;       % Element-wise multiplication: [2 4; 6 8]
```

**Matrix multiplication** (linear algebra):

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];
C = A * B;        % Matrix multiplication
```

**Element-wise division**:

```matlab
A = [10 20; 30 40];
B = [2 4; 5 8];
C = A ./ B;       % Element-wise division: [5 5; 6 5]
```

**Matrix division** (solving linear systems):

```matlab
A = [2 1; 1 3];
b = [8; 10];
x = A \ b;        % Solve Ax = b
```

**Element-wise exponentiation**:

```matlab
x = [1 2 3 4];
y = x .^ 2;       % [1 4 9 16]
```

**Matrix transpose**:

```matlab
A = [1 2 3; 4 5 6];
B = A';           % Transpose: [1 4; 2 5; 3 6]
```

**Matrix inverse**:

```matlab
A = [1 2; 3 4];
A_inv = inv(A);
```

**Determinant**:

```matlab
A = [1 2; 3 4];
d = det(A);       % Returns -2
```

**Matrix rank** (number of linearly independent rows/columns):

```matlab
A = [1 2 3; 4 5 6; 7 8 9];
r = rank(A);      % Returns 2 (last row is a linear combination)
```

**Eigenvectors and eigenvalues**:

```matlab
A = [1 2; 2 1];
[V, D] = eig(A);  % V contains eigenvectors, D is diagonal matrix of eigenvalues
```

### Syntax Breakdown

For matrix multiplication `C = A * B`:

- `A` — first matrix (m×n)
- `*` — matrix multiplication operator
- `B` — second matrix (n×p)
- `C` — result matrix (m×p)
- The inner dimensions (n) must match

For element-wise operation `C = A .* B`:

- `.` — indicates "element-wise"
- `*` — the operation
- Both arrays must have the same dimensions or be compatible for broadcasting
- Result has the same shape as the inputs

For solving a system `x = A \ b`:

- `A` — coefficient matrix (m×n)
- `\` — left division (backslash) operator
- `b` — right-hand side vector (m×1)
- `x` — solution vector (n×1)

### Simple Example

Computing reaction rates in parallel reactors:

```matlab
% Initial concentrations of reactant A (mol/L) in 3 reactors
C_A0 = [2.5 2.5 2.5];

% Reaction rate constant (1/s) at different temperatures
k = [0.1 0.15 0.2];

% After 5 seconds, concentration changes as C = C0 * exp(-k*t)
% For simplicity, C = C0 - k*C0*t (linear approximation)
t = 5;

% Element-wise calculation: rate loss per reactor
rate_loss = k .* C_A0 .* t;

% Remaining concentrations
C_A_final = C_A0 - rate_loss;
```

Here, `k .* C_A0` uses element-wise multiplication because we're multiplying corresponding elements (rate × concentration in each reactor independently).

### Example Walkthrough

Solving a material balance system:

Consider a process with three streams. Stream 1 and Stream 2 mix to form Stream 3. Given:
- Stream 1: 100 kg/s of component A, 50 kg/s of component B
- Stream 2: 50 kg/s of component A, 100 kg/s of component B
- Find the composition of Stream 3

```matlab
% Define the stream matrix (rows = streams, columns = components)
% Each row represents flow rates of components in that stream
streams = [100  50;    % Stream 1
            50  100];  % Stream 2

% Sum the streams (element-wise addition)
stream3 = streams(1, :) + streams(2, :);
% Result: [150 150] kg/s of components A and B

% Total flow rate
total_flow = sum(stream3);  % 300 kg/s

% Mass fractions
mass_fractions = stream3 / total_flow;  % Element-wise division
% Result: [0.5 0.5], so 50% each component
```

### Chemical Engineering Example

Energy balance across multiple heat exchangers:

```matlab
% Temperature of inlet streams (K)
T_inlet = [500 400 350];  % Three inlet streams

% Temperature of outlet streams (K)
T_outlet = [450 380 330];

% Heat capacity of each stream (J/(s·K))
Cp = [1000 1500 800];

% Temperature difference
Delta_T = T_inlet - T_outlet;  % Element-wise subtraction

% Heat duty of each exchanger: Q = m*Cp*ΔT (simplified, m=1)
Q = Cp .* Delta_T;  % Element-wise multiplication

% Total heat transferred
Q_total = sum(Q);

% Average temperature drop (weighted)
avg_temp_drop = Q_total / sum(Cp);
```

### Common Mistakes

**Mistake 1: Confusing `*` and `.*`**
```matlab
A = [1 2; 3 4];
B = [2 3; 4 5];

C = A * B;      % Matrix multiplication (linear algebra)
% Result: [1*2+2*4  1*3+2*5; 3*2+4*4  3*3+4*5] = [10 13; 22 29]

D = A .* B;     % Element-wise multiplication
% Result: [1*2  2*3; 3*4  4*5] = [2 6; 12 20]
% Completely different!
```

**Mistake 2: Incompatible dimensions for matrix multiplication**
```matlab
A = [1 2 3];    % 1×3
B = [4 5 6];    % 1×3

C = A * B;      % ERROR! Inner dimensions (3 and 1) don't match
C = A * B';     % CORRECT: [1×3] * [3×1] = [1×1]
```

**Mistake 3: Forgetting that `\` is not regular division**
```matlab
A = [2 1; 1 3];
b = [5; 6];

% WRONG: This is element-wise division
x = b / A;    % ERROR (actually this does right division, not solving)

% CORRECT: Solve Ax = b using left division
x = A \ b;    % Solves the system
```

**Mistake 4: Using `inv()` for solving systems (inefficient and less stable)**
```matlab
A = [2 1; 1 3];
b = [5; 6];

% Slower and less accurate:
x = inv(A) * b;

% Better:
x = A \ b;
```

### Key Takeaways

- Matrix operations follow linear algebra rules; dimensions must be compatible
- Element-wise operations require the dot notation (`.`) and work element-by-element
- Use `*` for matrix multiplication, `.*` for element-wise multiplication
- Use `\` (left division) to solve systems Ax = b
- Transpose with `'` (single quote)

**Other matrix functions:**
- `inv(A)` — inverse (avoid for solving)
- `det(A)` — determinant
- `rank(A)` — rank, tells if system is independent
- `gradient(v)` — numerical gradient, useful for dT/dz

---

## Worked Example: Chemical Reactor Network Analysis

Consider a series of three connected reactors where material flows from one to the next. We track concentrations of a chemical species (mol/L) at each reactor stage.

**Problem**: Given inlet concentration and flow rates, calculate outlet concentrations and steady-state conversions.

**Data**:
- Inlet concentration: 5 mol/L
- Reactor volumes: [500, 800, 1000] L
- Volumetric flow rate: 100 L/min
- Reaction rate constant: k = 0.01 min^-1 (first-order reaction)

**Solution**:

```matlab
% Given data
C_inlet = 5;                    % mol/L
V = [500 800 1000];            % Reactor volumes (L)
F_vol = 100;                   % Volumetric flow rate (L/min)
k = 0.01;                      % Reaction rate constant (min^-1)

% Calculate residence time in each reactor
tau = V / F_vol;               % tau = [5 8 10] min

% For first-order reaction: C_outlet = C_inlet * exp(-k*tau)
% Element-wise calculation
C_out_1 = C_inlet .* exp(-k .* tau(1));
C_out_2 = C_out_1 .* exp(-k .* tau(2));
C_out_3 = C_out_2 .* exp(-k .* tau(3));

% Or vectorized: compute all at once
C_cascade = C_inlet * exp(-k * sum(tau));

% Conversion at each stage
conversion_1 = (C_inlet - C_out_1) / C_inlet;
conversion_2 = (C_out_1 - C_out_2) / C_out_1;
conversion_3 = (C_out_2 - C_out_3) / C_out_2;
overall_conversion = (C_inlet - C_out_3) / C_inlet;

% Display results
fprintf('Inlet concentration: %.2f mol/L\n', C_inlet);
fprintf('Residence times: %.1f, %.1f, %.1f min\n', tau(1), tau(2), tau(3));
fprintf('\nOutlet concentrations:\n');
fprintf('  After Reactor 1: %.4f mol/L (conversion: %.2f%%)\n', C_out_1, conversion_1*100);
fprintf('  After Reactor 2: %.4f mol/L (conversion: %.2f%%)\n', C_out_2, conversion_2*100);
fprintf('  After Reactor 3: %.4f mol/L (conversion: %.2f%%)\n', C_out_3, conversion_3*100);
fprintf('\nOverall conversion: %.2f%%\n', overall_conversion*100);
```

**Output**:
```
Inlet concentration: 5.00 mol/L
Residence times: 5.0, 8.0, 10.0 min

Outlet concentrations:
  After Reactor 1: 4.7561 mol/L (conversion: 4.88%)
  After Reactor 2: 4.3905 mol/L (conversion: 7.69%)
  After Reactor 3: 3.9727 mol/L (conversion: 9.52%)

Overall conversion: 20.55%
```

---

## Chemical Engineering Application: Distillation Column Composition Profiles

In a multi-component distillation column, we need to track mole fractions of multiple components across many stages. Matrices are ideal for this application.

```matlab
% Distillation column with 8 stages
% Separating a 3-component mixture: benzene (B), toluene (T), xylene (X)

% Mole fraction composition at each stage (rows = stages, columns = components)
% Stage 1 is reboiler, Stage 8 is condenser
composition = [0.95 0.04 0.01;      % Stage 1 - heavy product (mostly B)
               0.88 0.10 0.02;      % Stage 2
               0.75 0.20 0.05;      % Stage 3
               0.55 0.35 0.10;      % Stage 4
               0.35 0.50 0.15;      % Stage 5
               0.20 0.65 0.15;      % Stage 6
               0.10 0.75 0.15;      % Stage 7
               0.05 0.90 0.05];     % Stage 8 - light product (mostly T)

% Extract products
bottoms = composition(1, :);        % Bottom product: [0.95 0.04 0.01]
overhead = composition(8, :);       % Top product: [0.05 0.90 0.05]

% Extract toluene composition profile
toluene_profile = composition(:, 2);

% Find stages where toluene exceeds 50%
high_toluene = composition(:, 2) > 0.5;
stages_high_T = find(high_toluene);  % [4, 5, 6, 7]

% Calculate separation quality
purity_B = bottoms(1) * 100;        % Benzene purity in bottoms
purity_T = overhead(2) * 100;       % Toluene purity in overhead

fprintf('Distillation Column Analysis\n');
fprintf('==============================\n');
fprintf('Bottom product (stage 1): %.1f%% benzene\n', purity_B);
fprintf('Top product (stage 8):    %.1f%% toluene\n', purity_T);
fprintf('Toluene content at stages with >50%%: stages %s\n', mat2str(stages_high_T));
```


---

## Key MATLAB Syntax Reference

| **Purpose** | **MATLAB Syntax** | **Example** |
|---|---|---|
| Create row vector | `x = [a b c ...]` | `x = [1 2 3 4]` |
| Create column vector | `x = [a; b; c; ...]` | `x = [1; 2; 3; 4]` |
| Create matrix | `M = [row1; row2; ...]` | `M = [1 2; 3 4]` |
| Evenly spaced vector | `linspace(start, end, n)` | `x = linspace(0, 1, 11)` |
| Vector with step | `start:step:end` | `x = 0:0.5:5` |
| Access element | `x(i)` or `M(i, j)` | `val = x(3); elt = M(1,2)` |
| Slice array | `x(start:end)` | `sub = x(2:4)` |
| All rows/columns | `:` | `row = M(2, :); col = M(:, 3)` |
| Logical indexing | `x(condition)` | `large = x(x > 10)` |
| Transpose | `x'` or `x.'` | `col = row'` |
| Concatenate | `[A B]` or `[A; B]` | `c = [x y]` or `c = [x; y]` |
| Reshape | `reshape(A, m, n)` | `M = reshape(x, 3, 4)` |
| Element-wise multiply | `A .* B` | `C = A .* B` |
| Matrix multiply | `A * B` | `C = A * B` |
| Element-wise power | `A .^ p` | `y = x .^ 2` |
| Matrix inverse | `inv(A)` | `A_inv = inv(A)` |
| Determinant | `det(A)` | `d = det(A)` |
| Matrix rank | `rank(A)` | `r = rank(A)` |
| Solve system Ax=b | `x = A \ b` | `x = A \ b` |

---

## Homework


### Problem


A chemical plant processes a mixture of three hydrocarbons: methane (CH₄), ethane (C₂H₆), and propane (C₃H₈).


**Given Data:**


- Inlet feed composition: 60% CH₄, 25% C₂H₆, 15% C₃H₈

- Inlet feed rate: 500 kmol/h

- Outlet composition: 45% CH₄, 35% C₂H₆, 20% C₃H₈

- Outlet flow rate: 480 kmol/h


**Required Tasks:**


1. Create a vector for inlet composition and calculate the inlet molar flow rates of each component.

2. Create a vector for outlet composition and calculate the outlet molar flow rates.

3. Perform a material balance check: Does mass balance? (Hint: Sum of inlet flow = Sum of outlet flow?)

4. Calculate the recovery of each component (outlet flow / inlet flow).

5. Display the results clearly.


**Concepts Being Tested:**


- Creating vectors

- Element-wise multiplication and division

- Vector indexing

- Material balance calculations


**Hints:**


- Use element-wise operations to calculate component flows

- Create a summary matrix or display the results row by row

- Remember that inlet and outlet flows should have approximately equal totals if no accumulation occurs


<details>

<summary><strong>Detailed Solution — Click to Reveal</strong></summary>


## Solution


### Concept


This problem applies **material balance principles** using MATLAB vectors. Material balance states that mass is conserved (ignoring accumulation):


$$\sum \text{Inlet Flow} = \sum \text{Outlet Flow} + \text{Accumulation}$$


In a steady-state process without accumulation or side streams, inlet and outlet should match.


### Idea


1. Create vectors for inlet and outlet compositions.

2. Use element-wise multiplication to calculate component flow rates.

3. Verify the total mass balance.

4. Calculate recovery fractions (outlet/inlet for each component).

5. Display organized results.


### Syntax


Key MATLAB operations:


- Vector creation: `v = [0.6, 0.25, 0.15]`

- Element-wise multiplication: `flow = composition .* total_flow`

- Element-wise division: `recovery = outlet ./ inlet`

- Logical check: `total_inlet == total_outlet`


### Syntax Breakdown


```matlab

% Create composition vectors

x_inlet = [0.6, 0.25, 0.15];     % Mole fractions: CH4, C2H6, C3H8

x_outlet = [0.45, 0.35, 0.20];   % Outlet mole fractions

```


These are row vectors with three elements each.


```matlab

% Calculate component flow rates

F_inlet_component = x_inlet .* F_inlet_total;

F_outlet_component = x_outlet .* F_outlet_total;

```


The `.*` operator multiplies each composition fraction by the total flow, giving the flow of each component.


```matlab

% Recovery calculation

recovery = F_outlet_component ./ F_inlet_component;

```


Element-wise division gives the fraction of each component recovered.


### MATLAB Code


```matlab

% Hydrocarbon Mixture Material Balance

% Process: Feed mixture → Separation/Processing → Product


% ===== INLET STREAM =====

% Composition (mole fractions)

comp_inlet = [0.60, 0.25, 0.15];        % CH4, C2H6, C3H8

F_inlet_total = 500;                    % kmol/h


% Calculate component flow rates in inlet

F_inlet = comp_inlet .* F_inlet_total;

% Result: [300, 125, 75] kmol/h


% ===== OUTLET STREAM =====

% Composition (mole fractions)

comp_outlet = [0.45, 0.35, 0.20];       % CH4, C2H6, C3H8

F_outlet_total = 480;                   % kmol/h


% Calculate component flow rates in outlet

F_outlet = comp_outlet .* F_outlet_total;

% Result: [216, 168, 96] kmol/h


% ===== MATERIAL BALANCE CHECK =====

total_inlet = sum(F_inlet);

total_outlet = sum(F_outlet);

mass_balance_error = total_inlet - total_outlet;


% ===== RECOVERY CALCULATION =====

recovery = F_outlet ./ F_inlet;         % Element-wise division


% ===== DISPLAY RESULTS =====

disp(''==========================================='')

disp(''      Hydrocarbon Plant Material Balance'')

disp(''==========================================='')

disp('' '')


% Create labels for clarity

components = {''Methane (CH4)'', ''Ethane (C2H6)'', ''Propane (C3H8)''};


disp(''INLET STREAM'')

disp([''Component | Mole Fraction | Flow Rate (kmol/h)''])

for i = 1:3

    fprintf(''%s | %.0f%% | %.1f\n'', components{i}, comp_inlet(i)*100, F_inlet(i))

end

fprintf(''Total inlet flow: %.1f kmol/h\n'', total_inlet)

disp('' '')


disp(''OUTLET STREAM'')

disp([''Component | Mole Fraction | Flow Rate (kmol/h)''])

for i = 1:3

    fprintf(''%s | %.0f%% | %.1f\n'', components{i}, comp_outlet(i)*100, F_outlet(i))

end

fprintf(''Total outlet flow: %.1f kmol/h\n'', total_outlet)

disp('' '')


disp(''MATERIAL BALANCE'')

fprintf(''Inlet total:  %.1f kmol/h\n'', total_inlet)

fprintf(''Outlet total: %.1f kmol/h\n'', total_outlet)

fprintf(''Difference:   %.1f kmol/h\n'', mass_balance_error)

disp('' '')


disp(''RECOVERY (Outlet / Inlet)'')

for i = 1:3

    fprintf(''%s: %.2f (%.1f%%)\n'', components{i}, recovery(i), recovery(i)*100)

end

```


### Line-by-Line Explanation


**Setting up inlet data:**


```matlab

comp_inlet = [0.60, 0.25, 0.15];

F_inlet_total = 500;

F_inlet = comp_inlet .* F_inlet_total;

```


The element-wise multiplication `.*` applies the total flow to each composition fraction:

- CH₄: 0.60 × 500 = 300 kmol/h

- C₂H₆: 0.25 × 500 = 125 kmol/h

- C₃H₈: 0.15 × 500 = 75 kmol/h


**Calculating recovery:**


```matlab

recovery = F_outlet ./ F_inlet;

```


This divides outlet by inlet for each component:

- CH₄: 216 / 300 = 0.72 (72% recovered)

- C₂H₆: 168 / 125 = 1.34 (134% recovered — impossible!)

- C₃H₈: 96 / 75 = 1.28 (128% recovered — also impossible!)


**Using fprintf for formatted output:**


```matlab

fprintf(''%s | %.0f%% | %.1f\n'', components{i}, comp_inlet(i)*100, F_inlet(i))

```


This formats output with text (%s), percentages (%.0f%%), and decimals (%.1f).


### Expected Result


```

===========================================

      Hydrocarbon Plant Material Balance

===========================================

 

INLET STREAM

Component | Mole Fraction | Flow Rate (kmol/h)

Methane (CH4) | 60% | 300.0

Ethane (C2H6) | 25% | 125.0

Propane (C3H8) | 15% | 75.0

Total inlet flow: 500.0 kmol/h

 

OUTLET STREAM

Component | Mole Fraction | Flow Rate (kmol/h)

Methane (CH4) | 45% | 216.0

Ethane (C2H6) | 35% | 168.0

Propane (C3H8) | 20% | 96.0

Total outlet flow: 480.0 kmol/h

 

MATERIAL BALANCE

Inlet total:  500.0 kmol/h

Outlet total: 480.0 kmol/h

Difference:   20.0 kmol/h

 

RECOVERY (Outlet / Inlet)

Methane (CH4): 0.72 (72.0%)

Ethane (C2H6): 1.34 (134.0%)

Propane (C3H8): 1.28 (128.0%)

```


### Engineering Interpretation


**Material Balance Issue:** The outlet total (480 kmol/h) is less than the inlet total (500 kmol/h) by 20 kmol/h. This 20 kmol/h represents either:


1. Losses in the process (product removed to storage)

2. Measurement error

3. Accumulation in equipment


**Recovery Anomaly:** The recovery fractions exceed 1.0 (100%) for ethane and propane, which is impossible. This indicates the problem data is inconsistent with physical reality. In a real scenario, you would:


1. Check that all measurements are correct

2. Look for unmeasured outlets (liquid product, waste stream, etc.)

3. Investigate whether the outlet composition was measured correctly


This demonstrates why material balance checks are critical in process engineering—they validate experimental data and reveal when something is wrong in the process or measurements.


</details>

---

