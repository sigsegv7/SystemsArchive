# I/O APIC

APIC (Advanced Programmable Interrupt Controller) is a collection of chips on modern machines that allow interrupt routing.
The I/O APIC is a chip within the chipset (or on more modern machines, integrated into the CPU silicon) that manages out-of-band
interrupt signalling. It allows devices to assert specific interrupt lines (typically edge-level triggered) assigned to them in order
to signal an event. These assertions of the interrupt lines are known as IRQs. The I/O APIC checks a table known as the redirection table
in order to determine where and how to send the IRQ as a system interrupt message. Each entry of this table is associated with an interrupt
pin (in software it can be known as a Global System Interrupt or GSI) on the I/O APIC.

## I/O APIC redirection table entry in C

```c
union ioapic_redentry {
    struct {
        uint8_t vector;
        uint8_t delmod          : 3;
        uint8_t destmod         : 1;
        uint8_t delivs          : 1;
        uint8_t intpol          : 1;
        uint8_t remote_irr      : 1;
        uint8_t trigger_mode    : 1;
        uint8_t interrupt_mask  : 1;
        uint64_t reserved       : 39;
        uint8_t dest_field;
    };
    uint64_t value;
};
```

The ``vector`` field specifies the target interrupt vector that this interrupt will be serviced by.

The ``delmod`` is the delivery mode. This can be:

Fixed: 000b, Lowest priority: 001b, SMI: 010b, NMI: 100b, INIT: 101b and ExtINT: 111b

The ``destmod`` field specifies the destination mode. This can be Physical: 0, Logical: 1

The ``delivs`` field is set to 1 if the interrupt has been set to the Local APIC but still pending.

The ``intpol`` field specifies the interrupt polarity (0: Active high, 1: Active low).

The ``remote_irr`` field is used for level triggered interrupts,  1 when local APIC(s) accept the level interrupt sent.

The ``trigger_mode`` field specifies the trigger mode (0: Edge, 1: Level).

The ``interrupt_mask`` field is set to 1 to mask the interrupt (cause it to be ignored).

The ``dest_field`` field specifies the Logical/Physical ID of the target Local APIC(s)
