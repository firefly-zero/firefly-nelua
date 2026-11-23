# firefly-nelua

The Nelua SDK for making [Firefly Zero](https://fireflyzero.com/) games.

Nelua is a statically typed and compiled language the goal of which is to be as close as possible to Lua. If you are a Lua developer or looking into porting a Lua game to Firefly, Nelua is probably the best choice for you.

* [▶️ getting started](https://docs.fireflyzero.com/dev/getting-started/)
* [🐙 github](https://github.com/firefly-zero/firefly-nelua)

## Installation

Use firefly CLI to create a new Nelua project:

```bash
ff new --lang=nelua hello-world
```

To update, replace `vendor/firefly.nelua` with the latest version from this repository.

## Usage

Import:

```lua
local ff = require 'vendor/firefly'
```

Graphics:

```lua
ff.set_color(ff.Color.Red, {r=255, g=0, b=0})

ff.clear_screen(ff.Color.Blue)

ff.flip_x()
ff.flip_y()

ff.draw_point({x=10, y=20}, ff.Color.Black)

ff.draw_line(
    {x=10, y=20}, {x=30, y=40},
    {color=ff.Color.Red, w=1},
)

local style = {
    fill_color=ff.Color.Red,
    stroke_color=ff.Color.Blue,
    stroke_width=1,
}
local point = {x=10, y=20}
local size = {w=30, h=40}

ff.draw_rect(point, size, style)

local corner = {w=4, h=4}
ff.draw_rounded_rect(point, size, corner, style)

ff.draw_circle(point, 10, style)

ff.draw_ellipse(point, size, style)

ff.draw_triangle(
    {x=40, y=20},
    {x=30, y=40},
    {x=50, y=40},
    style,
)

local diameter = 10;
ff.draw_arc(
    point,
    diameter,
    -- start and sweep angles
    ff.radians(0),
    ff.radians(math.pi / 2),
    style,
)

ff.draw_sector(
    point,
    diameter,
    -- start and sweep angles
    ff.radians(0),
    ff.radians(math.pi / 2),
    style,
)

local font = ff.load_file("font").to_font()
ff.draw_text("oh hi mark", font, point, ff.Color.Black)

ff.draw_qr(
    "https://fireflyzero.com/",
    point, ff.Color.Black, ff.Color.White,
)
```

Multiplayer:

```lua
local peer = ff.get_me()

local peers = ff.get_peers().to_span()
for i = 1, #peers do
    peer = peers[i]
end
```

Input:

```lua
local pad, touched = ff.read_pad(peer)
if touched then
    {pad.x, pad.y}
end

local btns = ff.read_buttons(peer)
{btns.s, btns.e, btns.w, btns.n}

-- Read combined inputs.
pad = ff.read_pad(ff.Peer.combined)
btns = ff.read_buttons(ff.Peer.combined)
```

Filesystem:

```lua
local file = ff.load_file("some-file").to_bytes()

ff.dump_file("font", file)

ff.remove_file("font")
```

Menu:

```lua
local menu_id = ff.MenuItem{1}
ff.add_menu_item(menu_id, "intentory")
ff.remove_menu_item(menu_id)
ff.open_menu()
```

Boards (scoreboards) and badges (achievements):

```lua
local badge1 = ff.Badge{1}
local badge2 = ff.Badge{2}

local progress = ff.get_progress(peer, badge1)
{progress.done, progress.goal}

progress = ff.add_progress(peer, badge1, 1)

local board1 = ff.Board{1}
local board2 = ff.Board{2}

local best_score = ff.get_score(peer, board1)
best_score = ff.add_score(peer, board1, 10)
```

Misc:

```lua
ff.log_debug("let's go!")

ff.log_error("oh no...")

ff.set_seed(13)

local seconds_since_start = ff.get_time()

local rand_val = ff.get_random()

local name = ff.get_name(peer)

local settings = ff.get_settings(peer)
if settings.language == "en" then
    -- ...
end

ff.restart()
ff.quit()
```

## License

MIT License. You can do whatever you want with the SDK, modify it, embed into any apps and games. Have fun!
