### Notes:

- Init 1-d list: `[0 for _ in range(N)]`
- Init 2-d list: `[[0] * col for _ in range(row)]`
- Init array of empty lists: `[[] for _ in range(N)]` = `[[], [], [], [], …]`
- Sorting:
    - By “end time” in list of intervals:`intervals.sort(key=lambda x:x[1])`
    - By value in dictionary: `sorted_x = dict(sorted(d.items(), key=lambda x:x[1])`