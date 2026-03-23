# mouse-fs

Abuses a Logitech mouse's DPI register to store 2 bytes, but just temporarily.

## Usage

    mouse-fs write "hi"   # store 2 bytes
    mouse-fs read         # retrieve them
    mouse-fs reset        # restore 1000 DPI
    mouse-fs raw          # show raw DPI value

## How it works

A Logitech mouse communicates via the HID++ 2.0 protocol over a
Unifying receiver.

By enumerating the device's feature table, we can locate the AdjustableDPI feature (`0x2201`).
It accepts arbitrary `u16` values without validation, which allows encoding 2 bytes of data into the DPI value.

However:

- Writing DPI only affects the active DPI
- The device also maintains a fallback/default DPI
- On power cycles, the fallback value is restored

So while you can store arbitrary data in the DPI register, it does not persist across power cycles.

Tested on macOS. Long HID++ reports are blocked by IOHIDManager so storage
is limited to one u16 (2 bytes) via short reports only.

## Requirements

- Logitech mouse connected via Unifying receiver
- Logi Options+ must not be running (it holds the HID++ interface)
- `brew install hidapi`

## Would be cool

- ~~Resolve HID++ feature index automatically so you can use this on every Logitech mouse? Idk about this~~
