# Circuit Lab

A drag-and-drop DC circuit simulator. Build circuits from batteries, wires,
resistors, capacitors, bulbs, switches and ammeters, then measure voltage,
current and resistance anywhere with a draggable multimeter.

## Running

No build step or dependencies. Serve the folder with any static server, e.g.

```sh
npx http-server .
```

and open `index.html` (opening the file directly also works in most browsers).

## Using it

- **Add parts**: drag them from the left palette onto the board (or click to drop one in the middle).
- **Wire**: drag from any terminal or empty grid point. Parts connect where their terminals meet.
- **Edit**: click a part to select it. Drag its end handles to stretch it and edit values in the inspector.
  `R` rotates, `F` flips (battery polarity), `Del` deletes, and double-click toggles a switch.
- **Measure**:
  - Hover any part to see its voltage, current, resistance and power.
  - The multimeter probes snap to terminals.
    - **V** reads the voltage of red relative to black.
    - **A** puts the meter *in* the circuit as a ~0 Ω path, just like a real ammeter. Connect it in series, or you'll short something.
    - **Ω** measures the resistance between the probes, with batteries treated as switched off.
- **Capacitors** charge and discharge over time. Use the **Time** control (paused, 0.01× to 10×) to slow
  fast RC circuits down or speed slow ones up. Select a capacitor to see its charge and stored energy, or to
  discharge it. Its plates show red (+) and blue (−) when charged.

The circuit, including capacitor charge, is saved to `localStorage` automatically.

## How it works

`src/circuit.js` is a nodal-analysis DC solver. Every part is a conductance, and
batteries also carry an EMF (a Norton equivalent). Wires are 0.1 mΩ, so the
system is always solvable, even with shorts or parallel batteries. Capacitors
are integrated with backward Euler: each time step, a capacitor becomes a C/Δt
conductance in series with its previous voltage. The time step is at most
1 ms, with several steps per animation frame. `src/app.js` handles canvas
rendering and interaction.

## Tests

```sh
npm test
```

## License

Released under the [MIT License](LICENSE). You're free to use, copy, modify,
and distribute this project, including commercially, as long as the copyright
notice and license text are kept.
