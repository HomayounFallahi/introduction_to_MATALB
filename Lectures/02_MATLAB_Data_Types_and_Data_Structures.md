# MATLAB Data Types and Data Structures

## Learning Objectives

By the end of this lecture, you will be able to:

- Distinguish MATLAB's fundamental data types: `double`, `single`, `logical`, `char`, and `string`
- Create and manipulate `datetime` and `duration` types for time-stamped process data and calculate time differences
- Build and index cell arrays, structure arrays, and table arrays to organize heterogeneous chemical engineering data
- Explain when and why to use sparse arrays for large, mostly-zero matrices (e.g., stoichiometric or connectivity matrices)
- Convert between numeric, string, logical, table, cell, and structure types safely
- Check data types using `class`, `isa`, `isnumeric`, `islogical`, `istable`
- Choose the appropriate data structure for experimental lab data, process historian data, and equipment databases

## Why This Topic Matters

In Lecture 1, every variable was a single `double`: `T = 350`. Real chemical engineering data is never that simple.

A lab notebook contains:
- Numeric: temperature 350 K, pressure 5 bar, concentration 0.45 mol/L
- Logical: sensor is faulty? true/false, alarm on/off
- Text: species names "Benzene", "Toluene", equipment tag "TIC-101"
- Time: sample taken at 2026-08-20 14:35:00, batch lasted 2.5 hours
- Heterogeneous: one experiment row = time (datetime) + species (string) + concentration (double) + valid? (logical)

MATLAB provides specialized types to store this cleanly. Using only `double` matrices forces you to encode species names as numbers (error-prone). Using the right type makes code readable, self-documenting, and less buggy.

For example:
- `table` arrays are the MATLAB equivalent of an Excel lab data sheet — perfect for CSTR kinetic data
- `struct` arrays model equipment: `reactor.volume`, `reactor.T`, `reactor.catalyst`
- `datetime` lets you subtract timestamps to get residence time, without manual hour conversions
- `sparse` stores a 10,000 x 10,000 stoichiometric matrix with only 0.1% nonzeros using kilobytes instead of 800 MB

This lecture teaches you to pick the right container for your engineering information.

---

## Fundamental Data Types

### What Are Data Types?

A **data type** specifies how MATLAB interprets and stores a value in memory. Common data types include numbers (integers, floating-point), logical values (true/false), and text.

### Numeric Data Types

#### Double Precision Floating-Point (`double`)

**What it is:** The default numeric type in MATLAB. Uses 64 bits of memory per number. Can represent values from approximately $10^{-308}$ to $10^{308}$ with about 15-17 significant digits.

**When to use:** For most engineering calculations. Temperature, pressure, concentration, flowrate, etc.

**Why it matters:** Double precision provides excellent accuracy for typical engineering applications without requiring excessive memory.

```matlab
T = 350;              % Temperature in Kelvin (default: double)
P = 101325;           % Pressure in Pa (default: double)
concentration = 2.5;  % mol/m³ (default: double)
```


You can explicitly create a `double`:

```matlab
x = double(42);       % Explicitly convert to double
class(x)              % Returns: 'double'
```

#### Single Precision Floating-Point (`single`)

**What it is:** Uses 32 bits of memory per number. Provides ~7 significant digits of accuracy.

**When to use:** Rarely, and only when memory constraints are severe (e.g., processing enormous datasets). Large image processing or GPU arrays sometimes use `single`.

**Example:**

```matlab
x_double = 3.14159265;           % Uses 64 bits
x_single = single(3.14159265);   % Uses 32 bits
whos  % Shows variable sizes

% Output:
%   Name         Size         Bytes   Class
%   x_double     1x1            8     double
%   x_single     1x1            4     single
```

For chemical engineering: Stick to `double` unless working with specialized hardware.

### Converting Between Numeric Types

```matlab
% Convert double to single
x = 3.14159265;
x_single = single(x);

% Convert single to double
y_double = double(x_single);

% Check the current type
class(x)        % returns ''double''
class(x_single) % returns ''single''
```

### Logical Data Type

**What it is:** Represents true/false values (also called Boolean values).

**When to use:** Conditional logic, filtering data, masking arrays.

**Example:**

```matlab
% Checking if a reaction temperature exceeds a safety limit
T_measured = 375;     % K
T_limit = 400;        % K
is_safe = T_measured < T_limit;  % Result: true (logical value)
class(is_safe)  % Returns: 'logical'

% Create a logical array
reactor_type = ["CSTR", "PFR", "Batch", "CSTR"];
is_cstr = (reactor_type == "CSTR");  % [1 0 0 1] as logical
```

**Why it matters:** Logical values allow you to filter data, control loops, and make decisions in your code.

### Character Arrays and Strings

**What they are:** Two related but distinct ways to store text.

#### Character Arrays (`char`)

Uses single quotes. Each character is stored individually.

```matlab
compound = 'methanol';   % Character array
class(compound)          % Returns: 'char'
```

#### String Arrays (`string`)

Uses double quotes. Introduced in later MATLAB versions. More flexible and recommended.

```matlab
compound = "methanol";    % String (preferred in modern MATLAB)
class(compound)           % Returns: 'string'
```

**Why the difference matters:**
- Strings are easier to work with (concatenation, comparison, manipulation)
- Character arrays are sometimes required by older functions
- Modern MATLAB strongly prefers strings

**Example — Chemical compound data:**

```matlab
% Using strings (modern, recommended)
compound = "benzene";
formula = "C6H6";
molar_mass = 78.11;       % g/mol

% Combined in a logical way (we'll see better approaches later)
fprintf("Compound: %s, Formula: %s, MW: %.2f g/mol\n", compound, formula, molar_mass);
% Output: Compound: benzene, Formula: C6H6, MW: 78.11 g/mol
```

**Syntax breakdown:**
- `fprintf()` — formats and prints to command window
- `%s` — placeholder for string
- `%.2f` — placeholder for decimal number with 2 digits after decimal point
- `\n` — newline character

---

## Date and Time Data

### Why Date and Time Data Matter

Experimental data is meaningless without a timestamp. When did the measurement occur? Over how long did the reaction run? How long did the cooling process take?

### Creating Date/Time Data

#### `datetime` — Specific Moments

**What it is:** Represents a specific date and time.

```matlab
% Create a datetime for when an experiment started
start_time = datetime(2024, 3, 15, 14, 30, 0);  % Year, Month, Day, Hour, Minute, Second
disp(start_time)
% Output: 15-Mar-2024 14:30:00
```

**Example — Recording experiment times:**

```matlab
% Experimental data collection
experiment_date = datetime(2024, 3, 15);
sample_1_time = datetime(2024, 3, 15, 9, 0, 0);   % 9:00 AM
sample_2_time = datetime(2024, 3, 15, 9, 30, 0);  % 9:30 AM
sample_3_time = datetime(2024, 3, 15, 10, 0, 0);  % 10:00 AM

disp(sample_1_time);  % 15-Mar-2024 09:00:00
```

#### `duration` — Time Intervals

**What it is:** Represents an elapsed time (e.g., "2 hours 30 minutes").

```matlab
% Calculate how long an experiment ran
reaction_duration = duration(0, 2, 30);  % 0 hours, 2 minutes, 30 seconds
disp(reaction_duration);
% Output: 0:02:30

% Alternatively, using hours()
residence_time = hours(2.5);  % 2.5 hours
disp(residence_time);
% Output: 2:30:00
```
Or:

```matlab
reaction_time = hours(2);      % 2 hours
reaction_time = minutes(30);   % 30 minutes
reaction_time = seconds(120);  % 120 seconds
```

**Why it matters:** In reactor engineering, residence time is critical. MATLAB's `duration` type makes it easy to work with these quantities correctly.

### Date/Time Arithmetic

```matlab
% A reaction starts at 10:00 AM and runs for 3 hours
start_time = datetime(2024, 3, 15, 10, 0, 0);
run_time = hours(3);
end_time = start_time + run_time;

disp(start_time);  % 15-Mar-2024 10:00:00
disp(end_time);    % 15-Mar-2024 13:00:00

% Calculate elapsed time between two timestamps
time_difference = end_time - start_time;
disp(time_difference);  % 3:00:00 (3 hours)
```

### Formatting Dates and Durations

```matlab
% Display a datetime in a specific format
t = datetime(2024, 3, 15, 14, 30, 45);
disp(t);                     % Default: 15-Mar-2024 14:30:45
disp(datestr(t, 'HH:MM'));   % Just hours and minutes: 14:30

% Extract individual components
year_val = year(t);    % 2024
month_val = month(t);  % 3
day_val = day(t);      % 15
hour_val = hour(t);    % 14
```

```matlab
t = datetime(2026, 8, 30, 9, 15, 0);
string(t, 'yyyy-MM-dd HH:mm')             % "2026-08-30 09:15"
string(t, 'dd-MMM-yyyy')                  % "30-Aug-2026"
```

#### Using Current Date and Time

```matlab
now_time = datetime('now');
today_date = datetime('today');
```
You can also create **arrays of timestamps**:

```matlab
t = datetime(2026, 8, 30) + hours(0:0.5:5);  % every 30 min for 5 h
```

### Common Mistakes

- Forgetting that subtraction of two `datetime` values yields a `duration`, not a number.
- Mixing time zones without explicit conversion.
- Using old `datenum` functions when `datetime` is clearer.

---

## MATLAB Data Structures

So far, we've stored single values (scalars) and discussed how to work with multiple values. Now we'll organize collections of related data.

### Cell Arrays

**What it is:** A flexible container that can hold different data types in different cells, like a box with compartments. Each compartment can hold anything.

**When to use:** Storing data of mixed types, or when you need maximum flexibility.

**Creating cell arrays:**

```matlab
% Create a cell array using curly braces {}
experimental_data = {
    "Reactor A",           % Cell 1: string
    350,                   % Cell 2: double (temperature in K)
    [0.5, 0.6, 0.7],     % Cell 3: numerical array (concentrations)
    true                   % Cell 4: logical (reactor online?)
};

class(experimental_data)  % Returns: 'cell'
```

**Create a 3x2 cell array** (3 rows, 2 columns)
```matlab
C = {'Alpha', 100; ...
     'Beta',  200; ...
     'Gamma', 300};
```

**Accessing cell array elements:**

```matlab
% Use curly braces {} to access the contents
reactor_name = experimental_data{1};      % "Reactor A"
temperature = experimental_data{2};       % 350
concentrations = experimental_data{3};    % [0.5, 0.6, 0.7]
is_online = experimental_data{4};         % true

% Use parentheses () to access the cell itself (not its contents)
cell_itself = experimental_data(1);       % Returns a cell, not the string
disp(class(cell_itself));                 % Returns: 'cell'
```

**Why the difference?** In the first example, `{}` extracts the contents. In the second, `()` extracts the cell as an object. 

**Common mistake:** 
- `{}` — cell array constructor and content access
- `()` — returns a cell (box), `{}` returns what's inside box
- `C{1,1}` vs `C(1,1)` is the most common source of errors


```matlab
% WRONG
name = experimental_data(1);   % Returns the cell, not the string
disp(name);                    % Displays: {'Reactor A'}

% CORRECT
name = experimental_data{1};   % Returns the string
disp(name);                    % Displays: Reactor A
```

### Structure Arrays

**What it is:** Organized data with named fields. Like a table with column headers.

**When to use:** When you want to organize data with meaningful field names, making your code more readable.

**Creating structures:**

```matlab
% Method 1: Dot notation
reactor.name = "Reactor A";
reactor.temperature = 350;        % K
reactor.pressure = 10;            % bar
reactor.feed_rate = 2.5;          % mol/s
reactor.product_concentration = 0.75;  % mol/m³

% Now 'reactor' is a structure with 5 fields
```

**Accessing structure fields:**

```matlab
% Use dot notation
T = reactor.temperature;           % 350
disp(reactor.name);                % Reactor A
```

**Why it's better than cell arrays for this purpose:**

```matlab
% With cell arrays, you must remember the order
data = {"Reactor A", 350, 10, 2.5, 0.75};
T = data{2};  % You must know that temperature is position 2

% With structures, the name is explicit
reactor.temperature;  % Clear what this is
```

**Multiple structures in an array:**

```matlab
% Create multiple reactors
reactor(1).name = "Reactor A";
reactor(1).temperature = 350;
reactor(1).pressure = 10;

reactor(2).name = "Reactor B";
reactor(2).temperature = 375;
reactor(2).pressure = 12;

% Access the second reactor's temperature
T_B = reactor(2).temperature;  % 375
```

**Modifying structure data:**

```matlab
% Change the temperature of Reactor A
reactor(1).temperature = 360;

% Add a new field to all reactors
reactor(1).conversion = 0.85;
reactor(2).conversion = 0.92;
```

### Table Arrays

**What it is:** A table with rows and columns, like a spreadsheet or database table. Each column has a name and contains data of the same type.

**When to use:** For organized, rectangular datasets (most common in data analysis and reporting). Better than structures when all rows have the same fields.

**Creating tables:**

```matlab
% Create column vectors first
reactor_names = ["Reactor A"; "Reactor B"; "Reactor C"];
temperatures = [350; 375; 360];          % K
pressures = [10; 12; 11];               % bar
conversions = [0.85; 0.92; 0.88];       % fraction

% Create table from columns
reactor_data = table(reactor_names, temperatures, pressures, conversions);
disp(reactor_data);
```

**Output:**

```
reactor_names  temperatures  pressures  conversions
_____________  ____________  _________  ___________
"Reactor A"         350          10         0.85
"Reactor B"         375          12         0.92
"Reactor C"         360          11         0.88
```
Or:
```matlab
Name = ["FEED"; "PRODUCT"; "RECYCLE"];
T    = [350; 450; 380];          % K
P    = [5.0; 4.8; 5.1];          % bar
F    = [120; 95; 25];            % mol/s

S = table(Name, T, P, F);
```
```matlab
% Simple material-balance table
comp   = ["A"; "B"; "C"];
F_in   = [50; 30; 20];           % mol/s
F_out  = [5;  28; 67];           % mol/s
conv   = (F_in - F_out) ./ F_in; % conversion (component A reacts)

MB = table(comp, F_in, F_out, conv, ...
    'VariableNames', {'Component','Fin_mol_s','Fout_mol_s','Conversion'});
disp(MB)
```

**Accessing table data:**

```matlab
% Access a column by name
temps = reactor_data.temperatures;      % [350; 375; 360]

% Access a single cell
T_first = reactor_data.temperatures(1);  % 350

% Add a new column
reactor_data.yield = [0.92; 0.88; 0.91];

% Access a row (returns as table)
first_reactor = reactor_data(1, :);
```

**Why tables are powerful:**

```matlab
% Calculate mean temperature (one line)
mean_temp = mean(reactor_data.temperatures);  % 361.67 K

% Find reactors with conversion > 0.90
high_conversion = reactor_data(reactor_data.conversions > 0.90, :);
disp(high_conversion);
```

---

## Specialized Data Types

### Sparse Arrays

**What it is:** A matrix that stores only non-zero values, saving memory when most elements are zero.

**When to use:** Large matrices with many zeros (common in solving linear systems from process models).

**Example — Why sparse arrays matter:**

```matlab
% In chemical process modeling, you often have sparse matrices
% For example, a connectivity matrix for a multi-stage process

% Create a full 1000 × 1000 matrix with mostly zeros
A_full = zeros(1000, 1000);
A_full(1, 2) = 0.8;    % Feed to stage 2
A_full(2, 3) = 0.9;    % Transfer stage 2 to 3
% ... add a few more non-zero entries ...

% Create a sparse version
A_sparse = sparse(A_full);

% Memory comparison
whos A_full A_sparse
% A_full: ~8 MB (stores all 1,000,000 zeros)
% A_sparse: ~0.01 MB (stores only the non-zero values)
```

**Creating sparse arrays directly:**

```matlab
% Specify row, column, and value for each non-zero element
rows = [1, 2, 3, 4];
cols = [2, 3, 4, 5];
values = [0.8, 0.9, 0.85, 0.95];

A = sparse(rows, cols, values, 5, 5);  % 5 × 5 sparse matrix
disp(A);
% Displays only the non-zero elements
```

**Important note:** For most introductory engineering calculations, you won't use sparse arrays. We mention them here for completeness.

---

## Data Type Conversion

### Why Convert Between Types?

Sometimes you need to change a variable's type:

- A function expects a `logical` but you have a `double`
- You need to convert text input from a user to a number
- You need to change precision to save memory

### Common Conversions

#### String to Number

```matlab
% User enters a value as text
user_input = "350";                    % string
T = double(user_input);                % Convert to double
disp(T);                               % 350 (numeric)

% Alternatively
T = str2double(user_input);            % str2double() also works
```

#### Number to String

```matlab
% Convert a number to text (useful for display)
T = 350.5;
T_string = string(T);                  % "350.5"
% or
T_string = num2str(T);                 % Also works
```

#### Logical Conversion

```matlab
% Convert double to logical
test_value = 1;
is_true = logical(test_value);         % true
disp(class(is_true));                  % 'logical'

% Non-zero double → true, zero → false
test_value2 = 0;
is_false = logical(test_value2);       % false
```

#### Converting Between Data Structures

```matlab
% Cell array to table (when cells contain uniform data)
data_cell = {"Temperature", 350; "Pressure", 10; "Flowrate", 2.5};
% This approach is cumbersome; better to use table() directly

% Better: Create table directly
reactor_info = table(["Temperature"; "Pressure"; "Flowrate"], ...
                     [350; 10; 2.5], ...
                     'VariableNames', {'Property', 'Value'});
```
Useful conversion functions:

| From → To          | Function              |
|--------------------|-----------------------|
| Number → string    | `string`, `num2str`   |
| String → number    | `str2double`, `str2num` |
| Anything → logical | `logical`             |
| Cell → table       | `cell2table`          |
| Table → cell       | `table2cell`          |
| Struct → table     | `struct2table`        |
| Table → struct     | `table2struct`        |
---
### Checking Data Types

#### `class()` Function

Returns the data type of a variable as a string.

```matlab
x = 42;
disp(class(x));        % 'double'

y = "hello";
disp(class(y));        % 'string'

z = true;
disp(class(z));        % 'logical'
```

#### `isa()` Function

Tests if a variable belongs to a specific type (returns logical true/false).

```matlab
x = 42;
isa(x, 'double');      % true
isa(x, 'string');      % false

y = "temperature";
isa(y, 'string');      % true
```

#### `whos` Command

Displays information about all variables in the workspace, including type and size.

```matlab
T = 350;
P = [101325, 202650];
reactor_name = "Reactor A";
is_online = true;

whos
% Output:
%   Name              Size         Bytes  Class
%   P                 1x2             16  double
%   T                 1x1              8  double
%   is_online         1x1              1  logical
%   reactor_name      1x1             38  string
```

---

## Chemical Engineering Example: Storing Reactor Experiment Data

Let's combine several data types to store realistic chemical engineering data.

### Problem

You run an experiment with three reactors. For each reactor, you record:
- Reactor name (text)
- Operating temperature (K)
- Inlet pressure (bar)
- Feed concentration (mol/m³)
- Reaction time (hours)
- Date and time the experiment started
- Whether the reactor was stable (true/false)
- Measured product concentrations at different times (array)

### Solution Using a Structure Array

```matlab
% Define three reactors as a structure array
% Reactor 1
reactor(1).name = "CSTR A";
reactor(1).temperature = 350;           % K
reactor(1).pressure_inlet = 5;          % bar
reactor(1).C_inlet = 2.0;               % mol/m³
reactor(1).time_reaction = hours(2);    % duration
reactor(1).start_time = datetime(2024, 3, 15, 9, 0, 0);
reactor(1).stable = true;
reactor(1).C_product = [0.5, 0.75, 1.0];  % Concentrations over time

% Reactor 2
reactor(2).name = "CSTR B";
reactor(2).temperature = 375;
reactor(2).pressure_inlet = 6;
reactor(2).C_inlet = 2.0;
reactor(2).time_reaction = hours(2);
reactor(2).start_time = datetime(2024, 3, 15, 9, 0, 0);
reactor(2).stable = true;
reactor(2).C_product = [0.6, 0.85, 1.05];

% Reactor 3
reactor(3).name = "PFR";
reactor(3).temperature = 360;
reactor(3).pressure_inlet = 5.5;
reactor(3).C_inlet = 2.0;
reactor(3).time_reaction = hours(1.5);
reactor(3).start_time = datetime(2024, 3, 15, 11, 0, 0);
reactor(3).stable = false;  % Unstable operation
reactor(3).C_product = [0.4, 0.6, 0.8];

% Access and display data
fprintf("Reactor Analysis\n");
fprintf("================\n\n");
for i = 1:3
    fprintf("Reactor: %s\n", reactor(i).name);
    fprintf("  Temperature: %.0f K\n", reactor(i).temperature);
    fprintf("  Stable: %s\n", string(reactor(i).stable));
    fprintf("  Residence time: %s\n", string(reactor(i).time_reaction));
    fprintf("  Final product concentration: %.2f mol/m³\n\n", reactor(i).C_product(end));
end
```

### Alternative Solution Using a Table

```matlab
% Create table for easier analysis
reactor_table = table(                                              ...
    ["CSTR A"; "CSTR B"; "PFR"],                                    ...
    [350; 375; 360],                                                ...
    [5; 6; 5.5],                                                    ...
    [2.0; 2.0; 2.0],                                                ...
    [0.5; 0.6; 0.4],                                                ...
    [true; true; false],                                            ...
    'VariableNames', {'Reactor', 'Temperature_K', 'Pressure_bar', ...
                      'C_inlet', 'C_product_final', 'Stable'});

disp(reactor_table);

% Calculate average temperature of stable reactors
stable_reactors = reactor_table(reactor_table.Stable == true, :);
mean_T = mean(stable_reactors.Temperature_K);
fprintf("Mean temperature of stable reactors: %.1f K\n", mean_T);
```

---

## Common Mistakes

### Mistake 1: Confusing `()` and `{}` with Cell Arrays

```matlab
% WRONG
data = {"Temperature", 350, "Pressure", 10};
T = data(2);           % Returns a cell {350}, not the number 350
T_value = T + 50;      % ERROR: Can't add to a cell

% CORRECT
data = {"Temperature", 350, "Pressure", 10};
T = data{2};           % Returns 350 (numeric)
T_value = T + 50;      % Works: T_value = 400
```

### Mistake 2: Trying to Add Different Data Types

```matlab
% WRONG
name = "Reactor A";
temperature = 350;
result = name + temperature;  % ERROR: Can't add string and number

% CORRECT
name = "Reactor A";
temperature = 350;
% Store in separate variables or use a structure
reactor.name = name;
reactor.temperature = temperature;
```

### Mistake 3: Forgetting to Specify Data Type Explicitly

```matlab
% This works but is ambiguous
x = "42";    % Is this text or a number?

% Better: Be explicit about intent
temperature_string = "350";          % Clearly text
temperature_double = double(temperature_string);  % Clearly numeric
```

### Mistake 4: Assuming All Rows in a Structure Array Have Identical Fields

```matlab
% WRONG ASSUMPTION: Structure arrays are like tables
reactor(1).temperature = 350;
reactor(2).temperature = 375;

% If you later do:
reactor(3).pressure = 10;  % Added only to reactor 3
% Later access to reactor(1).pressure will fail!

% BETTER: Use a table if all rows should have the same fields
% OR ensure all structure instances have the same fields
reactor(3).temperature = 360;  % Add temperature to maintain consistency
```

### Mistake 5: Using `datetime` Without Units

```matlab
% WRONG: Unclear what the numbers represent
t = datetime(350, 10, 2);  % Is this 350 AD?

% CORRECT: Use a realistic date
t = datetime(2024, 3, 15, 14, 30, 0);  % Year, Month, Day, Hour, Minute, Second
```

---

## Key Takeaways

| Concept | When to Use | Example |
|---------|-------------|---------|
| `double` | Default numeric type | `T = 350;` |
| `single` | Memory-constrained applications | Rarely used in engineering |
| `logical` | True/false conditions, masks | `is_safe = T < T_limit;` |
| `string` | Text, names, descriptions | `compound = "methanol";` |
| `datetime` | Timestamps, experiment times | `start = datetime(2024, 3, 15, 10, 0, 0);` |
| `duration` | Time intervals | `run_time = hours(2);` |
| Cell array `{}` | Mixed types, maximum flexibility | `data = {"Name", 350, true};` |
| Structure | Organized data with field names | `reactor.temperature = 350;` |
| Table | Rectangular datasets (rows × columns) | `reactor_data = table(names, temps, pressures);` |
| Sparse array | Large matrices with mostly zeros | Solving large linear systems |

---



## Key MATLAB Syntax Reference

| Purpose | MATLAB Syntax |
|---------|---------------|
| Create a number (double) | `x = 42;` |
| Create a string | `s = "text";` |
| Create a logical value | `flag = true;` |
| Check data type | `class(x)` or `isa(x, 'double')` |
| Create datetime | `t = datetime(2024, 3, 15, 10, 0, 0);` |
| Create duration | `d = hours(2);` or `duration(0, 2, 30);` |
| Create cell array | `c = {1, "text", [1 2 3]};` |
| Access cell contents | `value = c{1};` |
| Create structure | `s.field = value;` |
| Create table | `t = table(col1, col2, col3);` |
| Convert to double | `double(x)` or `str2double(x)` |
| Convert to string | `string(x)` or `num2str(x)` |
| Convert to logical | `logical(x)` |
| Display variable info | `whos` |

---

## Homework

### Problem

A pilot-plant experiment recorded the following data for three operating periods of a continuous stirred-tank reactor (CSTR):

| Period | Start time          | End time            | Inlet flow (L/min) | Outlet concentration of A (mol/L) |
|--------|---------------------|---------------------|--------------------|-----------------------------------|
| 1      | 2026-09-01 08:00:00 | 2026-09-01 10:30:00 | 12.5               | 0.85                              |
| 2      | 2026-09-01 10:45:00 | 2026-09-01 13:15:00 | 15.0               | 0.72                              |
| 3      | 2026-09-01 13:30:00 | 2026-09-01 16:00:00 | 10.0               | 0.91                              |

**Required Tasks**

1. Create `datetime` arrays for the start and end times.
2. Compute the duration of each period in hours.
3. Store all information in a single MATLAB **table** with meaningful column names.
4. Calculate the total operating time (sum of the three durations) and the average outlet concentration (weighted by duration is optional; a simple mean is acceptable).
5. Display the table and the two summary results using `fprintf` or `disp`.

### Concepts Being Tested

- Creation and arithmetic with `datetime` / `duration`
- Construction of a `table`
- Basic numeric calculations on table columns
- Clear variable naming and formatted output

### Hints

- You can create datetime values with `datetime(2026,9,1,8,0,0)` or from strings.
- Subtracting two datetime values yields a duration; use `hours(d)` to obtain a numeric value in hours.
- Column names in a table should not contain spaces (use underscores).

<details>
<summary><strong>Detailed Solution — Click to Reveal</strong></summary>

### Concept

The exercise integrates datetime handling, table creation, and simple statistical summary—skills needed whenever you import or organize plant or laboratory data.

### Approach / Idea

1. Build start- and end-time datetime vectors.
2. Compute durations and convert them to hours.
3. Assemble a table containing period ID, times, duration, flow, and concentration.
4. Compute the required summary statistics from the table columns.
5. Print the table and the summaries.

### Syntax

- `datetime(...)` for absolute times
- Subtraction of datetimes → duration
- `hours(duration)` → numeric hours
- `table(...)` with named variables
- Dot notation for column access: `T.Duration_h`

### Syntax Breakdown

```matlab
t_start = datetime([2026 9 1 8 0 0; ...]);  % matrix form also works
dur = t_end - t_start;                      % duration array
h = hours(dur);                             % convert to double
T = table(Period, t_start, t_end, h, ...);  % create table
mean(T.Cout)                                % column statistics
```

### MATLAB Code

```matlab
% CSTR pilot-plant operating periods

Period   = [1; 2; 3];

t_start  = [datetime(2026,9,1, 8, 0,0);
            datetime(2026,9,1,10,45,0);
            datetime(2026,9,1,13,30,0)];

t_end    = [datetime(2026,9,1,10,30,0);
            datetime(2026,9,1,13,15,0);
            datetime(2026,9,1,16, 0,0)];

Duration_h = hours(t_end - t_start);        % hours (double)

Fin_Lmin   = [12.5; 15.0; 10.0];            % L/min
Cout_molL  = [0.85; 0.72; 0.91];            % mol/L

% Create the table
opData = table(Period, t_start, t_end, Duration_h, Fin_Lmin, Cout_molL, ...
    'VariableNames', {'Period','Start','End','Duration_h','Fin_Lmin','Cout_molL'});

% Summary calculations
totalTime_h = sum(opData.Duration_h);
avgCout     = mean(opData.Cout_molL);

% Display
disp(opData)
fprintf('\nTotal operating time = %.2f h\n', totalTime_h);
fprintf('Average outlet concentration = %.3f mol/L\n', avgCout);
```

### Line-by-Line Explanation

- `datetime` vectors are built for the three start and end instants.
- Subtraction produces duration objects; `hours` converts them to numeric hours.
- All columns are collected into a single table with descriptive variable names.
- `sum` and `mean` operate directly on the table columns.
- `disp` shows the formatted table; `fprintf` reports the two scalar results.

### Expected Result

```
Period           Start                    End             Duration_h    Fin_Lmin    Cout_molL
______    ____________________    ____________________    __________    ________    _________
  1       01-Sep-2026 08:00:00    01-Sep-2026 10:30:00        2.5         12.5         0.85
  2       01-Sep-2026 10:45:00    01-Sep-2026 13:15:00        2.5           15         0.72
  3       01-Sep-2026 13:30:00    01-Sep-2026 16:00:00        2.5           10         0.91

Total operating time = 7.50 h
Average outlet concentration = 0.827 mol/L
```

### Engineering Interpretation

Each period lasted 2.5 h, giving a total of 7.5 h of operation. The average outlet concentration of approximately 0.83 mol/L can be used, together with the flow rates, to estimate the amount of reactant converted or to compare the three operating regimes. Storing the data in a table makes further analysis (filtering by flow rate, plotting concentration versus time, exporting to Excel, etc.) straightforward.

</details>


