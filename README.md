# Game Of Life - Custom Version

This is my custom version of the "Game Of Life" simulation. In this code, I have implemented the following features:

## Requirements

1. **Real-Time Simulation**
   - Make the simulation real-time.

2. **Pause/Resume Logic**
   - Add logic to pause and resume the simulation.

3. **Save/Load Logic**
   - Implement the ability to save and load the game state.

## Implementation Details

### 1. Real-Time Simulation

To achieve real-time simulation, the following code snippet has been implemented:

```python
running = True
paused = False
tick_interval = 0.5  # Time interval between ticks in seconds
last_tick = pygame.time.get_ticks()  # Time tracking for simulation

while running:
    current_time = pygame.time.get_ticks()
    delta_time = (current_time - last_tick) / 1000.0

    if not paused and delta_time >= tick_interval:
        last_tick = current_time
        next_generation()

    # Additional code for rendering the simulation
    screen.fill(white)
    draw_grid()
    draw_cells()
    draw_button()
    draw_info_text()
    pygame.display.flip()
```
### 2. Pause/Resume Logic
he pause/resume logic is implemented as follows:
# button
```python
button_width, button_height = 200, 50
button_x, button_y = (width - button_width) // 2, height - button_height - 10

def draw_button():
    pygame.draw.rect(screen, green, (button_x, button_y, button_width, button_height))
    font = pygame.font.Font(None, 36)
    text = font.render("Start/Stop", True, black)
    text_rect = text.get_rect(center=(button_x + button_width // 2, button_y + button_height // 2))
    screen.blit(text, text_rect)

def toggle_pause():
    global paused
    paused = not paused

# Event handling
if event.type == pygame.MOUSEBUTTONDOWN:
    if button_x <= event.pos[0] <= button_x + button_width and button_y <= event.pos[1] <= button_y + button_height:
        toggle_pause()
    else:
        x, y = event.pos[0] // cell_width, event.pos[1] // cell_height
        game_state[x, y] = not game_state[x, y]

```
### 3. Save/Load Logic
The save/load logic is implemented as follows:
```python
# info
def draw_info_text():
    font = pygame.font.Font(None, 25)
    text = font.render("CTRL+S to Save / CTRL+L to Load game", True, black)
    text_rect = text.get_rect(center=(width // 2, height - button_height - 30))

    rect_width = text_rect.width + 20
    rect_height = text_rect.height + 10
    rect_x = (width - rect_width) // 2
    rect_y = height - button_height - 40

    pygame.draw.rect(screen, green, (rect_x, rect_y, rect_width, rect_height))
    screen.blit(text, text_rect)

# Logic for saving and loading
def save_game_state(filename):
    with open(filename, 'wb') as file:
        pickle.dump(game_state, file)

def load_game_state(filename):
    global game_state
    with open(filename, 'rb') as file:
        game_state = pickle.load(file)

if event.type == pygame.KEYDOWN:
    if event.key == pygame.K_s and pygame.key.get_mods() & pygame.KMOD_CTRL:
        save_game_state("save.pickle")
    elif event.key == pygame.K_l and pygame.key.get_mods() & pygame.KMOD_CTRL:
        load_game_state("save.pickle")
```
