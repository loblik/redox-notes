# Flashing NRF51 with Olimex and OpenOCD

With Olimex FTDI JTAG the target must be powered. For transmitter just run it from battery. The receiver can be also normally powerd from Arduino micro during flashing.

Run openocd server.
```
openocd -f interface/ftdi/olimex-arm-usb-ocd-h.cfg -f interface/ftdi/olimex-arm-jtag-swd.cfg  -c "set WORKAREASIZE 0"   -f target/nrf51.cfg
```

Paste flashing function to your shell.
```
flash_nordic() {
    (   
        echo "reset halt";
        sleep 0.1;
        echo "nrf51 mass_erase";
        sleep 1;
        echo "flash write_image erase" $(readlink -f $1);
        sleep 11
        echo "reset";
        exit;
    ) | telnet 127.0.0.1 4444
}
```

Flash it.
```
flash_nordic ./custom/armgcc/_build/nrf51822_xxac-keyboard-right.hex
```
