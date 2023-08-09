---
summary: >-
  Another exercise from "Eloquent JavaScript": an implementation of Conway's
  Game of Life using form checkboxes
---

# JavaScript silliness, 2: checkbox Game of Life

This was another task from [*Eloquent JavaScript*][ejs], to get familiar with
form elements: an implementation of [Conway's Game of Life][gol] using
checkboxes.

The state can be manipulated between steps.

::: {#grid}
:::

<button id="next">Next generation</button>

[ejs]: <https://eloquentjavascript.net>
[gol]: <https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life>

<script>
class Grid {
    constructor(fillRate, side) {
        let grid = [];
        for (let y = 0; y < side; ++y) {
            let row = [];
            for (let x = 0; x < side; ++x) {
                row.push(this._box(fillRate > Math.random()));
            }
            grid.push(row);
        }

        this.grid = grid;
        this.side = side;
    }

    static create(elem, fillRate = 0.25, side = 33) {
        elem.style.lineHeight = 0.6;
        let g = new Grid(fillRate, side);
        g.grid.forEach(row => {
            row.forEach(box => elem.appendChild(box));
            elem.appendChild(document.createElement("br"));
        });

        return g;
    }

    _countLiveNeighbours(x, y) {
        let count = 0;
        for (let dy = -1; dy <= 1; ++dy) {
            for (let dx = -1; dx <= 1; ++dx) {
                if (y + dy < 0 || y + dy >= this.side ||
                    x + dx < 0 || x + dx >= this.side ||
                    dx == 0 && dy == 0) {
                    continue;
                }

                count += this.grid[y+dy][x+dx].checked ? 1 : 0;
            }
        }

        return count;
    }

    _box(checked) {
        let box = document.createElement("input");
        box.type = "checkbox";
        box.style.margin = 0;
        box.checked = checked;

        return box;
    }

    update(elem) {
        let newGrid = [];

        for (let y = 0; y < this.side; ++y) {
            let row = [];
            for (let x = 0; x < this.side; ++x) {
                let newChecked = false;
                let curChecked = this.grid[y][x].checked;
                let liveNeighbours = this._countLiveNeighbours(x, y);

                if (curChecked && (liveNeighbours == 2 || liveNeighbours == 3)) {
                    newChecked = true;
                }
                else if (!curChecked && liveNeighbours == 3) {
                    newChecked = true;
                }

                row.push(this._box(newChecked));
            }
            newGrid.push(row);
        }

        elem.innerHTML = "";
        newGrid.forEach(row => {
            row.forEach(box => elem.appendChild(box));
            elem.appendChild(document.createElement("br"));
        });

        this.grid = newGrid;
    }
}

let div = document.getElementById("grid");

// Figure out how many boxes to draw
let cb = document.createElement("input");
cb.type = "checkbox";
cb.style.margin = 0;
div.appendChild(cb);
let n = Math.min(33, Math.floor(div.clientWidth / cb.clientWidth));
cb.remove();

let grid = Grid.create(div, 0.25, n);

document.getElementById("next").addEventListener("click", () => grid.update(div));
</script>
