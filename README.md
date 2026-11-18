# ENCODEX × FluxNum GENIUS — 64-Bit Compact Edition

Compact browser GUI + separate localhost Python backend, based on ENCODEX/FluxNum GENIUS Studio v3. The GUI is a **single self-contained HTML file** (~55 KB); its CSS and JavaScript are embedded, so there are no CDN dependencies. The backend is separate and optional.

> **Architecture note:** HTML is not itself a 32-bit or 64-bit executable. This project **performs exact 64-bit unsigned/signed integer operations** using browser `BigInt` and Python integer masking. It runs on modern browsers regardless of the operating system's word size, including 64-bit Windows/macOS/Linux. Physical USB-C hardware requires a separately programmable USB-C microcontroller.

## Open the standalone HTML GUI (offline mode)

Open `frontend/index.html` in Chrome, Edge, Safari or Firefox. No install or server is required. Features available offline: all GUI sections, LENS local sample index, FORGE grounded draft, basic analogy commands, 64-bit word operations, FluxNum v1 codec, simulated board routing, temporary offline classifier, simulated waves/InterSphere viewport, image/video preview, code templates and downloadable JSON checkpoints.

Offline mode **does not** persist backend training records or remotely search documents. No large language model or Internet search is bundled. On mobile Safari, file-opening behavior may vary; a normal web server or a Files-compatible browser may be necessary.

## Start the separate Python backend (persistent mode)

Requires **Python 3.10 or later**. This Python API uses the standard library; `pip install` is not needed.

### Windows
Double-click `start_windows.bat` in the extracted project folder, or in a terminal at the project root run:

```powershell
py -3 -m backend.app --port 8765
```

### macOS / Linux
Run `./start.sh`, or in a terminal from the project root:

```bash
python3 -m backend.app --port 8765
```

Then open **http://127.0.0.1:8765/frontend/index.html** in a browser on the same computer. A green `PYTHON BACKEND` indicator confirms connected mode. If port 8765 is occupied, use `--port 8876` and visit `http://127.0.0.1:8876/frontend/index.html`.

The server accepts only localhost binding. `backend/app.py` serves the GUI and specific API routes; the other backend source and local `data/` files are not web-browsable.

## Files

```text
frontend/index.html             runnable HTML/CSS/JavaScript GUI, offline or API mode
backend/app.py                 localhost JSON API and HTML server
backend/engine.py              LENS retrieval, FORGE drafts, commands, training, save points
backend/trainer.py             Naive Bayes text classifier
backend/int64.py               exact 64-bit numeric transforms (strings over JSON)
firmware/*.py                  compatible CircuitPython USB-C microcontroller firmware
clients/                       Java, Node JavaScript, C, C++, C#, Objective-C examples
examples/genius_terms.gtl      analogy command examples
docs/                          project/hardware integration references
preview/*.jpg                  actual GUI JPEG captures
start.sh / start_windows.bat    local backend launchers
tests/                          Python codec, engine, and uint64 tests
```

## Features and functional limits

| Component | Available now | Requires additional hardware/SDK |
|---|---|---|
| Exact 64-bit numbers | unsigned/signed, hex/binary, XOR, rotate, modular addition | No |
| Numerical codec | original FluxNum v1, bases 2/8/10/16, checksum and nonce-shuffle | USB link requires suitable RP2040 firmware/transport |
| GENIUS command terms | LENS, FORGE, MIRROR, FLUX, TRAIN, CLASSIFY, ROUTE, SNAP | Full language compiler not included |
| Search and machine learning | local indexed docs, small Naive Bayes classifier, persisted examples with backend | Internet/LLM use not included |
| Logic board and haptics | virtual JSON signal router, waveform monitor | Actual sensor/actuator integration not included |
| InterSphere | 2D levitating-city canvas and moving ship | 3D, physics and STL processing not included |
| IDE | source editing and export for 9 language choices | Native compilers/SDKs required |
| Save points | JSON export/import, persistent backend checkpoints | No cloud sync |

The randomized shuffled codec and XOR word processing are reversible numeric formats, **not cryptographic protection**. Board routing is strictly a **simulation**; it does not control voltage, power delivery, lasers, motors, or real robots.

## APIs

- `GET /api/health`
- `GET /api/search?q=robotics`
- `POST /api/generate` — `{ "prompt": "usb-c optical sensor" }`
- `POST /api/command` — `{ "expression": "FLUX 3 5 8" }`
- `POST /api/int64` — `{ "value":"0xFEDCBA9876543210", "operation":"xor", "key":"0x123456789ABCDEF0", "shift":13 }`
- `POST /api/codec` — `{ "operation":"encode", "data":"hello", "base":16 }`
- `POST /api/numeric`, `/api/train`, `/api/classify`, `/api/route`, `/api/snap`
- `GET /api/events`, `/api/checkpoints`

All uint64 values are sent/received as **strings**. Valid `operation` values are `xor`, `rotate`, `add`. An omitted key uses a new random uint64 mask for `xor` or `add`; rotation does not need a key.

## Tests performed

- 19 Python unit tests passed, including the original cross-language C codec test.
- Browser interaction smoke tests passed for offline search, design draft, exact 64-bit operations, codec, route, ML, IDE, save state, and mobile-width layout.
- All four FluxNum bases tested for JavaScript ↔ Python compatibility.
- XOR, rotation, and modular addition tested for JavaScript ↔ Python exact result agreement.
- Local HTTP API endpoints tested with Python standard-library HTTP requests.

The C#, Objective-C, and native mobile clients were not newly compiled for this compact edition; their provided files are integration examples.
