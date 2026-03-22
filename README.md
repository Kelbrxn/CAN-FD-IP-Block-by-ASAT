zynq_can_fd_ip/
└── rtl/
    ├── src/
    │   ├── top/
    │   │   └── can_fd_ip_top.sv            # Top-level wrapper (AXI <-> PHY)
    │   │
    │   ├── axi_interface/
    │   │   ├── axi_slave_interface.sv      # Main AXI Handshake FSM //check
    │   │   ├── address_decoder.sv          # Generates internal Chip Selects (Updated for Mailbox/ECC offsets) //check
    │   │   ├── register_bank.sv            # Config & Status registers (SRR, MSR, BTR, SR, ECR) //check
    │   │   ├── interrupt_manager.sv        # Aggregates IRQ signals (ISR, IER, ICR) //check
    │   │   └── acceptance_filter_bank.sv   # **NEW:** Holds memory-mapped ID/Mask registers (0x60, 0xE0)
    │   │
    │   ├── buffers/
    │   │   ├── tx_mailbox_manager.sv       # **NEW/REPLACED:** Manages TX Message Space (0x100) & Ready Request (0x90) //check
    │   │   ├── rx_buffer_manager.sv        # **NEW/REPLACED:** Manages RX Message Space (0x1100) & FIFO Status (FSR) //check
    │   │   ├── ecc_controller.sv           # **NEW:** 1-bit/2-bit Error Correction & Parity logic (0xC8-0xD4) 
    │   │   └── ram_wrapper.sv              # Standard Dual-Port BRAM primitive (used by TX/RX managers) //check
    │   │
    │   ├── can_core/
    │   │   ├── top/
    │   │   │   └── can_core_top.sv         # Protocol Engine Wrapper
    │   │   │
    │   │   ├── timing/
    │   │   │   ├── bit_timing_logic.sv     # Prescaler & Time Quanta generation
    │   │   │   ├── sync_logic.sv           # Hard Sync & Resync (SJW)
    │   │   │   ├── tdc_measure.sv          # **NEW:** Transceiver Delay Compensation (calculates TDCV feedback)
    │   │   │   └── timestamp_generator.sv  # **NEW:** Global timer for RX message timestamping
    │   │   │
    │   │   ├── stream/
    │   │   │   ├── stream_processor_top.sv # Serializer/Deserializer
    │   │   │   ├── bit_stuffer.sv          # Logic for dynamic & fixed stuffing
    │   │   │   └── bit_destuffer.sv        # Logic to remove stuff bits
    │   │   │
    │   │   ├── crc/
    │   │   │   ├── crc_top.sv              # Polynomial MUX /check
    │   │   │   ├── crc15_lfsr.sv           # Classic CAN CRC //check
    │   │   │   ├── crc17_lfsr.sv           # FD Short CRC //check
    │   │   │   ├── crc21_lfsr.sv           # FD Long CRC //check
    │   │   │   └── stuff_count_logic.sv    # 3-bit Stuff Count + Parity
    │   │   │
    │   │   └── control/
    │   │       ├── protocol_fsm.sv         # Main ISO 11898-1 State Machine
    │   │       ├── error_management.sv     # TEC/REC counters & Bus Off logic (feeds to ECR in AXI)
    │   │       └── acceptance_filter.sv    # Hardware comparator (checks bus stream against the filter bank)
    │   │
    │   └── phy_interface/
    │       └── phy_adapter.sv              # Loopback MUX & IO Pad drivers (TX/RX)
    │
    └── includes/
        ├── can_defines.vh                  # Constants (FDF, DLC, Intermission)
        └── register_offsets.vh             # Memory Map (Will hold all 0x00 to 0x2100 Xilinx offsets)
