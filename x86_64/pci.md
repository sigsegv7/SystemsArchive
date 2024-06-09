## PCI signals

```
CLK: PCI Bus clock
FRAME#: Pulled low to indicate start of bus cycle
AD[31:0]: Address/Data
IRDY#: Initiator ready
TRDY#: Target ready
C/BE#[3:0]: Command (address phase), byte enables (data phases)
DEVSEL#: Address recognized
RST#: Reset
... TODO
```

## PCI read cycles

PCI typically supports a 33 MHz clock signal but can optionally run at 66 MHz starting with PCI 2.1.
During a read cycle, the ``FRAME#`` signal is pulled low to indicate the start of the transaction.
The address and data are multiplexed onto the ``AD`` bus. After the address phase, ``IRDY#`` is pulled
low to indicate that the initiator is ready to transition to the data phase. Once ``TRDY#`` and ``DEVSEL#``
are pulled low, data is transmitted with each rising edge of the ``CLK`` signal until the end of the transaction
is reached, which is indicated with ``FRAME#`` being pulled high.
