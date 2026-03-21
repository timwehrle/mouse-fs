# mouse-fs

Stores 2 bytes of arbitrary data in a Logitech MX Vertical's DPI register.

The mouse persists the DPI value in flash across power cycles and reconnections,
making it 2 bytes of cross-computer storage that lives in your mouse.

## Usage

    mouse-fs write "hi"   # store 2 bytes
    mouse-fs read         # retrieve them
    mouse-fs reset        # restore 1000 DPI
    mouse-fs raw          # show raw DPI value

## How it works

The Logitech MX Vertical communicates via the HID++ 2.0 protocol over a
Unifying receiver. By enumerating the device's feature table we found that
the AdjustableDPI feature (0x2201) accepts arbitrary u16 values with no
quantization, and persists them in the mouse's onboard flash.

Tested on macOS. Long HID++ reports are blocked by IOHIDManager so storage
is limited to one u16 (2 bytes) via short reports only.

## Requirements

- Logitech MX Vertical connected via Unifying receiver
- Logi Options+ must not be running (it holds the HID++ interface)
- `brew install hidapi`

## Would be cool

- Resolve HID++ feature index automatically so you can use this on every Logitech mouse? Idk about this
