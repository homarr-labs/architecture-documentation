# GPT Algorithm.py

Original 16 columns

```
abbccccddddeeefg
hbbccccddddeeeij
klllmmmmmnneeeop
qlllmmmmmnnrrrst
ulllmmmmmnnrrrvw
xyz           äö
```

## Version 1

```python
import numpy as np

def reorder_board(original_grid, widget_sizes, new_columns):
    # Step 1: Extract widget order
    widget_order = []
    visited = set()
    for row in original_grid:
        for cell in row:
            if cell != " " and cell not in visited:
                widget_order.append(cell)
                visited.add(cell)

    # Calculate new rows needed for the new grid
    total_area = sum(widget_sizes[w][0] * widget_sizes[w][1] for w in widget_order)
    new_rows = -(-total_area // new_columns)  # Ceiling division

    # Initialize the new grid with empty spaces
    new_grid = [[" " for _ in range(new_columns)] for _ in range(new_rows)]

    # Helper to check if a widget can fit in the grid at a specific position
    def can_place(grid, widget, start_row, start_col):
        h, w = widget_sizes[widget]
        if start_row + h > len(grid) or start_col + w > len(grid[0]):
            return False
        for r in range(h):
            for c in range(w):
                if grid[start_row + r][start_col + c] != " ":
                    return False
        return True

    # Helper to place a widget in the grid
    def place_widget(grid, widget, start_row, start_col):
        h, w = widget_sizes[widget]
        for r in range(h):
            for c in range(w):
                grid[start_row + r][start_col + c] = widget

    # Step 2: Place widgets in new grid
    for widget in widget_order:
        h, w = widget_sizes[widget]
        placed = False
        # Try to place the widget as close as possible to previous widgets
        for r in range(new_rows):
            for c in range(new_columns):
                if can_place(new_grid, widget, r, c):
                    place_widget(new_grid, widget, r, c)
                    placed = True
                    break
            if placed:
                break

    return new_grid

# Example usage
original_grid = [
    "abbccccddddeeefg",
    "hbbccccddddeeeij",
    "klllmmmmmnneeeop",
    "qlllmmmmmnnrrrst",
    "ulllmmmmmnnrrrvw",
    "xyz           äö"
]

widget_sizes = {
    "a": (1, 1),
    "b": (2, 2),
    "c": (2, 4),
    "d": (2, 4),
    "e": (3, 3),
    "f": (1, 1),
    "g": (1, 1),
    "h": (1, 1),
    "i": (1, 1),
    "j": (1, 1),
    "k": (1, 1),
    "l": (3, 3),
    "m": (3, 5),
    "n": (3, 2),
    "o": (1, 1),
    "p": (1, 1),
    "q": (1, 1),
    "r": (2, 3),
    "s": (1, 1),
    "t": (1, 1),
    "u": (1, 1),
    "v": (1, 1),
    "w": (1, 1),
    "x": (1, 1),
    "y": (1, 1),
    "z": (1, 1),
    "ä": (1, 1),
    "ö": (1, 1)
}

new_columns = 10

new_grid = reorder_board(original_grid, widget_sizes, new_columns)

# Display the new grid
for row in new_grid:
    print("".join(row))
```

### Results

**10 Columns**

```
abbcccceee
fbbcccceee
ddddghieee
ddddjklllo
mmmmmplllq
mmmmmslllt
mmmmmnnrrr
uvwxynnrrr
zäö  nn
```

**6 Columns**

```
abbfgh
ibbjko
ccccnn
ccccnn
ddddnn
ddddpq
eeelll
eeelll
eeelll
mmmmms
mmmmmt
mmmmmu
rrrvwx
rrryzä
ö
```

**3 Columns**

```
abb
fbb
eee
eee
eee
ghi
jko
lll
lll
lll
nnp
nnq
nns
rrr
rrr
tuv
wxy
zäö
```

## Version 2

```python
import math

def reorder_board_with_resizing(original_grid, widget_sizes, new_columns):
    # Step 1: Extract widget order
    widget_order = []
    visited = set()
    for row in original_grid:
        for cell in row:
            if cell != " " and cell not in visited:
                widget_order.append(cell)
                visited.add(cell)

    # Adjust widget sizes for the new column count
    adjusted_sizes = {}
    for widget, (height, width) in widget_sizes.items():
        if width > new_columns:
            new_width = new_columns
            scaling_factor = new_width / width
            new_height = math.ceil(height * scaling_factor)
            adjusted_sizes[widget] = (new_height, new_width)
        else:
            adjusted_sizes[widget] = (height, width)

    # Calculate new rows needed for the new grid
    total_area = sum(adjusted_sizes[w][0] * adjusted_sizes[w][1] for w in widget_order)
    new_rows = -(-total_area // new_columns)  # Ceiling division

    # Initialize the new grid with empty spaces
    new_grid = [[" " for _ in range(new_columns)] for _ in range(new_rows)]

    # Helper to check if a widget can fit in the grid at a specific position
    def can_place(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        if start_row + h > len(grid) or start_col + w > len(grid[0]):
            return False
        for r in range(h):
            for c in range(w):
                if grid[start_row + r][start_col + c] != " ":
                    return False
        return True

    # Helper to place a widget in the grid
    def place_widget(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        for r in range(h):
            for c in range(w):
                grid[start_row + r][start_col + c] = widget

    # Step 2: Place widgets in new grid
    for widget in widget_order:
        placed = False
        # Try to place the widget as close as possible to previous widgets
        for r in range(new_rows):
            for c in range(new_columns):
                if can_place(new_grid, widget, r, c):
                    place_widget(new_grid, widget, r, c)
                    placed = True
                    break
            if placed:
                break

    return new_grid

# Example usage
original_grid = [
    "abbccccddddeeefg",
    "hbbccccddddeeeij",
    "klllmmmmmnneeeop",
    "qlllmmmmmnnrrrst",
    "ulllmmmmmnnrrrvw",
    "xyz           äö"
]

widget_sizes = {
    "a": (1, 1),
    "b": (2, 2),
    "c": (2, 4),
    "d": (2, 4),
    "e": (3, 3),
    "f": (1, 1),
    "g": (1, 1),
    "h": (1, 1),
    "i": (1, 1),
    "j": (1, 1),
    "k": (1, 1),
    "l": (3, 3),
    "m": (3, 5),
    "n": (3, 2),
    "o": (1, 1),
    "p": (1, 1),
    "q": (1, 1),
    "r": (2, 3),
    "s": (1, 1),
    "t": (1, 1),
    "u": (1, 1),
    "v": (1, 1),
    "w": (1, 1),
    "x": (1, 1),
    "y": (1, 1),
    "z": (1, 1),
    "ä": (1, 1),
    "ö": (1, 1)
}

new_columns = 10

new_grid = reorder_board_with_resizing(original_grid, widget_sizes, new_columns)

# Display the new grid
for row in new_grid:
    print("".join(row))

```

### Results

**10 Columns**

```
abbcccceee
fbbcccceee
ddddghieee
ddddjklllo
mmmmmplllq
mmmmmslllt
mmmmmnnrrr
uvwxynnrrr
zäö  nn
```

**6 Columns**

```
abbfgh
ibbjko
ccccnn
ccccnn
ddddnn
ddddpq
eeelll
eeelll
eeelll
mmmmms
mmmmmt
mmmmmu
rrrvwx
rrryzä
ö
```

**3 Columns**

```
abb
fbb
ccc
ccc
ddd
ddd
eee
eee
eee
ghi
jko
lll
lll
lll
mmm
mmm
nnp
nnq
nns
rrr
rrr
tuv
wxy
zäö
```

## Version 3

```python
import math

def reorder_board_with_resizing(original_grid, widget_sizes, new_columns):
    # Step 1: Extract widget order
    widget_order = []
    visited = set()
    for row in original_grid:
        for cell in row:
            if cell != " " and cell not in visited:
                widget_order.append(cell)
                visited.add(cell)

    # Adjust widget sizes for the new column count
    adjusted_sizes = {}
    for widget, (height, width) in widget_sizes.items():
        if width > new_columns:
            new_width = new_columns
            scaling_factor = new_width / width
            new_height = math.ceil(height * scaling_factor)
            adjusted_sizes[widget] = (new_height, new_width)
        else:
            adjusted_sizes[widget] = (height, width)

    # Calculate new rows needed for the new grid
    total_area = sum(adjusted_sizes[w][0] * adjusted_sizes[w][1] for w in widget_order)
    new_rows = -(-total_area // new_columns)  # Ceiling division

    # Initialize the new grid with empty spaces
    new_grid = [[" " for _ in range(new_columns)] for _ in range(new_rows)]

    # Helper to check if a widget can fit in the grid at a specific position
    def can_place(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        if start_row + h > len(grid) or start_col + w > len(grid[0]):
            return False
        for r in range(h):
            for c in range(w):
                if grid[start_row + r][start_col + c] != " ":
                    return False
        return True

    # Helper to place a widget in the grid
    def place_widget(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        for r in range(h):
            for c in range(w):
                grid[start_row + r][start_col + c] = widget

    # Fallback placement logic for tight grids
    def fallback_place(grid, widget):
        h, w = adjusted_sizes[widget]
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if can_place(grid, widget, r, c):
                    place_widget(grid, widget, r, c)
                    return True
        return False

    # Step 2: Place widgets in new grid
    for widget in widget_order:
        placed = False
        # Try to place the widget as close as possible to previous widgets
        for r in range(new_rows):
            for c in range(new_columns):
                if can_place(new_grid, widget, r, c):
                    place_widget(new_grid, widget, r, c)
                    placed = True
                    break
            if placed:
                break

        # Use fallback placement if regular placement fails
        if not placed:
            if not fallback_place(new_grid, widget):
                raise ValueError(f"Failed to place widget '{widget}' in the grid")

    return new_grid

# Example usage
original_grid = [
    "abbccccddddeeefg",
    "hbbccccddddeeeij",
    "klllmmmmmnneeeop",
    "qlllmmmmmnnrrrst",
    "ulllmmmmmnnrrrvw",
    "xyz           äö"
]

widget_sizes = {
    "a": (1, 1),
    "b": (2, 2),
    "c": (2, 4),
    "d": (2, 4),
    "e": (3, 3),
    "f": (1, 1),
    "g": (1, 1),
    "h": (1, 1),
    "i": (1, 1),
    "j": (1, 1),
    "k": (1, 1),
    "l": (3, 3),
    "m": (3, 5),
    "n": (3, 2),
    "o": (1, 1),
    "p": (1, 1),
    "q": (1, 1),
    "r": (2, 3),
    "s": (1, 1),
    "t": (1, 1),
    "u": (1, 1),
    "v": (1, 1),
    "w": (1, 1),
    "x": (1, 1),
    "y": (1, 1),
    "z": (1, 1),
    "ä": (1, 1),
    "ö": (1, 1)
}

new_columns = 10

new_grid = reorder_board_with_resizing(original_grid, widget_sizes, new_columns)

# Display the new grid
for row in new_grid:
    print("".join(row))
```

### Results

**10 Columns**

```
abbcccceee
fbbcccceee
ddddghieee
ddddjklllo
mmmmmplllq
mmmmmslllt
mmmmmnnrrr
uvwxynnrrr
zäö  nn
```

**6 Columns**

```
abbfgh
ibbjko
ccccnn
ccccnn
ddddnn
ddddpq
eeelll
eeelll
eeelll
mmmmms
mmmmmt
mmmmmu
rrrvwx
rrryzä
ö
```

**3 Columns**

```
abb
fbb
ccc
ccc
ddd
ddd
eee
eee
eee
ghi
jko
lll
lll
lll
mmm
mmm
nnp
nnq
nns
rrr
rrr
tuv
wxy
zäö
```

## Version 4

```python
import math

def reorder_board_with_dynamic_rows(original_grid, widget_sizes, new_columns):
    # Step 1: Extract widget order
    widget_order = []
    visited = set()
    for row in original_grid:
        for cell in row:
            if cell != " " and cell not in visited:
                widget_order.append(cell)
                visited.add(cell)

    # Adjust widget sizes for the new column count
    adjusted_sizes = {}
    for widget, (height, width) in widget_sizes.items():
        if width > new_columns:
            new_width = new_columns
            scaling_factor = new_width / width
            new_height = math.ceil(height * scaling_factor)
            adjusted_sizes[widget] = (new_height, new_width)
        else:
            adjusted_sizes[widget] = (height, width)

    # Initialize the new grid with a reasonable starting number of rows
    initial_rows = -(-sum(adjusted_sizes[w][0] * adjusted_sizes[w][1] for w in widget_order) // new_columns)
    new_grid = [[" " for _ in range(new_columns)] for _ in range(initial_rows)]

    # Helper to check if a widget can fit in the grid at a specific position
    def can_place(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        if start_row + h > len(grid) or start_col + w > len(grid[0]):
            return False
        for r in range(h):
            for c in range(w):
                if grid[start_row + r][start_col + c] != " ":
                    return False
        return True

    # Helper to place a widget in the grid
    def place_widget(grid, widget, start_row, start_col):
        h, w = adjusted_sizes[widget]
        for r in range(h):
            for c in range(w):
                grid[start_row + r][start_col + c] = widget

    # Helper to expand the grid by adding more rows
    def expand_grid(grid, additional_rows):
        for _ in range(additional_rows):
            grid.append([" " for _ in range(new_columns)])

    # Step 2: Place widgets in new grid
    for widget in widget_order:
        placed = False
        while not placed:
            for r in range(len(new_grid)):
                for c in range(new_columns):
                    if can_place(new_grid, widget, r, c):
                        place_widget(new_grid, widget, r, c)
                        placed = True
                        break
                if placed:
                    break
            # If the widget couldn't be placed, expand the grid
            if not placed:
                expand_grid(new_grid, 1)

    return new_grid

# Example usage
original_grid = [
    "abbccccddddeeefg",
    "hbbccccddddeeeij",
    "klllmmmmmnneeeop",
    "qlllmmmmmnnrrrst",
    "ulllmmmmmnnrrrvw",
    "xyz           äö"
]

widget_sizes = {
    "a": (1, 1),
    "b": (2, 2),
    "c": (2, 4),
    "d": (2, 4),
    "e": (3, 3),
    "f": (1, 1),
    "g": (1, 1),
    "h": (1, 1),
    "i": (1, 1),
    "j": (1, 1),
    "k": (1, 1),
    "l": (3, 3),
    "m": (3, 5),
    "n": (3, 2),
    "o": (1, 1),
    "p": (1, 1),
    "q": (1, 1),
    "r": (2, 3),
    "s": (1, 1),
    "t": (1, 1),
    "u": (1, 1),
    "v": (1, 1),
    "w": (1, 1),
    "x": (1, 1),
    "y": (1, 1),
    "z": (1, 1),
    "ä": (1, 1),
    "ö": (1, 1)
}

new_columns = 10

new_grid = reorder_board_with_dynamic_rows(original_grid, widget_sizes, new_columns)

# Display the new grid
for row in new_grid:
    print("".join(row))
```

### Results

**10 Columns**

```
abbcccceee
fbbcccceee
ddddghieee
ddddjklllo
mmmmmplllq
mmmmmslllt
mmmmmnnrrr
uvwxynnrrr
zäö  nn
```

**6 Columns**

```
abbfgh
ibbjko
ccccnn
ccccnn
ddddnn
ddddpq
eeelll
eeelll
eeelll
mmmmms
mmmmmt
mmmmmu
rrrvwx
rrryzä
ö
```

**3 Columns**

```
abb
fbb
ccc
ccc
ddd
ddd
eee
eee
eee
ghi
jko
lll
lll
lll
mmm
mmm
nnp
nnq
nns
rrr
rrr
tuv
wxy
zäö
```

## Results with sidebar dynamic section

**Input**

```
original_grid = [
    "abbccccddddeeeff",
    "hbbccccddddeeeff",
    "klllmmmmmnneeeff",
    "qlllmmmmmnnrrrff",
    "ulllmmmmmnnrrrff",
    "xyz           ff"
]

widget_sizes = {
    "a": (1, 1),
    "b": (2, 2),
    "c": (2, 4),
    "d": (2, 4),
    "e": (3, 3),
    "f": (1, 1),
    "h": (1, 1),
    "k": (1, 1),
    "l": (3, 3),
    "m": (3, 5),
    "n": (3, 2),
    "q": (1, 1),
    "r": (2, 3),
    "u": (1, 1),
    "x": (1, 1),
    "y": (1, 1),
    "z": (1, 1)
}
```

**10 Columns**

```
abbcccceee
hbbcccceee
ddddffkeee
ddddfflllq
nnuxffllly
nnz fflll
nn  ffrrr
    ffrrr
mmmmm
mmmmm
mmmmm
```

**6 Columns**

```
abbhff
kbbqff
ccccff
ccccff
ddddff
ddddff
eeelll
eeelll
eeelll
mmmmmu
mmmmmx
mmmmmy
nnrrrz
nnrrr
nn
```

**3 Columns**

```
abb
hbb
ccc
ccc
ddd
ddd
eee
eee
eee
ffk
ffq
ffu
ffx
ffy
ffz
lll
lll
lll
mmm
mmm
nn
nn
nn
rrr
rrr
```
