[README 13.md](https://github.com/user-attachments/files/32074338/README.13.md)
# Session 13 — Recursion & Variable Scope in Python

This notebook covers recursive functions and the difference between **local**
and **global** variable scope in Python.

## Contents

### 1. Reversing a String Recursively
Reverses a string by recursively reversing everything after the first character, then appending the first character to the end.

```python
def reverse_string(s):
    if len(s) <= 1:
        return s
    return reverse_string(s[1:]) + s[0]

print(reverse_string("hello"))
```

**Output:**
```
olleh
```

---

### 2. Summing Playlist Durations Recursively
Totals a list of song durations (in seconds) — similar to how Spotify totals a playlist — using recursion instead of a loop.

```python
def sum_playlist_durations(durations):
    if not durations:
        return 0
    return durations[0] + sum_playlist_durations(durations[1:])

durations = [210, 180, 245, 200]
print(sum_playlist_durations(durations))
```

**Output:**
```
835
```

---

### 3. Local vs Global Variable Scope
Demonstrates that assigning to a variable inside a function creates a **local** variable, even if a global variable with the same name exists — the global one is left untouched.

```python
count = 10

def update_count():
    count = 5
    print('Inside:', count)

update_count()
print('Outside:', count)
```

**Output:**
```
Inside: 5
Outside: 10
```

`count` inside `update_count()` is local to that function since it's assigned there; the global `count` (10) is never modified.

---

### 4. Counting Likes in Nested Replies Recursively
Sums the `'likes'` across a nested structure of Instagram-style posts and replies, where each reply can itself contain further replies.

```python
def count_likes(posts):
    total = 0
    for post in posts:
        total += post.get('likes', 0)
        if 'replies' in post:
            total += count_likes(post['replies'])
    return total


posts = [
    {
        'likes': 25,
        'replies': [
            {
                'likes': 5,
                'replies': [
                    {'likes': 2, 'replies': []}
                ]
            },
            {'likes': 3, 'replies': []}
        ]
    },
    {
        'likes': 10,
        'replies': []
    }
]

print(count_likes(posts))
```

**Output:**
```
45
```

---

### 5. Local vs Global Variable Lifetime
Shows the lifetime of a local variable (`user_status`) versus a global variable (`app_status`) — the local variable only exists while the function is running, while the global variable persists before, during, and after the call.

```python
app_status = "offline"  # global variable

def update_user_status():
    user_status = "online"  # local variable, only exists inside this function
    print("During function call -> user_status (local):", user_status)
    print("During function call -> app_status (global):", app_status)

# Before the function call
print("Before function call -> app_status (global):", app_status)

# Calling the function
update_user_status()

# After the function call
print("After function call -> app_status (global):", app_status)

# Trying to access user_status here would raise a NameError,
# since it only exists inside update_user_status()
try:
    print("After function call -> user_status (local):", user_status)
except NameError as e:
    print("After function call -> user_status (local): Error ->", e)
```

**Output:**
```
Before function call -> app_status (global): offline
During function call -> user_status (local): online
During function call -> app_status (global): offline
After function call -> app_status (global): offline
After function call -> user_status (local): Error -> name 'user_status' is not defined
```

---

## Key Concepts Covered
- **Recursion**: solving a problem by breaking it into smaller subproblems of the same type, with a clear base case (e.g., empty string/list) to stop the recursion.
- **Local scope**: variables assigned inside a function exist only for the duration of that function call.
- **Global scope**: variables defined at the top level of a script persist for the life of the program and are readable (but not writable, without `global`) inside functions.

## Requirements
- Python 3.x
- No external libraries needed

## How to Run
Open `session_13.ipynb` in Jupyter Notebook / JupyterLab and run the cells sequentially, or run the equivalent `.py` scripts from the command line.
