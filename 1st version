import sys
from collections import deque
from typing import Optional
 
def debug(*args):
    print(*args, file=sys.stderr, flush=True)
 
# ── Input parsing ────────────────────────────────────────────────────────────
 
def read_init():
    my_id = int(input())
    width = int(input())
    height = int(input())
    grid = []
    for _ in range(height):
        grid.append(input())
    snakebots_per_player = int(input())
    my_ids = [int(input()) for _ in range(snakebots_per_player)]
    enemy_ids = [int(input()) for _ in range(snakebots_per_player)]
    return my_id, width, height, grid, my_ids, enemy_ids
 
def read_turn():
    power_count = int(input())
    energies = set()
    for _ in range(power_count):
        x, y = map(int, input().split())
        energies.add((x, y))
    snakebot_count = int(input())
    snakebots = {}
    for _ in range(snakebot_count):
        line = input().split()
        sid = int(line[0])
        body_str = line[1]
        body = []
        for coord in body_str.split(':'):
            cx, cy = map(int, coord.split(','))
            body.append((cx, cy))
        snakebots[sid] = body
    return energies, snakebots
 
# ── Geometry helpers ─────────────────────────────────────────────────────────
 
DIRECTIONS = {
    'UP':    (0, -1),
    'DOWN':  (0,  1),
    'LEFT':  (-1, 0),
    'RIGHT': ( 1, 0),
}
 
def move(pos, d):
    return (pos[0] + d[0], pos[1] + d[1])
 
def in_bounds(x, y, width, height):
    return 0 <= x < width and 0 <= y < height
 
def is_platform(x, y, grid):
    if not in_bounds(x, y, len(grid[0]), len(grid)):
        return False
    return grid[y][x] == '#'
 
def solid_cells(grid, snakebots, width, height):
    """All cells that count as solid (platforms + all snake bodies + energy)."""
    solids = set()
    for y in range(height):
        for x in range(width):
            if grid[y][x] == '#':
                solids.add((x, y))
    for body in snakebots.values():
        for seg in body:
            solids.add(seg)
    return solids
 
# ── BFS to find nearest energy ───────────────────────────────────────────────
 
def bfs_to_energy(start, energies, grid, all_body_cells, width, height):
    """Return (direction_name, distance) toward the nearest energy."""
    if not energies:
        return None, 999
 
    visited = {start}
    queue = deque()
    # Seed with initial moves
    for dname, dv in DIRECTIONS.items():
        nx, ny = move(start, dv)
        if not in_bounds(nx, ny, width, height):
            continue
        if (nx, ny) in all_body_cells and (nx, ny) not in energies:
            continue
        if is_platform(nx, ny, grid):
            continue
        queue.append((nx, ny, dname, 1))
        visited.add((nx, ny))
 
    while queue:
        x, y, first_dir, dist = queue.popleft()
        if (x, y) in energies:
            return first_dir, dist
        for dv in DIRECTIONS.values():
            nx, ny = move((x, y), dv)
            if not in_bounds(nx, ny, width, height):
                continue
            if (nx, ny) in visited:
                continue
            if (nx, ny) in all_body_cells and (nx, ny) not in energies:
                continue
            if is_platform(nx, ny, grid):
                continue
            visited.add((nx, ny))
            queue.append((nx, ny, first_dir, dist + 1))
 
    return None, 999
 
# ── Safety check: will this move immediately kill us? ────────────────────────
 
def is_safe_direction(head, dname, grid, all_body_cells, energies, width, height):
    dx, dy = DIRECTIONS[dname]
    nx, ny = head[0] + dx, head[1] + dy
    # Going out of left/right is risky — we'll lose the head but still survive
    # Going out of bottom (fall off) is fatal
    if ny >= height:
        return False
    # Hitting platform or own/enemy body (not energy) loses the head
    # Only fatal if snake < 3 parts — caller handles that; here we just
    # flag walls and OOB bottom as unsafe
    if is_platform(nx, ny, grid):
        return False  # head loss
    if (nx, ny) in all_body_cells and (nx, ny) not in energies:
        return False  # head loss
    return True
 
def pick_safe_fallback(head, current_dir, grid, all_body_cells, energies, width, height):
    """Return any direction that doesn't immediately kill or lose a head."""
    for dname in [current_dir, 'UP', 'LEFT', 'RIGHT', 'DOWN']:
        if dname and is_safe_direction(head, dname, grid, all_body_cells, energies, width, height):
            return dname
    return current_dir or 'UP'
 
# ── Gravity simulation: will the snake fall off after this move? ─────────────
 
def simulate_fall(body, grid, snakebots_copy, width, height):
    """Rough check: after a move, does gravity leave the snake in-bounds?"""
    # Find min y-position of body; if all above ground they'll fall
    # This is a heuristic, not a full simulation
    for seg in body:
        x, y = seg
        below = (x, y + 1)
        if in_bounds(*below, width, height):
            if is_platform(*below, grid):
                return False  # at least one seg has solid below
            if below in {s for b in snakebots_copy.values() for s in b}:
                return False
        else:
            # seg is at bottom row — that itself is ground
            return False
    return True  # might fall
 
 
# ── Main strategy ─────────────────────────────────────────────────────────────
 
def choose_direction(sid, body, energies, grid, snakebots, my_ids, enemy_ids,
                     width, height, last_dirs):
    head = body[0]
    length = len(body)
 
    all_body_cells = {seg for b in snakebots.values() for seg in b}
 
    # BFS toward nearest energy
    best_dir, dist = bfs_to_energy(head, energies, grid, all_body_cells, width, height)
 
    current_dir = last_dirs.get(sid, 'UP')
 
    if best_dir and is_safe_direction(head, best_dir, grid, all_body_cells, energies, width, height):
        return best_dir
 
    # Fallback: pick safest direction
    return pick_safe_fallback(head, current_dir, grid, all_body_cells, energies, width, height)
 
 
# ── Main loop ─────────────────────────────────────────────────────────────────
 
def main():
    my_id, width, height, grid, my_ids, enemy_ids = read_init()
    last_dirs = {}  # sid -> last direction given
 
    while True:
        energies, snakebots = read_turn()
 
        actions = []
 
        for sid in my_ids:
            if sid not in snakebots:
                continue  # snake is dead
 
            body = snakebots[sid]
            direction = choose_direction(
                sid, body, energies, grid, snakebots,
                my_ids, enemy_ids, width, height, last_dirs
            )
            last_dirs[sid] = direction
            actions.append(f"{sid} {direction}")
 
        if not actions:
            actions.append("WAIT")
 
        print(';'.join(actions), flush=True)
 
 
if __name__ == '__main__':
    main()
