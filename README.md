# CAN-FD-IP-Block-by-ASAT

zynq_can_fd_ip/
└── rtl/
    ├── src/
    │   ├── top/
    │   │   └── can_fd_ip_top.sv            # Top-level wrapper (AXI <-> PHY)
    │   │
    │   ├── axi_interface/
    │   │   ├── axi_slave_interface.sv      # Main AXI Handshake FSM //check
    │   │   ├── address_decoder.sv          # Generates internal Chip Selects //check
    │   │   ├── register_bank.sv            # Config registers (SRR, MSR, BTR) //check
    │   │   └── interrupt_manager.sv        # Aggregates IRQ signals 
    │   │
    │   ├── buffers/
    │   │   ├── fifo_controller.sv          # Read/Write pointer logic //check
    │   │   └── ram_wrapper.sv              # Standard BRAM primitive //check
    │   │
    │   ├── can_core/
    │   │   ├── top/
    │   │   │   └── can_core_top.sv         # Protocol Engine Wrapper
    │   │   │
    │   │   ├── timing/
    │   │   │   ├── bit_timing_logic.sv     # Prescaler & Time Quanta generation
    │   │   │   ├── sync_logic.sv           # Hard Sync & Resync (SJW)
    │   │   │   └── tdc_logic.sv            # Transceiver Delay Compensation (FD)
    │   │   │
    │   │   ├── stream/
    │   │   │   ├── stream_processor_top.sv # Serializer/Deserializer
    │   │   │   ├── bit_stuffer.sv          # Logic for dynamic & fixed stuffing
    │   │   │   └── bit_destuffer.sv        # Logic to remove stuff bits
    │   │   │
    │   │   ├── crc/
    │   │   │   ├── crc_top.sv              # Polynomial MUX
    │   │   │   ├── crc15_lfsr.sv           # Classic CAN CRC
    │   │   │   ├── crc17_lfsr.sv           # FD Short CRC
    │   │   │   ├── crc21_lfsr.sv           # FD Long CRC
    │   │   │   └── stuff_count_logic.sv    # 3-bit Stuff Count + Parity
    │   │   │
    │   │   └── control/
    │   │       ├── protocol_fsm.sv         # Main ISO 11898-1 State Machine
    │   │       ├── error_management.sv     # TEC/REC counters & Bus Off logic
    │   │       └── acceptance_filter.sv    # ID Mask/Code comparison
    │   │
    │   └── phy_interface/
    │       └── phy_adapter.sv              # Loopback MUX & IO Pad drivers
    │
    └── includes/
        ├── can_defines.vh                  # Constants (FDF, DLC, Intermission)
        └── register_offsets.vh             # Memory Map (0x00, 0x04...)
