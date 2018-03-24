# MycroftDisplay

Convert a pixel-art version of the Mark 1's display into the code needed to display it, and back again.

## General usage

`from MycroftDisplay import Mark1`

`Mark1.from_grid(grid, space='.', fill='#')`

Takes an ASCII-art image of the Mycroft display, and converts it to an `img_code` which can then be given to `enclosure.mouth_display()`.

Example:

```
img_code = Mark1.from_grid('''
        ................
        ................
        #...#....#..###.
        ##.##...#.#..#..
        #.#.#...###..#..
        #...#...#.#..#..
        #...#.#.#.#.###.
        ................
''')

=> "QIMHIAABIAMHAAAEAAIHEBIHAAEEMHEEAA"
```

Whitespace (other than newlines) is ignored in grids, so that they can be indented in source code without problems. Blank lines are also ignored.

The `space` and `fill` characters dictate what the grid uses. `from_grid` will throw an error if the supplied grid contains characters other than the space and fill characters (and newlines and whitespace).

`Mark1.to_grid(img_code, space='.', fill='#')`

Does the reverse, converting an `img_code` to an ASCII-art grid.

```

grid = Mark1.to_grid("QIMHIAABIAMHAAAEAAIHEBIHAAEEMHEEAA", fill='█',space='·')

=> '''
················
················
█···█····█··███·
██·██···█·█··█··
█·█·█···███··█··
█···█···█·█··█··
█···█·█·█·█·███·
················
'''
```

Unfortunately, `enclosure.mouth_display()` can only cope with grids of size 16x8. The Mark 1 display is 32x8. So, if you need to work with a larger grid, use `Mark1.from_large_grid` and `Mark1.to_large_grid`.

`Mark1.from_large_grid` returns a list of `(grid, x, y)` tuples, so that one can do:

```
for img_code, x, y in Mark1.from_large_grid(my_ascii_art_grid):
    self.enclosure.mouth_display(img_code, x=x, y=y)
```

`Mark1.to_large_grid` converts this list of tuples back into ASCII.

## Utilities

`MycroftDisplay.utils.insert_grid(base, sub, x, y)` composites one grid onto another, like HTML5's canvas.drawImage. Pass a `base` grid, a `sub` grid, and `x` and `y` coordinates and the sub grid will be overlaid on the base grid at those coordinates. Spaces in the sub grid are treated as transparent.

`MycroftDisplay.utils.normalise_grid(grid)` removes extraneous whitespace and indents and tidies up the grid. Use this if you need to compare grid values for some reason.

