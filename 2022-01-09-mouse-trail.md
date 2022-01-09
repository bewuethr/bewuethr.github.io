# JavaScript silliness, 1: mouse trail

I have *finally* finished reading the excellent [Eloquent JavaScript][ejs], and
some of the exercises were fun enough that I don't want them to just collect
dust in GitHub.

First up: mouse trails! This was in the chapter about event handling.

It models two masses attached to the mouse cursor with (invisible) springs; `c`
is the damping coefficient.

```javascript
function updateMouse(event) {
    mouse = {
        x: event.clientX + window.scrollX,
        y: event.clientY + window.scrollY
    };
}

function animate(time, lastTime) {
    if (lastTime != null) {
        let dt = (time - lastTime) / 1000;

        for (let i = 0; i < dots.length; ++i) {
            // Calculate spring force
            let FX = mouse.x - dots[i].x;
            let FY = mouse.y - dots[i].y;

            // Add frictional force
            FX -= dots[i].c * dots[i].vX;
            FY -= dots[i].c * dots[i].vY;

            // Calculate acceleration
            let aX = FX / dots[i].m;
            let aY = FY / dots[i].m;

            // Update velocity
            dots[i].vX += dt * aX;
            dots[i].vY += dt * aY;

            // Update position
            dots[i].x += dt * dots[i].vX;
            dots[i].y += dt * dots[i].vY;
            divs[i].style.left = (dots[i].x - dots[i].r) + "px";
            divs[i].style.top = (dots[i].y - dots[i].r) + "px";
        }
    }

    requestAnimationFrame(newTime => animate(newTime, time));
}

let mouse = {x: 0, y: 0};
let dots = [
    {x: 0, y: 0, vX: 0, vY: 0, m: 0.1, c: 0.3, r: 3, col: "teal"},
    {x: 0, y: 0, vX: 0, vY: 0, m: 1, c: 0.5, r: 6, col: "red"}
];

let divs = [];
for (let dot of dots) {
    let d = document.body.appendChild(document.createElement("div"));
    d.className = "trail";
    d.style.height = 2 * dot.r + "px";
    d.style.width = 2 * dot.r + "px";
    d.style.borderRadius = dot.r + "px";
    d.style.background = dot.col;
    divs.push(d);
}

window.addEventListener("mousemove", updateMouse);
requestAnimationFrame(animate);
```

[ejs]: <https://eloquentjavascript.net>

<style>
.trail {
    position: absolute;
    height: 6px; width: 6px;
    border-radius: 3px;
    background: teal;
}
</style>

<script>
function updateMouse(event) {
    mouse = {
        x: event.clientX + window.scrollX,
        y: event.clientY + window.scrollY
    };
}

function animate(time, lastTime) {
    if (lastTime != null) {
        let dt = (time - lastTime) / 1000;

        for (let i = 0; i < dots.length; ++i) {
            // Calculate spring force
            let FX = mouse.x - dots[i].x;
            let FY = mouse.y - dots[i].y;

            // Add frictional force
            FX -= dots[i].c * dots[i].vX;
            FY -= dots[i].c * dots[i].vY;

            // Calculate acceleration
            let aX = FX / dots[i].m;
            let aY = FY / dots[i].m;

            // Update velocity
            dots[i].vX += dt * aX;
            dots[i].vY += dt * aY;

            // Update position
            dots[i].x += dt * dots[i].vX;
            dots[i].y += dt * dots[i].vY;
            divs[i].style.left = (dots[i].x - dots[i].r) + "px";
            divs[i].style.top = (dots[i].y - dots[i].r) + "px";
        }
    }

    requestAnimationFrame(newTime => animate(newTime, time));
}

let mouse = {x: 0, y: 0};
let dots = [
    {x: 0, y: 0, vX: 0, vY: 0, m: 0.1, c: 0.3, r: 3, col: "teal"},
    {x: 0, y: 0, vX: 0, vY: 0, m: 1, c: 0.5, r: 6, col: "red"}
];

let divs = [];
for (let dot of dots) {
    let d = document.body.appendChild(document.createElement("div"));
    d.className = "trail";
    d.style.height = 2 * dot.r + "px";
    d.style.width = 2 * dot.r + "px";
    d.style.borderRadius = dot.r + "px";
    d.style.background = dot.col;
    divs.push(d);
}

window.addEventListener("mousemove", updateMouse);
requestAnimationFrame(animate);
</script>
